# Claude CLI + Codex MCP Demo 示例

## 1. 这件事到底做了什么

你做的事情是：

```text
把 Codex 启动成一个 MCP server，
再把这个 MCP server 注册给 Claude Code。
```

也就是说：

```text
Claude Code 是 MCP client
Codex 是 MCP server
Claude 可以把 Codex 当成一个工具来调用
```

当前有效命令是：

```powershell
claude mcp add codex -s user -- "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" -m gpt-5-codex -c model_reasoning_effort=high mcp-server
```

其中最关键的是最后的：

```text
mcp-server
```

它的意思是：把 Codex 作为 MCP 服务启动，供 Claude 调用。

不要写成：

```text
mcp
```

因为 `codex mcp` 是管理 Codex 自己的 MCP 配置，不是把 Codex 暴露给 Claude。

## 2. 它可以实现什么

这个 MCP 可以实现：

```text
1. Claude 在同一个会话里调用 Codex 做第二视角分析
2. Claude 把复杂 bug、代码审查、方案复核交给 Codex
3. Codex 返回分析结果后，Claude 继续执行、修改或总结
4. 你不用手动复制粘贴两边对话
```

适合场景：

```text
复杂 bug 定位
大项目根因分析
代码审查
边界条件检查
方案风险评估
修改后复核
部署前 checklist
```

不太适合场景：

```text
简单重命名
普通文案修改
明确的一两行代码修改
不需要第二视角的小任务
```

## 3. 它是不是能读取内容

可以，但要理解边界。

Claude 能看到当前项目，是因为 Claude Code 本身在你的项目目录里运行。

Codex MCP 能看到什么，取决于：

```text
1. Claude 调用 Codex 时传了什么上下文
2. Codex MCP 运行时所在的工作目录
3. Codex 是否被要求读取文件
4. 本地权限是否允许它读取或修改
```

它不是自动读取你电脑所有内容。

更准确地说：

```text
Claude 可以让 Codex 去检查当前项目中的相关文件；
Codex 返回分析结果；
Claude 再决定下一步。
```

建议第一次使用时明确限制：

```text
请调用 codex MCP 做只读分析，不要修改文件。
```

## 4. 两个 AI 会自动对话吗

不会自动无限对话。

真实流程更像这样：

```text
你 -> Claude
Claude -> 调用 Codex MCP
Codex -> 返回结果
Claude -> 综合回答你
```

如果你要求多轮复核，Claude 可以多次调用 Codex：

```text
Claude 分析
调用 Codex 复核
Claude 根据 Codex 结果修正
再次调用 Codex 检查
Claude 给最终结论
```

但这不是两个 AI 在后台自己聊天，而是 Claude 作为主控方，按需调用 Codex。

## 5. 新开一个文件夹要做什么

你现在用的是：

```text
-s user
```

意思是用户级配置，所有项目都能用。

所以新开一个文件夹时，一般不需要重新安装。

流程：

```powershell
cd 新项目目录
claude
```

进入 Claude Code 后输入：

```text
/mcp
```

看到：

```text
codex · connected · 2 tools
```

就说明这个新文件夹也能用 Codex MCP。

## 6. 如果新文件夹里不想用怎么办

最简单：

```text
不要让 Claude 调用 codex MCP。
```

你可以直接对 Claude 说：

```text
这个项目不要调用 codex MCP，只使用 Claude 自己分析和执行。
```

如果你想彻底移除用户级 Codex MCP：

```powershell
claude mcp remove codex -s user
```

这样所有项目都不能再用这个用户级 Codex MCP。

如果你只想某个项目用 Codex，其他项目不用，推荐改成项目/本地级安装：

```powershell
claude mcp remove codex -s user
cd 需要使用 Codex 的项目目录
claude mcp add codex -s local -- "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" -m gpt-5-codex -c model_reasoning_effort=high mcp-server
```

`-s local` 表示只在当前本地项目配置中使用。

## 7. 是否可以用话语让 Claude 自动完成安装/删除

可以。Claude Code 可以运行 shell 命令，所以你可以直接用自然语言要求它检查、安装、删除。

但注意：

```text
1. Claude 可能会请求你确认执行命令
2. 安装/删除 MCP 是配置变更，建议让它先检查再执行
3. 如果命令失败，要让它读取错误并修复命令
```

推荐输入：

```text
请检查当前是否已经安装 codex MCP。
如果没有，请使用用户级配置安装。
如果安装了但无法连接，请修复为 codex.exe 绝对路径，并确保最后参数是 mcp-server。
完成后用 claude mcp list 和 claude mcp get codex 验证。
```

删除时输入：

```text
请从 Claude Code 用户级配置里删除 codex MCP。
执行后用 claude mcp list 验证它已经不存在。
```

## 8. Demo 1：检查是否已连接

### 你的输入

```text
请检查 codex MCP 是否已经连接成功。
只检查，不要修改配置。
```

### Claude 可能执行

```powershell
claude mcp list
claude mcp get codex
```

### 样例输出

```text
codex MCP 已连接成功。

状态：
codex ... mcp-server - √ Connected

详情：
Scope: User config
Type: stdio
Args: -m gpt-5-codex -c model_reasoning_effort=high mcp-server
```

## 9. Demo 2：让 Claude 调用 Codex 做只读复核

### 你的输入

```text
请先自己分析当前项目结构，
然后调用 codex MCP 做第二视角复核。
只读检查，不要修改文件。
最后分别列出 Claude 的判断、Codex 的判断、综合结论。
```

### 样例输出

