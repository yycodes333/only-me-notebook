```
学习资源
		https://zhuanlan.zhihu.com/p/1972655407605520311
```



# Claude CLI + Codex MCP 使用手册

## 当前结论

你现在已经安装成功，不需要再改配置。

当前 Claude 里的 Codex MCP 状态是：

```text
Status: √ Connected
Command: C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe
Args: -m gpt-5-codex -c model_reasoning_effort=high mcp-server
```

这表示 Claude Code 已经可以通过 MCP 调用 Codex。

## 这条命令是什么意思

```powershell
codex.exe -m gpt-5-codex -c model_reasoning_effort=high mcp-server
```

含义：

```text
codex.exe
启动 Codex CLI

-m gpt-5-codex
让 Codex 使用 gpt-5-codex 模型

-c model_reasoning_effort=high
把推理强度设为 high，适合复杂 bug、代码审查、疑难定位

mcp-server
把 Codex 启动成一个 MCP server，供 Claude Code 调用
```

注意：`mcp-server` 很关键。

`codex mcp` 和 `codex mcp-server` 不是一回事：

```text
codex mcp
管理 Codex 自己可以使用的外部 MCP 服务

codex mcp-server
把 Codex 本身变成 MCP 服务，给 Claude Code 调用
```

所以 Claude 里必须用 `mcp-server`，不能用 `mcp`。

## 为什么你的配置用了绝对路径

你的机器上裸 `codex` 或 `codex.exe` 可能会解析到 WindowsApps 里的 Codex，可执行文件会出现拒绝访问。

所以当前使用了更稳定的绝对路径：

```powershell
C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe
```

PowerShell 手动运行绝对路径时，前面要加 `&`：

```powershell
& "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" --version
```

## Claude + Codex 的分工

推荐理解：

```text
Claude Code = 主力开发助手
Codex MCP = 外接代码专家
```

Claude Code 适合：

```text
日常开发
理解项目
多轮对话
改文件
组织任务
推进实现
```

Codex 适合：

```text
复杂 bug 分析
代码细节复核
第二视角审查
定位隐藏问题
对修复方案做技术判断
```

推荐工作流：

```text
1. 平时直接用 Claude Code 做主力开发
2. 遇到复杂 bug 或不确定方案
3. 让 Claude Code 调用 codex MCP
4. Codex 返回独立分析
5. Claude Code 综合结论并继续修改代码
```

## 安装命令

当前推荐安装命令：

```powershell
claude mcp add codex -s user -- "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" -m gpt-5-codex -c model_reasoning_effort=high mcp-server
```

参数说明：

```text
claude mcp add codex
给 Claude 添加一个名为 codex 的 MCP 服务

-s user
安装到用户级配置，所有项目都能用

--
后面的内容是 MCP 服务真正要执行的命令

"...\codex.exe" ... mcp-server
启动 Codex MCP server
```

## 验证是否安装成功

命令行验证：

```powershell
claude mcp list
```

成功时应看到：

```text
codex: ... mcp-server - √ Connected
```

查看详情：

```powershell
claude mcp get codex
```

成功时应看到：

```text
Status: √ Connected
Type: stdio
Args: -m gpt-5-codex -c model_reasoning_effort=high mcp-server
```

Claude Code 里验证：

```text
/mcp
```

成功时应看到：

```text
codex · connected · 2 tools
```

## 第一次怎么试用

在 Claude Code 里输入：

```text
请调用 codex MCP，作为第二审查者，分析当前项目结构和潜在问题。
只做只读检查，不要修改文件。
最后请分别给出 Claude 自己的判断和 Codex 的判断。
```

更贴近实际 bug 场景：

```text
这个 bug 我怀疑比较深。请你先自己分析一遍，
然后调用 codex MCP 让 Codex 独立复核根因，
最后综合两边结论给我一个修复方案。
```

代码审查场景：

```text
请先审查当前改动，然后调用 codex MCP 做第二轮代码审查。
重点看隐藏 bug、边界条件、并发问题、数据一致性和测试缺口。
```

