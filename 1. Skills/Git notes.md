# Git 使用教程

```
学习资源：
		https://hackmd.io/@GDSC-CYCU/Linux-Git-WorkShop
		https://liaoxuefeng.com/books/git/branch/collaboration/index.html
```



## 简介

Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。

Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

## Git安装

这里仅介绍在Linux上安装。首先验证自己是否安装过git。那么输入git即可：

```shell
git
```

若输出以下内容则说明没有安装.

```shell
The program 'git' is currently not installed. You can install it by typing:
sudo apt-get install git
```

Ubuntu Git 安装最新稳定版本命令为：

```shell
sudo apt update
sudo apt install git
```

补充;若已经安装，只升级git,可执行下面命令

```shell
sudo apt install git --only-upgrade
```

接着我们进行验证是否安装成功，若成功输出对应版本，则说明安装成功！

```shell
git --version
git version 2.34.1
```

## Git配置

### 1.全局配置

全局配置，最明显的关键词是**global**，指的是在本台电脑上，当前用户的所有Git仓库，默认提交人是全局配置的用户，若有局部配置，后续会介绍，优先级大于全局配置。

接下来我们进行全局配置，在命令行输入：

```shell
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

配置完成后，输入以下命令检查全局配置信息

```shell
git config --global user.name
git config --global user.email
```

或者查看所有的全局配置

```shell
git config --global --list
```

### 2. 局部配置

如果你的需求是对某个仓库指定特定的用户名和Email地址，你可以采取局部配置。

加入你现在有了一个仓库`A`，进入仓库A，然后执行下列命令

```shell
git config user.name "你的名字"
git config user.email "你的邮箱"
```

并且同样，配置完进行查看配置信息

```shell
git config user.name
git config user.email
```

或者查看所有的局部配置信息

```shell
git config --list
```

## Git 实现笔记记录

### 1. 创建版本库与工作区

工作区（Working Directory）指的就是你在电脑里能看到的目录，比如我的`notebook`文件夹就是一个工作区：

![git6](image/git6.png)

什么是版本库呢？版本库又名仓库（Repository），你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

版本库（Repository）工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![git7](image/git7.png)

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

那么我们如何创建版本库呢？这也就是接下来我们要讲到的 **git init**这个命令工具了。

首先，进入的哦阿你想管理的仓库中，这里以我的`notebook`仓库为例。

![git1](image/git1.png)

进入`notebook`终端，然后输入

```shell
git init
```

验证该仓库是否变成版本库

```shell
ls -a
```

若输出 `.git` ，则证明你创建版本库成功！

**说明**：`. git`隐藏文件夹非常重要，它保存了Git的版本记录、分支信息、配置内容等，这也是后边我们逐次讲解会涉及到的。

**`git init` 使用一次**：`git init`什么时候使用呢？只要你的仓库想要实现版本库，在这个仓库下必须就执行一次`git init`即可，并且通过`ls -a`可以查看到这个隐藏文件即可。

### 2. Git 常用命令介绍

#### 1. 查看修改状态及内容

`git status`命令可以让我们时刻掌握仓库当前的状态，但不能看具体修改了的内容，而`git diff`这个命令可以帮我们查看具体修改的内容。

**注意**：在现实内容信息时候如果你的文件或者文件夹是中文会出现乱码问题，所以建议使用`Enhlish`对你的内容命名。

目前我们仓库是已经上传完的，输入`git status`,显示如下。

```shell
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean

```

接着，我们展示如果作出修改这两个命令的作用。

##### 新增内容体现

如果原来我的README.md内容如下：

```
机械臂智能抓取是机器人学、机器视觉、深度学习与工程系统集成的典型交叉方向。对于学习者而言，真正的难点并不只是理解单个概念，而是如何将相机采集、图像处理、目标识别、位姿估计、坐标变换、运动规划与机械执行串联为一个完整闭环。

本规划书结合学习者已有基础，围绕“编程基础 → 机器人学核心 → 机器视觉与相机系统 → ROS2 与 MoveIt2 工程落地 → 深度学习进阶 → 3D 点云与抓取网络 → 综合项目实施”构建系统路线。每个章节均给出学习内容、学习目标、学习价值、学习安排与资源建议，避免仅停留在概念堆叠或代码搬运层面。