```text
我先完成了 Claude 侧的项目结构分析：

Claude 判断：
1. 项目入口在 src/main.ts
2. 配置读取集中在 src/config.ts
3. 当前风险点是错误处理分散，测试覆盖不足

随后我调用了 codex MCP 做只读复核。

Codex 判断：
1. src/config.ts 对缺失环境变量处理不完整
2. src/api/client.ts 的超时逻辑可能导致请求悬挂
3. tests 里缺少失败路径覆盖

综合结论：
优先处理配置校验和 API 超时，再补失败路径测试。
我建议下一步先改 src/config.ts，并添加对应单元测试。
```

## 10. Demo 3：复杂 bug 场景

### 你的输入

```text
这个登录 bug 查了几轮还不稳定。
请你先自己分析一遍，再调用 codex MCP 独立判断根因。
不要急着改代码，先给我根因排序和验证步骤。
```

### 样例输出

```text
Claude 初步判断：
1. token refresh 可能存在竞态
2. 登录态缓存与接口返回状态不一致
3. 前端路由守卫可能提前跳转

Codex MCP 复核结果：
1. 最大嫌疑是 token refresh 并发请求互相覆盖
2. 第二嫌疑是 localStorage 写入后未同步内存状态
3. 路由守卫更像是结果，不是根因

综合排序：
P1 token refresh 并发覆盖
P2 登录态双来源不一致
P3 路由守卫触发时机

建议验证：
1. 在 refresh 入口添加日志，观察是否同一时间发起多个 refresh
2. 模拟 refresh 慢响应
3. 检查旧 token 是否覆盖新 token
```

## 11. Demo 4：安装 Codex MCP

### 你的输入

```text
请帮我安装 codex MCP 到 Claude Code 的用户级配置。
要求使用 gpt-5-codex，reasoning effort 为 high。
安装后请验证连接状态。
```

### Claude 应执行

```powershell
claude mcp add codex -s user -- "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" -m gpt-5-codex -c model_reasoning_effort=high mcp-server
claude mcp list
claude mcp get codex
```

### 样例输出

```text
Codex MCP 已安装到用户级配置。

验证结果：
codex ... mcp-server - √ Connected

现在所有 Claude Code 项目都可以通过 /mcp 看到 codex。
```

## 12. Demo 5：删除 Codex MCP

### 你的输入

```text
请从 Claude Code 用户级配置中删除 codex MCP。
删除后验证它已经不再显示。
```

### Claude 应执行

```powershell
claude mcp remove codex -s user
claude mcp list
```

### 样例输出

```text
已删除用户级 codex MCP。

当前 claude mcp list 中不再显示 codex。
如果之后还想使用，需要重新添加。
```

## 13. MCP 联动和手动双 AI 协作有什么区别

### 手动双 AI 协作

你手动操作：

```text
1. 问 Codex 制定计划
2. 复制给 Claude 审核
3. 再复制 Claude 的意见给 Codex
4. 让 Codex 执行
5. 再复制结果给 Claude 验收
```

优点：

```text
控制感强
每一步你都能决定发什么
适合非常谨慎的任务
```

缺点：

```text
复制粘贴多
上下文容易漏
两个会话状态不同步
效率低
很容易忘记把错误日志、diff、文件路径传完整
```

### MCP 联动

Claude 作为主控：

```text
1. 你只和 Claude 对话
2. Claude 判断何时调用 Codex
3. Codex 返回结果给 Claude
4. Claude 综合结果继续推进
```

优点：

```text
不用复制粘贴
结果回到同一个 Claude 会话
Claude 可以把 Codex 结论接到后续执行里
更适合“主力 + 专家顾问”模式
```

缺点：

```text
Codex 不是独立常驻项目经理
Claude 不一定每次都自动调用，除非你明确要求
Codex 看到的上下文取决于 Claude 传了什么和它读取了什么
复杂任务仍然需要你设定边界，比如只读、不要部署、先给计划
```

核心区别：

```text
手动双 AI = 你做人肉中转站
MCP 联动 = Claude 把 Codex 当工具调用
```

## 14. 推荐实际用法

最稳妥的模式：

```text
Claude 负责执行
Codex 负责复核
```

例如：

```text
请你作为主执行者完成这个任务。
遇到复杂 bug、架构风险或你不确定的地方，请调用 codex MCP 做第二视角复核。
Codex 默认只做分析和建议，不直接修改文件。
最终修改由你执行，执行后再调用 Codex 做一次只读复查。
```

部署任务建议：

```text
1. Claude 制定部署步骤
2. Codex MCP 复核风险
3. Claude 执行部署命令
4. Codex MCP 复核日志和结果
5. Claude 给最终验收结论
```

不要一开始就让两个模型都随便改代码。这样容易出现责任边界不清。

## 15. 最推荐的万能提示词

```text
你是主执行者，Codex MCP 是第二审查者。

工作要求：
1. 简单任务不要调用 Codex
2. 遇到复杂 bug、架构风险、隐藏边界条件、部署风险时调用 Codex MCP
3. Codex 默认只读分析，不直接修改文件
4. 你需要综合 Claude 自己的判断和 Codex 的判断
5. 修改代码后，如果风险较高，再调用 Codex MCP 做最终复核
6. 最后告诉我：Claude 判断、Codex 判断、最终决定、执行结果
```

## 16. 最短记忆版

```text
Claude = 主控和执行
Codex MCP = 外接专家和复核
MCP = 让 Claude 在同一会话里调用 Codex
不是两个 AI 自动聊天
不是自动读取所有内容
新项目默认可用，因为你装的是 user scope
不用时可以口头禁止，彻底不用就 remove
```
