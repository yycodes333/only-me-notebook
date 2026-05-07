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

实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用`git log`命令查看。

`git log`命令显示从**最近到最远**的提交日志。

当进入 `git log`，与 `git diff`类似的 操作使用如下

| 按键      | 作用           |
| --------- | -------------- |
| `q`       | 退出           |
| `空格`    | 向下翻一页     |
| `b`       | 向上翻一页     |
| `回车`    | 向下滚动一行   |
| `↑` / `↓` | 上下移动       |
| `/关键词` | 搜索关键词     |
| `n`       | 下一个搜索结果 |
| `N`       | 上一个搜索结果 |

`git log `参数如下

| 参数         | 作用               |
| ------------ | ------------------ |
| `--oneline`  | 每个提交显示一行   |
| `--graph`    | 显示分支图         |
| `--decorate` | 显示分支名、标签名 |
| `--all`      | 显示所有分支       |

一般我们使用 `git oneline`，相当于 `git log`的简介版本

##### 情景一

查看当前仓库的历史，并对比-`git log`和 `git log --oneline`

```shell
使用 git log
Author: yy <2033170479@qq.com>
Date:   Wed May 6 19:04:35 2026 +0800

    更新Git笔记
----------------------------------------------------
使用 git log --oneline
d13d5c4 更新Git笔记
```

##### 情景二

若想看到具体某个文件的历史

`git log --oneline <文件名> `   

**注意**：--oneline一定要在文件名前面！

```shell
(base) yy@yy-VivoBook-ASUSLaptop-X512FLC-V5000FL:~/My_Data/notebook$ git log --oneline Outline.md
d1765b1 解决中文乱码问题
```

##### 情景三

若想看到每次提交具体修改了什么内容

`git log --oneline -p <文件名> `

```shell
git log --oneline  -p README.md
a279bf3 测试不用管
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

##### 情境四

若想查看最近几个,我们以三个举例子。

`git log --oneline -3 `

```shell
(base) yy@yy-VivoBook-ASUSLaptop-X512FLC-V5000FL:~/My_Data/notebook$ git log --oneline -3
3d3586a (HEAD -> main, origin/main) 更新Git笔记
d13d5c4 更新Git笔记
8af8375 更新部分Git教学
```

当然对于文件同理

`git log --oneline -3 <文件名>`

```shell
(base) yy@yy-VivoBook-ASUSLaptop-X512FLC-V5000FL:~/My_Data/notebook$ git log --oneline -3 README.md
ccbc4d0 测试
a279bf3 测试不用管
6438a58 解决中文乱码
```

##### 情境五

对于 `git log`查看的当前分支的历史， 若想看到所有分支的历史

`git log --all`

执行git `log -all --oneline`后显示如下

![git5](image/git5.png)

#### 3.追溯历史版本









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



