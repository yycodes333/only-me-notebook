# Git 使用教程

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

### 1. 创建版本库

什么是版本库呢？版本库又名仓库（Repository），你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

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

#### 1. git statue & git diff

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

![git3](/home/yy/My_Data/notebook/Usage tips habit cultivation/image/git3.png)



#### 2. git log