本版本特别强调内容完整性。此前版本中部分学习模块存在表达压缩现象，例如 NumPy 学习条目中“向量化运算”“图像矩阵与张量基础”等内容曾被弱化。本最终版已统一恢复为完整表达，确保每一模块都可直接执行和验收。
```

接着作出修改如下：

```
机械臂智能抓取是机器人学、机器视觉、深度学习与工程系统集成的典型交叉方向。对于学习者而言，真正的难点并不只是理解单个概念，而是如何将相机采集、图像处理、目标识别、位姿估计、坐标变换、运动规划与机械执行串联为一个完整闭环。

本规划书结合学习者已有基础，围绕“编程基础 → 机器人学核心 → 机器视觉与相机系统 → ROS2 与 MoveIt2 工程落地 → 深度学习进阶 → 3D 点云与抓取网络 → 综合项目实施”构建系统路线。每个章节均给出学习内容、学习目标、学习价值、学习安排与资源建议，避免仅停留在概念堆叠或代码搬运层面。

本版本特别强调内容完整性。此前版本中部分学习模块存在表达压缩现象，例如 NumPy 学习条目中“向量化运算”“图像矩阵与张量基础”等内容曾被弱化。本最终版已统一恢复为完整表达，确保每一模块都可直接执行和验收。

这里新增内容1
```



执行 `git status`，只能看到修改的文本，但无法看到修改的内容。

```shell
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md      #这里是重点，修改的内容对象是READER.md

no changes added to commit (use "git add" and/or "git commit -a")
```

执行 `git diff`

```shell
+这里新增内容1   #最明显特征就是，加号后面是较原来add 存储区版本多的内容
```

即若我将这里的新增内容1删除点，我并没有进行add ，那么最终`git diff`,什么也不会输出。

然后执行 `git add .``接着执行git status`

```shell
yy@yy-VivoBook-ASUSLaptop-X512FLC-V5000FL:~/My_Data/notebook$ git add .
yy@yy-VivoBook-ASUSLaptop-X512FLC-V5000FL:~/My_Data/notebook$ git status 
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   README.md#这里告诉我们，将要被提交的修改包括`README.md`，下一步，就可以放心地提交了

yy@yy-VivoBook-ASUSLaptop-X512FLC
```



然后我们此时执行`git diff`会发现也是空的，这时我们就知道了，`git diff`比较的就是存储区（add）之后的状态和当前状态。



##### 删除内容体现

如果原来我的README.md内容如下：

```
机械臂智能抓取是机器人学、机器视觉、深度学习与工程系统集成的典型交叉方向。对于学习者而言，真正的难点并不只是理解单个概念，而是如何将相机采集、图像处理、目标识别、位姿估计、坐标变换、运动规划与机械执行串联为一个完整闭环。

本规划书结合学习者已有基础，围绕“编程基础 → 机器人学核心 → 机器视觉与相机系统 → ROS2 与 MoveIt2 工程落地 → 深度学习进阶 → 3D 点云与抓取网络 → 综合项目实施”构建系统路线。每个章节均给出学习内容、学习目标、学习价值、学习安排与资源建议，避免仅停留在概念堆叠或代码搬运层面。

本版本特别强调内容完整性。此前版本中部分学习模块存在表达压缩现象，例如 NumPy 学习条目中“向量化运算”“图像矩阵与张量基础”等内容曾被弱化。本最终版已统一恢复为完整表达，确保每一模块都可直接执行和验收。

这里新增内容1
```



然后我们删除`“这里新增内容1”`这个内容

```
机械臂智能抓取是机器人学、机器视觉、深度学习与工程系统集成的典型交叉方向。对于学习者而言，真正的难点并不只是理解单个概念，而是如何将相机采集、图像处理、目标识别、位姿估计、坐标变换、运动规划与机械执行串联为一个完整闭环。

本规划书结合学习者已有基础，围绕“编程基础 → 机器人学核心 → 机器视觉与相机系统 → ROS2 与 MoveIt2 工程落地 → 深度学习进阶 → 3D 点云与抓取网络 → 综合项目实施”构建系统路线。每个章节均给出学习内容、学习目标、学习价值、学习安排与资源建议，避免仅停留在概念堆叠或代码搬运层面。

本版本特别强调内容完整性。此前版本中部分学习模块存在表达压缩现象，例如 NumPy 学习条目中“向量化运算”“图像矩阵与张量基础”等内容曾被弱化。本最终版已统一恢复为完整表达，确保每一模块都可直接执行和验收。