修复方案复核：

```text
我准备这样修复：<粘贴方案>。
请调用 codex MCP 评估这个方案有没有风险，并给出更稳妥的修改顺序。
```

## 常用 Claude MCP 命令

列出 MCP 服务：

```powershell
claude mcp list
```

查看某个 MCP：

```powershell
claude mcp get codex
```

添加 Codex MCP：

```powershell
claude mcp add codex -s user -- "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" -m gpt-5-codex -c model_reasoning_effort=high mcp-server
```

删除 Codex MCP：

```powershell
claude mcp remove codex -s user
```

在 Claude Code 交互界面查看 MCP：

```text
/mcp
```

## 常用 Codex MCP 命令

注意：这些命令是管理 Codex 自己可用的 MCP 服务，不是给 Claude 添加 Codex。

查看 Codex 里的 MCP：

```powershell
& "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" mcp list
```

查看某个 Codex MCP：

```powershell
& "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" mcp get chrome-devtools
```

添加 Codex 外部 MCP：

```powershell
& "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" mcp add-json chrome-devtools '{"command":"npx","args":["chrome-devtools-mcp@latest"]}'
```

删除 Codex 外部 MCP：

```powershell
& "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" mcp remove chrome-devtools
```

## 你截图里的两个 MCP 列表怎么理解

Claude Code 的 `/mcp`：

```text
codex · connected · 2 tools
```

意思是 Claude 能调用 Codex。

Codex App 的 MCP 面板：

```text
chrome-devtools
codex_apps
node_repl
```

意思是 Codex 自己能使用这些 MCP 或工具。

这两个列表不是同一个东西：

```text
Claude 的 MCP 列表：Claude 能调用谁
Codex 的 MCP 列表：Codex 自己能调用谁
```

## 什么时候该调用 Codex MCP

适合调用：

```text
复杂 bug
大项目里找根因
Claude 分析几轮仍不确定
需要第二模型独立复核
担心修改方案有隐藏风险
需要更严格的代码审查
```

不必调用：

```text
简单重命名
小改文案
普通文件移动
明确的一两行修改
不需要深度分析的日常任务
```

## 推荐日常提示词

让 Claude 自动决定是否调用：

```text
你是主执行者。遇到复杂 bug、架构风险、隐藏边界条件或你不确定的地方，
请调用 codex MCP 做第二视角分析。简单任务不要调用。
```

强制调用 Codex：

```text
请调用 codex MCP 独立分析这个问题，不要只用你自己的判断。
```

只读复核：

```text
请调用 codex MCP 做只读复核，不要修改文件。
```

实现后复查：

```text
我已经完成修改。请调用 codex MCP 复查这次改动是否有 bug、遗漏测试或回归风险。
```

## 出问题时怎么排查

如果 Claude Code 里 `/mcp` 不显示 codex：

```powershell
claude mcp list
claude mcp get codex
```

如果显示 `Failed to connect`：

1. 确认使用的是绝对路径，不要用裸 `codex`
2. 确认参数最后是 `mcp-server`，不是 `mcp`
3. 确认 Codex CLI 能运行：

```powershell
& "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" --version
```

4. 重新添加：

```powershell
claude mcp remove codex -s user
claude mcp add codex -s user -- "C:\Users\20331\AppData\Local\OpenAI\Codex\bin\f1c7ee7a13db5fed\codex.exe" -m gpt-5-codex -c model_reasoning_effort=high mcp-server
```

## 最短使用顺序

```text
1. 打开项目目录
2. 启动 Claude Code
3. 输入 /mcp，确认 codex connected
4. 平时正常让 Claude 做任务
5. 遇到复杂问题时说：请调用 codex MCP 复核
6. Claude 调用 Codex
7. Claude 汇总结论并继续执行
```

一句话总结：

```text
Claude Code 负责推进任务，Codex MCP 负责在困难问题上做深度复核。
```