这里新增内容1
```

执行`git diff`，输出如下内容：

```shell
-这里新增内容1   #最明显特征就是，减号后面是较原来add 存储区版本少的内容
```





##### 补充

**git diff内容查阅操作**

| `q`       | 退出               |
| --------- | ------------------ |
| `空格`    | 向下翻一页         |
| `b`       | 向上翻一页         |
| `回车`    | 向下滚动一行       |
| `↑` / `↓` | 上下移动           |
| `/关键词` | 搜索关键词         |
| `n`       | 跳到下一个搜索结果 |
| `N`       | 跳到上一个搜索结果 |

**git status 简介版命令**

当我们查阅状态时候发现，内容太多太乱，有很多我们压根儿就不想看的内容出现，我们就像看到是什么文件被修改了以及修改类型我们可以使用 `git status -s`，这个命令只会输出修改文件以及文件修改类型。其中`s`是`short`简称

例如，以下是 `git status`输出

```shell
yy@yy-VivoBook-ASUSLaptop-X512FLC-V5000FL:~/My_Data/notebook$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   Usage tips habit cultivation/Git notes.md
```

如果使用`git status -s`输出如下

```shell
 M "Usage tips habit cultivation/Git notes.md"
```

这就非常简介了，注意到构成结构是 **修改类型+文件**

修改类型如下

| 标记 | 含义                 |
| ---- | -------------------- |
| `??` | 新文件，未追踪       |
| `M`  | 文件被修改           |
| `A`  | 新文件，已加入暂存区 |
| `D`  | 文件被删除           |
| `R`  | 文件被重命名         |

注意到，在修改后没有放到暂存区时候即没有执行 `git add`，是红色的M，执行 `git add`后变成绿色的M

![git2](image/git2.png)

并且在这里也说明 `??`和 `A`关系如下，新添加内容，如果没有进入暂存区显示 `？？`，加入暂存区显示 `A`

![git4png](image/git4png.png)

如果我将 README.md(copy)删除后，如下显示，可见有顺序，先添加后删除，这里表示为为 `A D`。

![git3](image/git3.png)



#### 2. 查看历史记录

在实际工作中，我们很难单靠记忆回想一个文件每次都修改了什么内容。

因此，Git 提供了查看历史记录的能力。接下来我们就围绕 `git log` 的常见用法，分场景进行说明。

---

##### 2.1 `git log` 的基本作用

首先，我们需要明确：

`git log` 命令用于查看提交历史，并且默认是按照 **从最近到最远** 的顺序显示提交记录。

也就是说，它可以帮助我们快速了解：

- 最近提交了什么
- 是谁提交的
- 每次提交的说明是什么

---

##### 2.2 `git log` 的交互界面操作

接下来要注意的是，执行 `git log` 后，通常会进入一个可滚动查看的界面，这和 `git diff` 类似。

因此，在这个界面中，常用的按键操作如下：

| 按键      | 作用               |
| --------- | ------------------ |
| `q`       | 退出               |
| `空格`    | 向下翻一页         |
| `b`       | 向上翻一页         |
| `回车`    | 向下滚动一行       |
| `↑ / ↓`   | 上下移动           |
| `/关键词` | 搜索关键词         |
| `n`       | 跳到下一个搜索结果 |
| `N`       | 跳到上一个搜索结果 |

**说明：**  
当提交历史较多时，这些快捷键可以帮助我们更方便地浏览内容。

---

##### 2.3 `git log` 的常用参数

除了直接使用 `git log` 之外，我们还可以通过参数来控制输出格式。

常见参数如下：

| 参数         | 作用               |
| ------------ | ------------------ |
| `--oneline`  | 每个提交只显示一行 |
| `--graph`    | 显示分支图         |
| `--decorate` | 显示分支名、标签名 |
| `--all`      | 显示所有分支       |

**说明：**  
这些参数可以单独使用，也可以组合使用。  
在日常学习和查看历史时，最常用的是：

```bash
git log --oneline
```

它可以看作是 `git log` 的简洁版。

---

##### 2.4 查看当前仓库的提交历史

下面我们先从最基础的情况开始：查看当前仓库的历史记录。

###### 2.4.1 使用 `git log`

首先执行：

```bash
git log
```

它会显示每次提交的详细信息，例如：

```bash
commit d13d5c4
Author: yy <2033170479@qq.com>
Date:   Wed May 6 19:04:35 2026 +0800

    更新Git笔记
```

**解释：**  
这种输出方式会显示提交 ID、作者、日期以及提交说明，信息比较完整，但内容也相对较多。

---

###### 2.4.2 使用 `git log --oneline`

如果我们只想快速浏览提交记录，可以使用：

```bash
git log --oneline
```

示例输出如下：

```bash
d13d5c4 更新Git笔记
```

**说明：**  
这种格式会把一条提交压缩成一行，阅读起来更加直观，适合快速查看历史。

---

###### 2.4.3 `git log` 与 `git log --oneline` 的区别

为了更清楚地理解两者差异，可以这样记：

- `git log`：信息完整，适合看详细内容
- `git log --oneline`：信息简洁，适合快速浏览

**结论：**  
如果你只是想大概看看历史，优先使用 `git log --oneline`。

---

##### 2.5 查看某个文件的提交历史

除了查看整个仓库的历史，我们还可以只查看某一个文件的历史记录。

这时，命令格式如下：

```bash
git log --oneline <文件名>
```

例如：

```bash
git log --oneline Outline.md
```

示例输出如下：

```bash
d1765b1 解决中文乱码问题
```

**注意：**  
这里的 `--oneline` 一定要写在文件名前面，否则命令可能无法正常识别。

**说明：**  
这种方式适合我们只关注某一个文件的变化过程，而不关心整个仓库的全部提交。

---

##### 2.6 查看某个文件每次提交具体改了什么

如果我们不只是想知道“提交了什么”，还想知道“每次具体改了哪里”，那就可以使用 `-p` 参数。

命令如下：

```bash
git log --oneline -p <文件名>
```

例如：

```bash
git log --oneline -p README.md
```

输出中会出现类似这样的差异内容：

```bash
diff --git a/README.md b/README.md
index cd47a99..265a378 100644
--- a/README.md
+++ b/README.md
@@ -6,7 +6,5 @@

-我要在这里作出修改。
-
-
+这里作出修改
```

**解释：**

- `-` 表示被删除的内容
- `+` 表示新增的内容

也就是说，上面的例子表示原来的内容被删掉，并替换成了新的内容。

**提示：**  
如果你想精确追踪某个文件的修改过程，这个命令非常有用。

---

##### 2.7  查看最近几次提交

有时候我们并不需要查看全部历史，只想看最近几次提交。

这时可以使用：

```bash
git log --oneline -3
```

其中，`-3` 表示只显示最近 3 次提交。

示例输出如下：

```bash
3d3586a (HEAD -> main, origin/main) 更新Git笔记
d13d5c4 更新Git笔记
8af8375 更新部分Git教学
```

**说明：**  
如果你想看最近 5 次提交，可以改成：

```bash
git log --oneline -5
```

同理，如果要看最近 10 次，就写成：

```bash
git log --oneline -10
```

---

###### 2.7.1 查看某个文件最近几次提交

除了查看整个仓库的最近提交，我们也可以只看某个文件最近几次的记录。

命令如下：

```bash
git log --oneline -3 <文件名>
```

例如：

```bash
git log --oneline -3 README.md
```

示例输出如下：

```bash
ccbc4d0 测试
a279bf3 测试不用管
6438a58 解决中文乱码
```

**说明：**  
这种写法可以帮助我们快速聚焦到某个文件的修改历史，而不必翻看整个项目的提交记录。

---

##### 2.8 查看所有分支的历史

前面我们看到的都是当前分支的历史。

如果我们希望查看所有分支上的提交记录，就可以使用：

```bash
git log --all
```

如果再结合 `--oneline`，会更清晰：

```bash
git log --all --oneline
```

示例输出如下：

![git5](image/git5.png)

**解释：**  
这个命令会把仓库中所有分支的提交都列出来，所以你会看到比普通 `git log` 更全面的内容。

**补充：**  
如果仓库有多个分支，而你又想了解它们的整体演进过程，那么这个命令非常适合。

---

##### 2.9 本节小结

最后，我们把本节内容做一个简要总结：

| 需求                      | 命令                            |
| ------------------------- | ------------------------------- |
| 查看完整提交历史          | `git log`                       |
| 简洁查看提交历史          | `git log --oneline`             |
| 查看某个文件的提交历史    | `git log --oneline <文件名>`    |
| 查看某个文件每次具体修改  | `git log --oneline -p <文件名>` |
| 查看最近 3 次提交         | `git log --oneline -3`          |
| 查看某个文件最近 3 次提交 | `git log --oneline -3 <文件名>` |
| 查看所有分支历史          | `git log --all`                 |
| 简洁查看所有分支历史      | `git log --all --oneline`       |

**总结一下：**  
`git log` 适合看历史，`--oneline` 适合快速浏览，`-p` 适合查看具体改动，而 `--all` 则适合看所有分支的提交记录。



#### 3.追溯历史版本

在 Git 中，常见的“回退”操作主要有三个命令：

- `git reset`
- `git revert`
- `git reflog`

它们都和版本回退有关，但使用场景完全不同。

简单来说：

| 命令         | 作用                               | 是否改写提交历史 | 常见用途                      |
| ------------ | ---------------------------------- | ---------------- | ----------------------------- |
| `git reset`  | 回到某个提交，并移动当前分支指针   | 会               | 本地回退、撤销提交            |
| `git revert` | 生成一个新的提交，用来抵消某次提交 | 不会             | 已推送到远程后的安全回退      |
| `git reflog` | 查看 HEAD 的移动历史               | 不直接修改       | 找回误删提交、误 reset 的版本 |

---

##### 1. git reset

`git reset` 用于让当前分支回到某一个历史提交。

它的核心作用是：

> 移动当前分支指针，并根据参数决定是否修改暂存区和工作区。

常见格式如下：

```bash
git reset [模式] <提交ID>
```

例如：

```bash
git reset --hard HEAD~1
```

表示回退到上一个提交。

---

**reset 的三种常见模式**

`git reset` 最重要的是理解下面三个参数：

| 命令                | 是否移动 HEAD | 是否修改暂存区 | 是否修改工作区 | 说明                                   |
| ------------------- | ------------- | -------------- | -------------- | -------------------------------------- |
| `git reset --soft`  | 是            | 否             | 否             | 只撤销提交，修改内容仍然在暂存区       |
| `git reset --mixed` | 是            | 是             | 否             | 撤销提交和暂存区，修改内容保留在工作区 |
| `git reset --hard`  | 是            | 是             | 是             | 彻底回退，工作区内容也会被覆盖         |

其中：

```bash
git reset <提交ID>
```

默认等价于：

```bash
git reset --mixed <提交ID>
```

---

###### 1.1 git reset --soft

`--soft` 只移动 HEAD，不修改暂存区和工作区。

比如现在提交历史如下：

```bash
A --- B --- C
          ↑
         HEAD
```

如果执行：

```bash
git reset --soft HEAD~1
```

则会变成：

```bash
A --- B
      ↑
     HEAD
```

但是提交 `C` 中的修改内容不会丢失，而是仍然保留在暂存区。

执行 `git status` 可能看到：

```bash
On branch main
Changes to be committed:
  modified:   README.md
```

这说明：

> 提交被撤销了，但是修改内容仍然处于 `git add` 之后的状态。

适合场景：

- commit 信息写错了
- 想撤销最近一次 commit，但保留已经 add 的内容
- 想重新提交一次

例如：

```bash
git reset --soft HEAD~1
git commit -m "重新整理后的提交说明"
```

---

###### 1.2 git reset --mixed

`--mixed` 会移动 HEAD，并重置暂存区，但不会修改工作区。

例如执行：

```bash
git reset --mixed HEAD~1
```

或者直接写：

```bash
git reset HEAD~1
```

效果是：

- 最近一次提交被撤销
- 暂存区被清空
- 修改内容仍然保留在工作区

执行 `git status` 可能看到：

```bash
On branch main
Changes not staged for commit:
  modified:   README.md
```

这说明：

> 修改内容还在，但是已经不是暂存状态了，需要重新执行 `git add`。

适合场景：

- 想撤销 commit
- 也想撤销 `git add`
- 但是不想丢失代码修改

常见操作如下：

```bash
git reset HEAD~1
git status
git add .
git commit -m "重新提交"
```

---

###### 1.3 git reset --hard

`--hard` 是最危险的 reset 模式。

它会：

- 移动 HEAD
- 重置暂存区
- 重置工作区

例如：

```bash
git reset --hard HEAD~1
```

执行后，最近一次提交中的修改内容会从当前工作区消失。

如果原来的历史是：

```bash
A --- B --- C
          ↑
         HEAD
```

执行后变成：

```bash
A --- B
      ↑
     HEAD
```

并且 `C` 中的文件修改也不会留在工作区。

执行 `git status` 可能看到：

```bash
On branch main
nothing to commit, working tree clean
```

适合场景：

- 确认不要最近的修改
- 想彻底回到某个历史版本
- 本地实验代码不要了

注意：

> `git reset --hard` 会直接覆盖工作区内容，执行前一定要确认没有重要修改。

---

###### 1.4 reset 回退到指定提交

可以先查看提交历史：

```bash
git log --oneline
```

例如输出如下：

```bash
f3a91c2 修改登录页面样式
a8c72d1 添加用户注册功能
92bc31e 初始化项目
```

如果想回到 `a8c72d1` 这个版本，可以执行：

```bash
git reset --hard a8c72d1
```

此时当前分支会回到：

```bash
a8c72d1 添加用户注册功能
```

后面的提交虽然暂时看不到了，但并不一定马上消失，可以通过 `git reflog` 找回来。

---

###### 1.5 reset 是否适合已经 push 的提交？

一般来说：

> 已经 push 到远程仓库的提交，不建议使用 `git reset` 回退后再强推。

因为 `reset` 会改写提交历史。

如果你已经把提交推送到了远程：

```bash
git push origin main
```

然后又执行：

```bash
git reset --hard HEAD~1
git push origin main
```

Git 通常会拒绝，因为远程历史和本地历史不一致。

如果强制推送：

```bash
git push -f origin main
```

虽然可以覆盖远程历史，但可能会影响其他协作者。

所以：

| 场景                  | 是否推荐 reset |
| --------------------- | -------------- |
| 本地提交，还没 push   | 推荐           |
| 已经 push，只有自己用 | 可以谨慎使用   |
| 已经 push，多人协作   | 不推荐         |

多人协作时，更推荐使用 `git revert`。

---

##### 2. git revert

`git revert` 也是回退代码，但它和 `reset` 的思路完全不同。

`reset` 是回到过去：

```bash
A --- B --- C
```

执行 reset 后可能变成：

```bash
A --- B
```

而 `revert` 是新增一个提交，用来抵消之前的提交：

```bash
A --- B --- C --- D
```

其中 `D` 是一个新的提交，它的作用是撤销 `C` 的修改。

---

###### 2.1 revert 的基本用法

查看提交历史：

```bash
git log --oneline
```

例如：

```bash
f3a91c2 修改登录页面样式
a8c72d1 添加用户注册功能
92bc31e 初始化项目
```

如果想撤销 `f3a91c2` 这个提交，可以执行：

```bash
git revert f3a91c2
```

Git 会自动生成一个新的提交，提交信息通常类似：

```bash
Revert "修改登录页面样式"
```

提交历史会变成：

```bash
d82ac91 Revert "修改登录页面样式"
f3a91c2 修改登录页面样式
a8c72d1 添加用户注册功能
92bc31e 初始化项目
```

注意：

> 原来的 `f3a91c2` 并没有消失，只是新增了一个提交来抵消它。

---

###### 2.2 revert 的特点

`git revert` 的最大特点是：

> 不会破坏提交历史。

它不会移动分支指针到过去，而是在当前最新位置继续新增提交。

所以它非常适合：

- 已经 push 到远程的提交
- 多人协作项目
- 线上分支回滚
- 需要保留完整操作记录的场景

---

###### 2.3 revert 单个提交

例如我们想撤销最近一次提交：

```bash
git revert HEAD
```

或者撤销指定提交：

```bash
git revert <提交ID>
```

例如：

```bash
git revert f3a91c2
```

执行后 Git 会打开编辑器，让你确认提交信息。

如果不想打开编辑器，可以加上：

```bash
git revert --no-edit f3a91c2
```

这样 Git 会直接使用默认的 revert 提交信息。

---

###### 2.4 revert 多个提交

如果想连续撤销多个提交，可以写多个提交 ID：

```bash
git revert <提交ID1> <提交ID2>
```

例如：

```bash
git revert f3a91c2 a8c72d1
```

也可以使用范围：

```bash
git revert HEAD~3..HEAD
```

表示撤销最近三个提交。

注意：

> `git revert HEAD~3..HEAD` 会依次创建多个新的 revert 提交。

如果你想先只生成修改，不自动提交，可以使用：

```bash
git revert -n HEAD~3..HEAD
```

其中 `-n` 是 `--no-commit` 的简写。

执行后，修改会出现在工作区和暂存区，你可以自己统一提交：

```bash
git commit -m "回退最近三个提交"
```

---

##### 2.5 revert 和 reset 的区别

| 对比项                 | git reset          | git revert   |
| ---------------------- | ------------------ | ------------ |
| 是否保留原提交         | 不保留在当前历史中 | 保留         |
| 是否新增提交           | 不会               | 会           |
| 是否改写历史           | 会                 | 不会         |
| 是否适合已 push 的提交 | 不太适合           | 适合         |
| 是否适合多人协作       | 谨慎使用           | 推荐使用     |
| 回退方式               | 移动 HEAD          | 创建反向提交 |

简单理解：

```bash
git reset
```

更像是：

> 回到过去，把后面的提交从当前历史中拿掉。

而：

```bash
git revert
```

更像是：

> 站在当前版本，新增一个提交，把之前的修改反向抵消掉。

---

##### 3. git reflog

`git reflog` 用于查看 Git 中 HEAD 的移动记录。

很多时候，我们执行了错误操作，比如：

```bash
git reset --hard HEAD~1
```

导致提交“消失”了。

这时不要慌，可以使用：

```bash
git reflog
```

查看之前 HEAD 指向过哪些位置。

---

###### 3.1 reflog 基本使用

执行：

```bash
git reflog
```

可能看到如下输出：

```bash
a8c72d1 HEAD@{0}: reset: moving to HEAD~1
f3a91c2 HEAD@{1}: commit: 修改登录页面样式
a8c72d1 HEAD@{2}: commit: 添加用户注册功能
92bc31e HEAD@{3}: commit: 初始化项目
```

这里的重点是：

```bash
HEAD@{1}
```

它表示 HEAD 上一次所在的位置。

如果我们刚刚误执行了：

```bash
git reset --hard HEAD~1
```

那么原来的提交可能就在：

```bash
HEAD@{1}
```

或者它对应的提交 ID，例如：

```bash
f3a91c2
```

---

###### 3.2 使用 reflog 找回误删提交

假设我们执行了：

```bash
git reset --hard HEAD~1
```

然后发现回退错了。

先查看：

```bash
git reflog
```

输出如下：

```bash
a8c72d1 HEAD@{0}: reset: moving to HEAD~1
f3a91c2 HEAD@{1}: commit: 修改登录页面样式
```

如果想回到 `f3a91c2`，可以执行：

```bash
git reset --hard f3a91c2
```

或者：

```bash
git reset --hard HEAD@{1}
```

这样就可以把刚才“消失”的提交找回来。

---

###### 3.3 reflog 能找回什么？

`git reflog` 可以帮助找回：

- 误删的 commit
- 误执行 reset 后丢失的提交
- checkout 切换前的位置
- rebase 前的位置
- merge 前的位置

例如：

```bash
git reflog
```

可能看到：

```bash
c91f3a0 HEAD@{0}: checkout: moving from dev to main
82fa1d9 HEAD@{1}: commit: 完成订单模块
13a2b8c HEAD@{2}: rebase finished: returning to refs/heads/dev
```

这些记录都可以作为回退参考。

---

###### 3.4 reflog 注意事项

`reflog` 并不是永久保存的。

Git 会在一定时间后清理 reflog 记录。

默认情况下：

- 可达提交的 reflog 通常保留 90 天
- 不可达提交的 reflog 通常保留 30 天

所以如果发现提交丢了，应该尽快使用：

```bash
git reflog
```

找到提交 ID，然后恢复。

---

##### 4. reset、revert、reflog 使用场景总结

###### 4.1 本地提交写错了，还没有 push

推荐使用：

```bash
git reset --soft HEAD~1
```

然后重新提交：

```bash
git commit -m "新的提交说明"
```

原因：

> 只撤销 commit，保留暂存区内容，适合修改 commit 信息或重新组织提交。

---

###### 4.2 本地提交不想要了，但代码还想保留

推荐使用：

```bash
git reset HEAD~1
```

或者：

```bash
git reset --mixed HEAD~1
```

原因：

> 撤销 commit 和暂存区，但保留工作区代码。

---

###### 4.3 本地提交和代码都不要了

推荐使用：

```bash
git reset --hard HEAD~1
```

原因：

> 彻底回到上一个提交，工作区也会被覆盖。

注意：

> 执行前一定确认当前修改不需要了。

---

###### 4.4 已经 push 到远程，想撤销某个提交

推荐使用：

```bash
git revert <提交ID>
```

原因：

> 不改写历史，只新增一个反向提交，适合多人协作。

---

###### 4.5 reset 错了，想找回原来的提交

推荐使用：

```bash
git reflog
```

找到提交 ID 后执行：

```bash
git reset --hard <提交ID>
```

例如：

```bash
git reflog
git reset --hard f3a91c2
```

---

##### 5. 常用命令速查

| 需求                            | 命令                        |
| ------------------------------- | --------------------------- |
| 查看提交历史                    | `git log --oneline`         |
| 撤销最近一次 commit，保留暂存区 | `git reset --soft HEAD~1`   |
| 撤销最近一次 commit，保留工作区 | `git reset HEAD~1`          |
| 撤销最近一次 commit，并丢弃修改 | `git reset --hard HEAD~1`   |
| 回退到指定提交，保留工作区      | `git reset <提交ID>`        |
| 强制回退到指定提交              | `git reset --hard <提交ID>` |
| 撤销某个提交                    |                             |





### 3. 关联Github仓库

现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。

首先，我们需要创建SSH Key公钥，在你的终端输入 `ssh-keygen`

接着，终端机会问你几个问题，**全部直接按** `Enter` 接受默认值即可。

- `Enter file in which to save the key...`：直接按 `Enter`。
- `Enter passphrase...`：直接按 `Enter` (不需要密码)。
- `Enter same passphrase again...`：再次按 `Enter`。

指令完成后，画面上会告诉你公钥存储的路径，例如 `/home/你的用户名/.ssh/id_rsa.pub` 或 `id_ed25519.pub`

钥匙有分公钥 (可以给别人) 和私钥 (绝对要自己保管好)。我们要将“公钥”复制到 GitHub。

在终端机输入以下指令来显示公钥内容。请注意，文件名可能是 `id_rsa.pub` 或 `id_ed25519.pub`，这取决于系统默认值，两个都可以。

```shell
cat ~/.ssh/id_rsa.pub
```

**完整复制**画面上出现的所有内容，它应该是以 `ssh-rsa` 或 `ssh-ed25519` 开头。

前往 GitHub 网站，登录你的账号。

点击右上角的个人头像，选择 **Settings**。

在左边的菜单中，找到并点击 **SSH and GPG keys**。

点击绿色的 **New SSH key** 按钮。

**Title**：给这个钥匙取一个好记的名字，例如 `My Ubuntu VM`。

**Key**：将你刚刚复制的公钥内容，完整地粘贴到这个栏位。

最后点击 **Add SSH key**。



然后，登陆GitHub，点击头像选择**Setting**，在左边的菜单找到并点击**SSH and GPG keys**，点击绿色的 **New SSH key** 按钮。

- **Title**：给这个钥匙取一个好记的名字，例如 `yy`。
- **Key**：将你刚刚复制的公钥内容，完整地粘贴到这个栏位。

- 点击 **Add SSH key**。

完成SSH KEY创建后接着创建Github仓库，在右上角找到“Create a new repo”按钮，创建一个新的仓库,在Repository name填入`你的仓库名词`，这里我叫做`notebooktemp`其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：

目前，在GitHub上的这个``仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

现在，我们根据GitHub的提示，在本地的`notebook`仓库下运行命令：

```shell
git remote add origin git@github.com:yycodes333/notebooktemp.git
git branch -M main
git add .
git commit -m "第一次提交内容"
git push -u origin main
```

*(注：git branch -M main将git init时候默认分支修改为main)*

最后输出以下内容说明你关联仓库成功！

```shell
(base) yy@yy-VivoBook-ASUSLaptop-X512FLC-V5000FL:~/My_Data/temp$ git push -u origin main
Enumerating objects: 154, done.
Counting objects: 100% (154/154), done.
Delta compression using up to 8 threads
Compressing objects: 100% (154/154), done.
Writing objects: 100% (154/154), 23.17 MiB | 2.60 MiB/s, done.
Total 154 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), done.
To github.com:yycodes333/notebooktemp.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.

```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们**第一次推送`master`分支时，必须加上了`-u`参数**，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令，使用`git push origin main(因为这里-M main已经将master改名为main)`即可。

加入后续单独开启新的分支，比如分支dev

```shell
git checkout -b dev
```

*(注：这条命令相当于 `git branch dev` 和 `git checkout dev` 两条命令的结合。现在你已经离开了 `main` 分支，站在了 `dev` 分支上。)*

然后想要将内容（此内容与第一回的main分支区分开可以另建一个本地工作间上传）推送到dev分支上，进行如下操作。

```shell
git add .
git commit -m "在 dev 分支上的第一次提交"
git push -u origin dev#由于是在该分支第一次操作一定要➕u
```

同理，当后续在这个分支操作时候就可以简写成git push 或者 git push origin dev了。



