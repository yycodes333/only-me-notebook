# 基于深度学习的 PiPER 机械臂智能抓取学习笔记

本仓库是一套面向机械臂智能抓取方向的系统学习笔记与项目路线。课程以 AgileX PiPER 六自由度机械臂为最终实践对象，围绕 Python、NumPy、OpenCV、机器人学、ROS2、MoveIt2、RGB-D 相机、手眼标定、点云处理、深度学习和抓取网络，整理出一条从基础到入门再到项目集成的学习路径。

网上关于机械臂抓取的资料较为分散：有些资料只讲 ROS2，有些只讲 OpenCV，有些只讲深度学习模型，有些只给机械臂 SDK 示例。本仓库希望把这些零散知识整理成一门可以按顺序学习、可以逐步实践、最终可以落到 PiPER 机械臂抓取演示的课程式笔记。

当前进度：正文笔记已整理到 [docs/06-deep-learning/6.1 入门基础.md](docs/06-deep-learning/6.1%20入门基础.md)，相机、点云、抓取网络和 PiPER 集成章节已创建占位文件，后续继续补充。

## 课程导论

机械臂智能抓取不是单一算法问题，而是一个完整系统问题。要让机械臂真正抓起物体，需要把感知、识别、位姿转换、路径规划和执行控制连接起来。

本课程最终面向的系统链路如下：

```text
RGB-D 相机采集
-> 图像与点云预处理
-> 深度学习模型预测抓取点或 6D 抓取位姿
-> 手眼标定与 TF 坐标转换
-> MoveIt2 生成机械臂抓取轨迹
-> PiPER SDK/CAN 执行关节与夹爪动作
-> 抓取结果记录、失败分析与系统优化
```

课程学习的重点不是“看过某个教程”，而是能回答以下问题：

- 图像中的一个像素点如何变成机械臂坐标系中的三维点。
- 相机坐标系、机械臂基座坐标系、末端 TCP 坐标系之间如何转换。
- MoveIt2 为什么能够规划机械臂轨迹，规划失败时应该排查什么。
- 深度学习抓取网络的输入和输出分别是什么。
- 抓取点、抓取角、夹爪宽度、6D 抓取位姿如何变成机械臂可执行动作。
- PiPER 机械臂在真实执行时需要考虑哪些限位、安全和误差问题。

## 课程说明

本课程不是纯理论课程，也不是单纯的软件安装记录。它按照“基础能力 -> 工程能力 -> 算法能力 -> 系统集成”的顺序设计。

学习过程中，每个模块都应形成四类产出：

- 理论笔记：能解释概念、公式和系统作用。
- 代码实验：能运行最小示例或复现实验。
- 结果记录：能保存截图、日志、曲线、表格或视频。
- 问题复盘：能说明失败原因、解决过程和下一步改进。

课程最终目标是完成一套基于深度学习、RGB-D 视觉、ROS2/MoveIt2 与 AgileX PiPER 机械臂的抓取闭环系统。

## 课程大纲

下表是本课程的简要大纲。点击“详细内容”可跳转到 README 中对应章节，点击“对应笔记”可进入仓库内的学习笔记目录。

| 章节 | 主题 | 简要说明 | 对应笔记 | 详细内容 |
| :-- | :-- | :-- | :-- | :-- |
| 第 1 章 | 课程导论与工程目标 | 明确机械臂抓取系统的整体链路、学习目标和项目边界 | [README](README.md) | [进入](#chapter-1) |
| 第 2 章 | 工具、仓库与学习方法 | Git、Markdown、资料整理、实验记录和公开仓库规范 | [docs/01-skills](docs/01-skills) | [进入](#chapter-2) |
| 第 3 章 | Python、NumPy、Tensor、Linux、OpenCV | 补齐后续机器人学、视觉和深度学习需要的基础能力 | [docs/03-basics](docs/03-basics) | [进入](#chapter-3) |
| 第 4 章 | 机器人学核心基础 | 坐标变换、D-H、正逆运动学、雅可比、轨迹规划 | [docs/04-robotics](docs/04-robotics) | [进入](#chapter-4) |
| 第 5 章 | 相机、RGB-D 与手眼标定 | 相机模型、标定、深度图、点云生成、坐标转换 | [docs/07-camera-rgbd](docs/07-camera-rgbd) | [进入](#chapter-5) |
| 第 6 章 | ROS2 与 MoveIt2 工程落地 | ROS2 通信、TF2、URDF、SRDF、规划场景和抓取动作 | [docs/05-ros-moveit2](docs/05-ros-moveit2) | [进入](#chapter-6) |
| 第 7 章 | 深度学习与视觉模型 | 神经网络、PyTorch、CNN、数据集、训练和推理 | [docs/06-deep-learning](docs/06-deep-learning) | [进入](#chapter-7) |
| 第 8 章 | 点云处理与抓取网络 | Open3D、点云滤波、分割、GG-CNN、GraspNet、6D 抓取 | [docs/08-point-cloud-3d](docs/08-point-cloud-3d)、[docs/09-grasp-learning](docs/09-grasp-learning) | [进入](#chapter-8) |
| 第 9 章 | PiPER 综合项目实施 | PiPER 硬件、SDK/CAN、MoveIt2 适配和视觉抓取闭环 | [docs/10-piper-integration](docs/10-piper-integration) | [进入](#chapter-9) |
| 第 10 章 | 阶段计划、验收与资源 | 阶段里程碑、验收标准、学习资源和资料声明 | [README](README.md) | [进入](#chapter-10) |

## 仓库结构

```text
.
├── README.md
├── docs/
│   ├── 01-skills/
│   ├── 02-tools-and-software/
│   ├── 03-basics/
│   ├── 04-robotics/
│   ├── 05-ros-moveit2/
│   ├── 06-deep-learning/
│   ├── 07-camera-rgbd/
│   ├── 08-point-cloud-3d/
│   ├── 09-grasp-learning/
│   └── 10-piper-integration/
└── archive/
    └── drafts/
```

| 目录 | 说明 |
| :-- | :-- |
| [docs/01-skills](docs/01-skills) | Git、LaTeX、NotebookLM、资料整理等辅助技能 |
| [docs/02-tools-and-software](docs/02-tools-and-software) | 软件安装、网站和工具记录，公开发布前需复核 |
| [docs/03-basics](docs/03-basics) | Python、NumPy、Tensor、Linux、OpenCV |
| [docs/04-robotics](docs/04-robotics) | 机器人学数学基础、运动学、速度静力、运动规划 |
| [docs/05-ros-moveit2](docs/05-ros-moveit2) | ROS、ROS2、MoveIt2、PiPER 图片资料 |
| [docs/06-deep-learning](docs/06-deep-learning) | 神经网络、CNN、PyTorch、训练流程 |
| [docs/07-camera-rgbd](docs/07-camera-rgbd) | 相机模型、相机标定、RGB-D、手眼标定 |
| [docs/08-point-cloud-3d](docs/08-point-cloud-3d) | 点云基础、Open3D、点云分割、6D 位姿 |
| [docs/09-grasp-learning](docs/09-grasp-learning) | 抓取问题建模、GG-CNN、GraspNet、抓取评估 |
| [docs/10-piper-integration](docs/10-piper-integration) | PiPER SDK、ROS2/MoveIt2 适配、视觉抓取闭环 |
| [archive/drafts](archive/drafts) | 历史草稿归档 |

## 推荐学习顺序

建议按下面顺序推进：

1. 工具与工程习惯：Git、Markdown、Linux、实验记录。
2. 编程基础：Python、NumPy、Tensor、OpenCV。
3. 机器人学：坐标变换、运动学、雅可比、轨迹规划。
4. 相机与 RGB-D：相机模型、标定、深度图、手眼标定。
5. ROS2 与 MoveIt2：TF2、URDF、SRDF、规划场景、抓取动作。
6. 深度学习：神经网络、CNN、PyTorch、训练和推理。
7. 点云与抓取网络：Open3D、点云分割、GG-CNN、GraspNet、6D 抓取。
8. PiPER 项目集成：SDK/CAN、MoveIt2、视觉模型、真实或仿真抓取。

当前目录中 ROS2/MoveIt2 位于 `docs/05-ros-moveit2`，深度学习位于 `docs/06-deep-learning`，相机和点云位于 `docs/07-camera-rgbd`、`docs/08-point-cloud-3d`。实际学习时可以交叉推进，但最终必须把视觉坐标、机械臂坐标和规划执行统一起来。

---

<a id="chapter-1"></a>

# 第 1 章 课程导论与工程目标

## 知识块 1.1 机械臂抓取系统的整体链路

需要学习：

- 机械臂抓取任务由哪些模块组成。
- 感知、定位、规划、控制之间的输入输出关系。
- 为什么单独会 OpenCV、ROS2 或深度学习还不足以完成真实抓取。
- 抓取任务中的典型失败原因：识别错误、深度噪声、标定误差、规划失败、碰撞、夹爪滑落。

学习目标：

- 能用自己的话说明从相机采集到机械臂执行的完整流程。
- 能画出系统架构图，包含相机、模型、TF2、MoveIt2、PiPER SDK。

实践任务：

- 绘制一张 PiPER 视觉抓取系统框架图。
- 写出每个模块的输入、输出和可能失败点。

## 知识块 1.2 PiPER 项目目标

需要学习：

- PiPER 机械臂作为最终实践对象的原因。
- 六自由度机械臂、夹爪、TCP、base_link、joint、link 的基本概念。
- 官方 SDK、ROS/MoveIt 资料、URDF 与真实机械臂之间的关系。

学习目标：

- 明确最终不是只写笔记，而是要形成 PiPER 抓取演示。
- 能解释 PiPER 项目需要哪些软件模块和硬件条件。

实践任务：

- 整理 PiPER 官方资料入口。
- 建立 PiPER 项目术语表：joint、link、TCP、URDF、SDK、CAN、MoveIt2。

## 知识块 1.3 课程成果要求

需要学习：

- 如何把学习笔记转化为项目成果。
- 如何记录实验环境、代码版本、输入数据和输出结果。
- 如何判断一个阶段是否真正学会。

学习目标：

- 每个阶段都能留下可复现材料。
- 最终 README 可以作为项目介绍、学习路线和成果说明。

实践任务：

- 为每章建立学习记录。
- 每个实验至少保存代码、截图、日志和问题复盘。

---

<a id="chapter-2"></a>

# 第 2 章 工具、仓库与学习方法

## 知识块 2.1 Git 与 GitHub 项目组织

对应笔记：[docs/01-skills/Git notes.md](docs/01-skills/Git%20notes.md)

需要学习：

- `git init`、`git add`、`git commit`、`git status`、`git diff`。
- 分支、远程仓库、提交历史、冲突处理。
- GitHub README、目录结构、相对路径图片。
- 哪些文件适合公开，哪些文件应归档或忽略。

学习目标：

- 能维护一个适合 GitHub 展示的学习仓库。
- 能避免图片路径、本地绝对路径、临时文件导致公开仓库混乱。

实践任务：

- 每完成一个学习阶段提交一次清晰 commit。
- 检查 Markdown 图片是否使用相对路径。
- 公开前检查 `docs/02-tools-and-software` 中是否存在不适合发布的个人内容。

## 知识块 2.2 Markdown、LaTeX 与资料整理

对应笔记：[docs/01-skills](docs/01-skills)

需要学习：

- Markdown 标题、表格、代码块、链接、图片引用。
- LaTeX 公式书写，尤其矩阵、向量、坐标变换公式。
- 图片、PDF、截图和实验材料的命名规范。

学习目标：

- 能把机器人学推导、坐标变换链路和实验记录写清楚。
- 能让笔记在 GitHub 页面中正常阅读。

实践任务：

- 为每章笔记统一标题和图片目录。
- 把关键公式改写为可读的 Markdown/LaTeX 表达。

## 知识块 2.3 实验记录方法

需要学习：

- 实验目的、环境、输入、参数、输出、结论。
- 成功实验和失败实验都要记录。
- 版本、依赖、硬件连接状态对复现的影响。

学习目标：

- 避免“当时能跑，现在无法复现”。
- 能通过实验记录定位问题，而不是只凭印象排查。

实践任务：

- 建立实验记录表。
- 每次 MoveIt2、相机标定、模型训练、机械臂执行都记录环境和结果。

---

<a id="chapter-3"></a>

# 第 3 章 Python、NumPy、Tensor、Linux、OpenCV 基础

## 知识块 3.1 Python 编程基础

对应笔记：[docs/03-basics/3.1Python编程基础.md](docs/03-basics/3.1Python编程基础.md)

需要学习：

- 变量、列表、字典、字符串、条件、循环。
- 函数、类、模块、包。
- 文件读写、异常处理、日志记录。
- `venv`、`conda`、依赖安装和环境隔离。
- 命令行参数、配置文件、JSON/YAML 读取。

学习目标：

- 能写图像处理脚本、数据预处理脚本、实验日志脚本。
- 能读懂开源项目中的 Python 文件结构。
- 能为 ROS2 Python 节点和 PyTorch 训练脚本打基础。

实践任务：

- 写一个图片批处理脚本。
- 写一个自动保存实验参数和输出路径的日志脚本。
- 写一个读取 YAML 配置并运行实验的模板脚本。

## 知识块 3.2 NumPy 与矩阵计算

对应笔记：[docs/03-basics/3.2 Numpy 与矩阵计算.md](<docs/03-basics/3.2 Numpy 与矩阵计算.md>)

需要学习：

- ndarray、shape、dtype、reshape、transpose。
- 索引、切片、布尔索引、高级索引。
- 广播机制、向量化运算。
- 点乘、矩阵乘法、逆矩阵、单位矩阵。
- 齐次坐标、4x4 变换矩阵。
- 三维点批量处理。
- 图像矩阵和深度图矩阵表达。

学习目标：

- 能实现 `T_base_camera @ P_camera` 这类坐标变换。
- 能批量处理相机坐标系下的点云或目标点。
- 能理解图像、深度图、张量和点云在数组层面的关系。

实践任务：

- 构造一个 4x4 齐次变换矩阵并变换一组 3D 点。
- 将欧拉角转换为旋转矩阵，再组合成位姿矩阵。
- 分析一张 RGB 图像和一张深度图的 shape、dtype、数值范围。

## 知识块 3.3 Tensor 与深度学习数据表达

对应笔记：[docs/03-basics/3.3Tensor基础.md](docs/03-basics/3.3Tensor基础.md)

需要学习：

- Tensor 的维度、索引、拼接、变换。
- NumPy array 与 PyTorch Tensor 的区别。
- 图像格式 `HWC`、`CHW`、`NCHW`。
- batch、channel、height、width 的含义。
- CPU/GPU Tensor 转换。
- 自动求导的基本概念。

学习目标：

- 能读懂模型输入输出维度。
- 能将 OpenCV 图像转换为 PyTorch 模型输入。
- 能理解深度图、RGB 图和点云数据如何进入模型。

实践任务：

- 将一张 OpenCV 图像从 `HWC` 转为 `NCHW`。
- 构造一个 batch 输入 CNN。
- 写一个简单自动求导示例。

## 知识块 3.4 Linux 与工程环境

对应笔记：[docs/03-basics/3.4Linux使用.md](docs/03-basics/3.4Linux使用.md)

需要学习：

- Linux 文件系统、路径、权限。
- shell 常用命令、环境变量、包管理。
- Python 环境、CUDA/cuDNN、ROS2 工作空间。
- 驱动安装、设备权限、串口/CAN 设备查看。
- 日志查看和错误定位。

学习目标：

- 能配置 ROS2、MoveIt2、RealSense、PyTorch 环境。
- 能根据报错定位依赖、路径、权限和驱动问题。

实践任务：

- 整理一份环境版本表。
- 记录常用命令和常见错误。
- 建立 ROS2 工作空间并成功编译一个包。

## 知识块 3.5 OpenCV 图像处理

对应笔记：[docs/03-basics/3.5OpenCV图像处理.md](docs/03-basics/3.5OpenCV图像处理.md)

需要学习：

- 图像读取、显示、保存。
- BGR、RGB、HSV、灰度图、二值图。
- 滤波、去噪、阈值分割。
- 形态学操作：腐蚀、膨胀、开运算、闭运算。
- 边缘检测、轮廓提取、ROI 裁剪。
- 最小外接矩形、目标中心、角度估计。

学习目标：

- 能完成简单目标区域提取。
- 能实现不依赖深度学习的 baseline。
- 能为后续相机标定、抓取点检测和模型输入预处理打基础。

实践任务：

- 用 HSV 提取一个颜色块。
- 用轮廓计算目标中心和方向角。
- 输出一个抓取点候选 `(u, v, theta)`。

---

<a id="chapter-4"></a>

# 第 4 章 机器人学核心基础

## 知识块 4.1 空间描述与坐标变换

对应笔记：[docs/04-robotics/4.2 数学基础.md](<docs/04-robotics/4.2 数学基础.md>)

需要学习：

- 位置、姿态、位姿。
- 旋转矩阵、欧拉角、轴角、四元数。
- 齐次变换矩阵。
- 复合变换和逆变换。
- 坐标系之间的乘法顺序。

学习目标：

- 能解释 `base_link`、`camera_link`、`tool0`、`object` 的关系。
- 能写出 `T_base_object = T_base_camera * T_camera_object`。
- 能判断角度单位、旋转顺序和坐标方向错误。

实践任务：

- 画出 PiPER 抓取系统坐标链。
- 用 NumPy 验证一次坐标变换。
- 比较欧拉角、四元数和旋转矩阵的优缺点。

## 知识块 4.2 D-H 建模与正运动学

对应笔记：[docs/04-robotics/4.3 运动学 Kinematics.md](<docs/04-robotics/4.3 运动学 Kinematics.md>)

需要学习：

- 连杆、关节、关节轴。
- 标准 D-H 与改进 D-H。
- 正运动学。
- TCP 工具坐标。
- URDF 模型与数学模型的对应关系。

学习目标：

- 能从关节角计算末端位姿。
- 能读懂 URDF 中 link、joint、origin、axis、limit。
- 能理解 PiPER 物理结构如何映射到模型。

实践任务：

- 为一个简单机械臂写 D-H 参数表。
- 用 Python 或 MATLAB 验证正运动学。
- 将 PiPER 的 link/joint 与物理结构图对应起来。

## 知识块 4.3 逆运动学、雅可比与奇异性

对应笔记：[docs/04-robotics/4.4 速度和静力.md](<docs/04-robotics/4.4 速度和静力.md>)

需要学习：

- 逆运动学多解。
- 解析法和数值法。
- 雅可比矩阵。
- 末端速度与关节速度映射。
- 奇异位形、操作度和规划失败原因。

学习目标：

- 能解释为什么某些抓取位姿不可达或不稳定。
- 能理解 MoveIt2 规划失败可能来自关节限制、奇异性或碰撞。

实践任务：

- 记录一个奇异姿态案例。
- 对比多个 IK 解的关节角差异。
- 说明某个抓取目标为什么不适合执行。

## 知识块 4.4 轨迹规划、检测与执行

对应笔记：[docs/04-robotics/4.5 运动规划、检测与执行.md](<docs/04-robotics/4.5 运动规划、检测与执行.md>)

需要学习：

- 关节空间规划。
- 笛卡尔空间规划。
- 三次和五次插值。
- 速度、加速度、平滑性。
- 碰撞检测和避障。
- 抓取动作序列。

学习目标：

- 能把抓取拆成 pre-grasp、approach、grasp、lift、place、home。
- 能理解为什么真实机械臂执行不能只给一个目标点。

实践任务：

- 设计抓取状态机。
- 生成一条简单关节轨迹曲线。
- 分析桌面、物体、夹爪之间的碰撞关系。

---

<a id="chapter-5"></a>

# 第 5 章 相机、RGB-D 与手眼标定

## 知识块 5.1 相机模型与成像基础

对应笔记：[docs/07-camera-rgbd/7.1 相机模型与成像基础.md](<docs/07-camera-rgbd/7.1 相机模型与成像基础.md>)

需要学习：

- 针孔相机模型。
- 像素坐标、图像坐标、归一化坐标、相机坐标。
- 焦距、主点、内参矩阵。
- 深度值和三维点恢复。
- 深度尺度和无效深度。

学习目标：

- 能从 `(u, v, depth)` 计算相机坐标系下的 `(x, y, z)`。
- 能理解为什么 RGB-D 相机能为机械臂提供空间抓取信息。

实践任务：

- 写出像素转三维点公式。
- 用一组相机内参和深度值计算 3D 点。
- 分析深度缺失对抓取的影响。

## 知识块 5.2 相机标定与畸变校正

对应笔记：[docs/07-camera-rgbd/7.2 相机标定与畸变校正.md](<docs/07-camera-rgbd/7.2 相机标定与畸变校正.md>)

需要学习：

- 棋盘格和 Charuco 标定板。
- 内参、外参、畸变参数。
- 径向畸变、切向畸变。
- 重投影误差。
- OpenCV 标定流程。

学习目标：

- 能完成一次相机标定。
- 能判断标定结果是否可信。
- 能保存并复用相机参数。

实践任务：

- 采集多角度标定板图片。
- 使用 OpenCV 输出 `camera_matrix`、`dist_coeffs` 和重投影误差。
- 对比畸变校正前后的图像。

## 知识块 5.3 RGB-D 相机与 RealSense

对应笔记：[docs/07-camera-rgbd/7.3 RGB-D 相机与 RealSense.md](<docs/07-camera-rgbd/7.3 RGB-D 相机与 RealSense.md>)

需要学习：

- RGB 图、深度图、点云。
- 彩色图和深度图对齐。
- RealSense SDK 或 ROS2 wrapper。
- `camera_info`、image topic、point cloud topic。
- 深度噪声、反光、遮挡和视角问题。

学习目标：

- 能稳定采集 RGB-D 数据。
- 能在 RViz2 或 Open3D 中显示点云。
- 能理解相机安装位置对抓取稳定性的影响。

实践任务：

- 保存一组 RGB 图和深度图。
- 在 ROS2 中查看相机 topic。
- 在 RViz2 中显示点云。

## 知识块 5.4 手眼标定与坐标转换

对应笔记：[docs/07-camera-rgbd/7.4 手眼标定与坐标转换.md](<docs/07-camera-rgbd/7.4 手眼标定与坐标转换.md>)

需要学习：

- eye-in-hand 和 eye-to-hand。
- `T_base_camera`、`T_camera_object`、`T_base_object`。
- 标定板位姿和机械臂末端位姿。
- OpenCV hand-eye calibration。
- TF2 静态变换发布。
- 标定误差验证。

学习目标：

- 能把相机看到的目标位姿转换到 PiPER 基座坐标系。
- 能解释手眼标定误差如何导致抓取偏移。

实践任务：

- 设计 PiPER 抓取系统 TF 树。
- 采集多组标定数据。
- 用已知点验证坐标转换误差。

---

<a id="chapter-6"></a>

# 第 6 章 ROS2 与 MoveIt2 工程落地

## 知识块 6.1 ROS 与 ROS2 基础

对应笔记：[docs/05-ros-moveit2/5.1 ROS.md](<docs/05-ros-moveit2/5.1 ROS.md>)、[docs/05-ros-moveit2/5.2 ROS2.md](<docs/05-ros-moveit2/5.2 ROS2.md>)

需要学习：

- 节点、话题、服务、Action。
- 参数、launch、workspace、package。
- rosbag 录制和回放。
- 时间戳、消息同步、QoS。
- ROS1 与 ROS2 差异。

学习目标：

- 能读懂 PiPER ROS 包结构。
- 能设计相机、模型推理、规划、执行之间的数据流。
- 能用 rosbag 保存实验数据。

实践任务：

- 写一个最小发布订阅节点。
- 用 launch 启动多个节点。
- 录制一段相机或位姿数据。

## 知识块 6.2 TF2 坐标树

需要学习：

- frame、transform、static transform。
- `base_link`、`tool0`、`camera_link`、`camera_color_optical_frame`。
- TF 时间戳。
- RViz2、`tf2_echo`、`view_frames` 调试。

学习目标：

- 能设计抓取系统的坐标树。
- 能定位 TF 缺失、frame 名称错误、时间戳不一致等问题。

实践任务：

- 画出 PiPER 抓取 TF 树。
- 发布相机到机械臂基座的静态变换。
- 在 RViz2 中检查坐标轴方向。

## 知识块 6.3 URDF、Xacro 与 SRDF

对应笔记：[docs/05-ros-moveit2/5.3 Moveit2.md](<docs/05-ros-moveit2/5.3 Moveit2.md>)

需要学习：

- URDF 的 link、joint、origin、axis、limit。
- Xacro 参数化建模。
- visual、collision、inertial。
- SRDF 规划组、末端执行器、禁用碰撞对。
- mesh 文件和模型显示。

学习目标：

- 能让 PiPER 或等效机械臂模型在 RViz2 中正确显示。
- 能理解 URDF 关节限制如何影响 MoveIt2 规划。

实践任务：

- 加载 PiPER URDF/mesh。
- 检查关节轴方向和关节限制。
- 配置 arm 和 gripper 规划组。

## 知识块 6.4 MoveIt2 规划与抓取动作

需要学习：

- planning group。
- 目标位姿规划。
- Cartesian Path。
- planning scene。
- 碰撞体添加。
- pre-grasp、grasp、lift、place。
- 规划失败排查。

学习目标：

- 能让机械臂模型规划到给定目标位姿。
- 能添加桌面和目标物碰撞体。
- 能把视觉输出的抓取位姿转成 MoveIt2 可执行目标。

实践任务：

- 在 RViz2 中完成一次单点规划。
- 添加桌面碰撞体并重新规划。
- 写出抓取动作序列伪代码。

---

<a id="chapter-7"></a>

# 第 7 章 深度学习与视觉模型

## 知识块 7.1 神经网络入门

对应笔记：[docs/06-deep-learning/6.1 入门基础.md](docs/06-deep-learning/6.1%20入门基础.md)

需要学习：

- 神经元、网络层、激活函数。
- 损失函数、梯度下降、反向传播。
- 训练集、验证集、测试集。
- 学习率、batch size、epoch。
- 欠拟合、过拟合、正则化。

学习目标：

- 能解释模型为什么能从数据中学习。
- 能看懂 loss、metric、learning rate。
- 能为抓取网络训练打基础。

实践任务：

- 训练一个线性回归模型。
- 完成一个简单分类实验。
- 写出反向传播的直观解释。

## 知识块 7.2 PyTorch 训练流程

需要学习：

- Dataset、DataLoader。
- `nn.Module`、forward。
- loss、optimizer、scheduler。
- GPU/CPU 切换。
- checkpoint 保存和加载。
- inference 推理脚本。

学习目标：

- 能独立完成数据读取、训练、验证、保存和推理。
- 能读懂开源抓取网络的代码结构。

实践任务：

- 写一个通用 PyTorch 训练模板。
- 记录 loss 曲线和训练参数。
- 加载模型权重并推理单张图片。

## 知识块 7.3 CNN、ResNet、UNet 与视觉任务

需要学习：

- 卷积、padding、stride、pooling。
- feature map 尺寸变化。
- 分类、检测、分割任务区别。
- ResNet 残差结构。
- UNet 编码器和解码器。
- MobileNet 轻量化思路。

学习目标：

- 能从普通图像任务过渡到抓取图像任务。
- 能理解抓取网络为什么输出质量图、角度图、宽度图。

实践任务：

- 训练一个基础 CNN。
- 记录每层 feature map shape。
- 对比分类模型和分割模型输出差异。

## 知识块 7.4 数据集、训练与评估

需要学习：

- 数据采集、标注、增强。
- 训练集、验证集、测试集划分。
- accuracy、precision、recall、IoU。
- 抓取任务中的离线指标和真实成功率。

学习目标：

- 能设计自己的 PiPER 抓取数据记录方式。
- 能判断模型指标是否真正有助于机械臂执行。

实践任务：

- 设计数据集目录：images、labels、splits。
- 做翻转、裁剪、噪声等数据增强。
- 保存训练曲线和评估结果。

---

<a id="chapter-8"></a>

# 第 8 章 点云处理与抓取网络

## 知识块 8.1 点云基础与 Open3D

对应笔记：[docs/08-point-cloud-3d/8.1 点云基础与 Open3D.md](<docs/08-point-cloud-3d/8.1 点云基础与 Open3D.md>)

需要学习：

- PCD、PLY、PointCloud2。
- 点坐标、颜色、法向量。
- 深度图转点云。
- 点云显示、裁剪、保存。
- Open3D 基础 API。

学习目标：

- 能从 RGB-D 数据生成点云。
- 能用 Open3D 或 RViz2 观察空间结构。

实践任务：

- 使用相机内参生成点云。
- 保存 PCD/PLY 文件。
- 裁剪桌面上方区域。

## 知识块 8.2 点云滤波、分割与配准

对应笔记：[docs/08-point-cloud-3d/8.2 点云滤波分割与配准.md](<docs/08-point-cloud-3d/8.2 点云滤波分割与配准.md>)

需要学习：

- 体素下采样。
- 统计滤波、半径滤波。
- RANSAC 平面分割。
- 欧式聚类。
- ICP 配准。

学习目标：

- 能从桌面场景中分离目标物体点云。
- 能减少背景和噪声对抓取预测的影响。

实践任务：

- 去除桌面平面。
- 分割目标物体点云。
- 对齐两帧点云并记录误差。

## 知识块 8.3 抓取问题建模

对应笔记：[docs/09-grasp-learning/9.1 抓取问题建模.md](<docs/09-grasp-learning/9.1 抓取问题建模.md>)

需要学习：

- 平面抓取和 6D 抓取区别。
- 抓取点、抓取角、夹爪宽度。
- 接近方向、抓取质量、碰撞检测。
- 夹爪宽度、物体尺寸、可达性约束。

学习目标：

- 能把抓取任务表示为模型可预测、MoveIt2 可执行的目标。
- 能区分目标检测、目标位姿估计和抓取位姿预测。

实践任务：

- 对比 `(u, v, theta, width)` 和 6D pose。
- 说明夹爪宽度如何约束抓取。
- 设计抓取候选筛选规则。

## 知识块 8.4 平面抓取网络 GG-CNN

对应笔记：[docs/09-grasp-learning/9.2 平面抓取网络 GG-CNN.md](<docs/09-grasp-learning/9.2 平面抓取网络 GG-CNN.md>)

需要学习：

- Cornell/Jacquard 平面抓取数据集。
- 深度图输入。
- 抓取质量图、角度图、宽度图。
- 最佳抓取点后处理。
- 图像坐标到三维坐标转换。

学习目标：

- 能在离线深度图上得到一个平面抓取候选。
- 能理解平面抓取网络如何接入机械臂执行。

实践任务：

- 运行或复现 GG-CNN 推理。
- 从输出图中选取最佳抓取点。
- 将 `(u, v, theta, width)` 转换为机械臂可用信息。

## 知识块 8.5 点云抓取与 6D 抓取

对应笔记：[docs/09-grasp-learning/9.3 点云抓取与 6D 抓取.md](<docs/09-grasp-learning/9.3 点云抓取与 6D 抓取.md>)

需要学习：

- GPD、GraspNet、AnyGrasp 等 6D 抓取思路。
- 点云输入、候选采样、抓取评分。
- 抓取姿态可视化。
- 碰撞检测和可达性筛选。

学习目标：

- 能从点云中生成多个抓取候选。
- 能筛选最适合 PiPER 执行的抓取姿态。

实践任务：

- 可视化多个抓取候选。
- 按质量、碰撞、可达性排序。
- 将最佳 6D 抓取姿态转成 MoveIt2 目标 pose。

---

<a id="chapter-9"></a>

# 第 9 章 PiPER 综合项目实施

## 知识块 9.1 PiPER 硬件与物理结构

对应笔记：[docs/10-piper-integration/10.1 PiPER 硬件与 SDK 控制.md](<docs/10-piper-integration/10.1 PiPER 硬件与 SDK 控制.md>)

需要学习：

- PiPER 六自由度机械臂结构。
- 连杆、关节、夹爪、TCP。
- 工作半径、负载、关节限制。
- 固件、SDK、CAN 通信。
- 安全限位、急停、低速测试。

学习目标：

- 能把 PiPER 物理结构对应到 URDF 模型。
- 能理解关节限制和夹爪限制对抓取动作的影响。
- 能建立安全测试流程。

当前参考信息：

- AgileX PiPER 产品页标注 PiPER 为轻量六自由度机械臂，本体约 4.2 kg，负载约 1.5 kg。
- 本仓库已有尺寸图：[docs/05-ros-moveit2/images/Piper物理大小.png](docs/05-ros-moveit2/images/Piper物理大小.png)。
- 图中可见工作半径约 R626.7 mm，并标注 123 mm、285.05 mm、250.65 mm、91 mm 等关键尺寸。

## 知识块 9.2 PiPER SDK/CAN 控制

需要学习：

- `python-can`。
- `piper_sdk` 安装与接口。
- CAN 设备启用、波特率、权限。
- 机械臂状态读取。
- 关节空间控制。
- 夹爪控制。
- 异常处理和安全停止。

学习目标：

- 能用 Python 读取 PiPER 状态。
- 能在安全速度下执行最小关节动作。
- 能控制夹爪开合并记录状态。

实践任务：

- 读取关节角、末端状态、夹爪状态。
- 执行一个安全关节动作。
- 测试夹爪开合。

## 知识块 9.3 PiPER ROS2 与 MoveIt2 适配

对应笔记：[docs/10-piper-integration/10.2 PiPER ROS2 与 MoveIt2 适配.md](<docs/10-piper-integration/10.2 PiPER ROS2 与 MoveIt2 适配.md>)

需要学习：

- 官方 `piper_ros` 中的 URDF、mesh、MoveIt 资料。
- ROS2 侧模型适配。
- planning group、end effector、joint limits。
- fake controller 与真实控制接口。

学习目标：

- 能在 RViz2 中正确显示 PiPER 模型。
- 能在 MoveIt2 中完成单点规划和笛卡尔路径规划。
- 能在真实控制前完成仿真验证。

关节限制参考：

| 关节 | 类型 | URDF limit |
| :-- | :-- | :-- |
| joint1 | revolute | -2.618 到 2.618 rad |
| joint2 | revolute | 0 到 3.14 rad |
| joint3 | revolute | -2.967 到 0 rad |
| joint4 | revolute | -1.745 到 1.745 rad |
| joint5 | revolute | -1.22 到 1.22 rad |
| joint6 | revolute | -2.0944 到 2.0944 rad |
| joint7 | prismatic gripper | 0 到 0.035 m |

实际控制限位应以当前固件、SDK 和本地 URDF 为准。

## 知识块 9.4 视觉抓取闭环系统

对应笔记：[docs/10-piper-integration/10.3 视觉抓取闭环系统.md](<docs/10-piper-integration/10.3 视觉抓取闭环系统.md>)

需要学习：

- RGB-D 相机启动。
- 图像或点云输入生成。
- 抓取模型推理。
- 坐标转换。
- MoveIt2 规划。
- PiPER SDK 执行。
- 抓取失败重试。

学习目标：

- 能完成一次完整仿真或真实抓取流程。
- 能解释系统中每个模块的输入、输出和失败点。

推荐闭环流程：

```text
启动相机
-> 获取 RGB-D 数据
-> 生成图像/点云输入
-> 模型预测抓取候选
-> 选择最佳抓取位姿
-> 相机坐标转换到 base_link
-> MoveIt2 规划 pre-grasp
-> 执行接近、下降、闭合、抬升
-> 记录是否成功
```

## 知识块 9.5 实验记录与问题排查

对应笔记：[docs/10-piper-integration/10.4 实验记录与问题排查.md](<docs/10-piper-integration/10.4 实验记录与问题排查.md>)

需要学习：

- 抓取成功率。
- 平均规划时间。
- 平均执行时间。
- 位姿误差。
- 失败类型统计。
- 光照、相机位置、目标物类别、机械臂初始姿态记录。

学习目标：

- 能从“跑通一次”提升到“可复现、可分析、可优化”。
- 能用实验数据支撑后续模型和系统改进。

实践任务：

| 指标 | 记录方式 |
| :-- | :-- |
| 抓取成功率 | 成功次数 / 总尝试次数 |
| 规划时间 | MoveIt2 日志或计时代码 |
| 执行时间 | SDK 执行前后时间戳 |
| 位姿误差 | 视觉估计与实测对比 |
| 失败原因 | 识别失败、标定误差、规划失败、碰撞、夹爪滑落 |

---

<a id="chapter-10"></a>

# 第 10 章 阶段计划、验收标准与学习资源

## 阶段计划

| 阶段 | 核心任务 | 主要成果 |
| :-- | :-- | :-- |
| 第一阶段 | Python、NumPy、OpenCV、Linux、Git | 基础脚本、图像处理实验、环境记录 |
| 第二阶段 | 机器人学空间变换、运动学、轨迹规划 | 坐标变换推导、正运动学验证、抓取动作状态机 |
| 第三阶段 | 相机模型、RGB-D、手眼标定 | 标定参数、深度转点云、TF 坐标链 |
| 第四阶段 | ROS2、TF2、URDF、MoveIt2 | RViz2 模型、规划组、抓取轨迹 |
| 第五阶段 | PyTorch、CNN、数据集、模型评估 | 训练脚本、loss 曲线、推理结果 |
| 第六阶段 | Open3D、点云分割、GG-CNN、GraspNet | 点云处理结果、抓取候选可视化 |
| 第七阶段 | PiPER SDK、MoveIt2、视觉抓取闭环 | 演示视频、实验报告、失败分析 |

## 验收标准

入门验收：

- 能用 NumPy 完成齐次坐标变换。
- 能用 OpenCV 检测简单目标中心和角度。
- 能解释 PiPER 的 link、joint、TCP、base_link。
- 能训练一个简单 PyTorch 模型。

工程验收：

- 能完成 RGB-D 相机标定和深度图读取。
- 能在 ROS2 中发布和订阅图像、位姿或状态消息。
- 能设计并查看 TF2 坐标树。
- 能在 RViz2 中显示机械臂模型。
- 能在 MoveIt2 中规划抓取轨迹。

项目验收：

- 能用 PiPER SDK 读取和控制机械臂。
- 能把视觉模型输出转成机械臂 base 坐标系下的抓取位姿。
- 能完成至少 20 次抓取实验并统计成功率。
- 能写出失败原因分析和下一步优化计划。

## 推荐学习资源

官方文档：

| 资源 | 推荐用途 | 链接 |
| :-- | :-- | :-- |
| NumPy User Guide | 数组、矩阵、广播、向量化 | https://numpy.org/doc/stable/user/ |
| OpenCV Python Tutorials | 图像处理、相机标定、姿态估计 | https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html |
| PyTorch Tutorials | PyTorch 入门、训练模板、模型保存 | https://pytorch.org/tutorials/ |
| ROS 2 Documentation | ROS2 节点、话题、服务、Action、TF2 | https://docs.ros.org/ |
| MoveIt 2 Tutorials | MoveIt2 规划、配置、运动规划接口 | https://moveit.picknik.ai/main/doc/tutorials/tutorials.html |
| Open3D Documentation | 点云读取、显示、滤波、配准 | https://www.open3d.org/docs/ |
| Intel RealSense GitHub | RGB-D 相机 SDK 与 ROS wrapper | https://github.com/IntelRealSense |

机器人学与运动规划：

| 资源 | 推荐用途 | 链接 |
| :-- | :-- | :-- |
| Modern Robotics | 坐标变换、运动学、动力学、控制 | https://modernrobotics.northwestern.edu/ |
| Modern Robotics GitHub | 配套代码和公式实现 | https://github.com/NxRLab/ModernRobotics |
| Peter Corke Robotics Toolbox for Python | 机械臂建模与仿真 | https://github.com/petercorke/robotics-toolbox-python |
| Peter Corke Robotics Toolbox for MATLAB | MATLAB 机器人学仿真 | https://github.com/petercorke/robotics-toolbox-matlab |

抓取学习与开源项目：

| 资源 | 推荐用途 | 链接 |
| :-- | :-- | :-- |
| GG-CNN | 平面抓取入门，质量图、角度图、宽度图 | https://github.com/dougsm/ggcnn |
| robotic-grasping | Cornell 数据集抓取实现参考 | https://github.com/skumra/robotic-grasping |
| GraspNet Baseline | 6D 抓取、点云抓取候选生成 | https://github.com/graspnet/graspnet-baseline |
| GPD | 点云抓取姿态检测经典工程参考 | https://github.com/atenpas/gpd |
| Dex-Net | 抓取质量评价和数据驱动抓取 | https://github.com/BerkeleyAutomation/dex-net |
| PointNet++ PyTorch | 点云深度学习基础参考 | https://github.com/erikwijmans/Pointnet2_PyTorch |

PiPER 官方资源：

| 资源 | 用途 | 链接 |
| :-- | :-- | :-- |
| AgileX PiPER 产品页 | 机械臂规格、物理结构、展示介绍 | https://global.agilex.ai/products/piper |
| AgileXRobotics GitHub | PiPER 相关官方仓库入口 | https://github.com/AgilexRobotics |
| piper_sdk | Python SDK、CAN 控制、状态读取 | https://github.com/agilexrobotics/piper_sdk |
| piper_ros | ROS、MoveIt、URDF、仿真资料 | https://github.com/agilexrobotics/piper_ros |

中文学习资源建议：

| 资源 | 推荐用途 | 使用建议 |
| :-- | :-- | :-- |
| 李沐《动手学深度学习》 | 深度学习与 PyTorch 入门 | 先学 CNN 和训练流程 |
| 鱼香ROS | ROS2 环境、节点、工程实践 | 对照官方文档验证版本 |
| 古月居 | ROS、MoveIt、机械臂实践 | 适合建立工程直觉 |
| 赵虚左 ROS 教程 | ROS 基础通信模型 | 用作 ROS 入门补充 |
| B站关键词：`MoveIt2 机械臂 抓取` | MoveIt2 实操视频 | 重点看版本和环境 |
| B站关键词：`RealSense ROS2 点云` | RGB-D 与点云实践 | 重点看驱动和 ROS2 发行版 |

## README 模板自查

本仓库的 README 已覆盖一般 GitHub 项目首页应具备的核心信息：

- 项目标题与一句话说明。
- 课程导论和课程说明。
- 当前进度。
- 可点击课程大纲。
- 仓库结构。
- 详细学习内容和实践任务。
- 阶段计划与验收标准。
- 学习资源。
- 资料来源与使用声明。

## 资料来源与使用声明

本课程笔记由本人在学习机械臂智能抓取方向过程中整理形成，内容来源包括：

- 本人对 Python、OpenCV、机器人学、ROS2、MoveIt2、深度学习、点云处理和 PiPER 机械臂资料的学习总结。
- 官方文档、开源项目、公开教程、论文资料和他人经验文章的学习参考。
- 实际配置、调试、实验和问题复盘过程中的个人记录。

本仓库仅用于个人学习整理、知识分享和项目交流，不涉及课程售卖、资料倒卖或商业培训用途。仓库中引用或借鉴的公开资料，其版权归原作者或原项目所有；如有引用不完整、来源标注不充分或内容不适合公开展示的情况，后续会继续修正。使用本仓库内容时，请以对应官方文档、开源项目许可证和原作者说明为准。

## 发布提醒

[docs/02-tools-and-software](docs/02-tools-and-software) 中包含个人软件安装和工具记录，公开发布到 GitHub 前需要自行复核是否适合保留。涉及账号、授权、商业软件、破解、隐私路径或个人配置的内容不建议公开。

## 附录：记录模板

学习日志：

| 日期 | 学习内容 | 学习时长 | 是否完成 | 遇到的问题 | 解决方式 | 下一步 |
| :-- | :-- | --: | :-- | :-- | :-- | :-- |
|  |  |  |  |  |  |  |

实验记录：

| 实验名称 | 实验目的 | 输入数据 | 关键参数 | 输出结果 | 是否成功 | 问题分析 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- |
|  |  |  |  |  |  |  |

项目进度：

| 模块 | 当前任务 | 完成度 | 风险点 | 解决方案 | 下一步 |
| :-- | :-- | --: | :-- | :-- | :-- |
|  |  |  |  |  |  |

常见缩写：

| 缩写 | 含义 |
| :-- | :-- |
| ROS2 | Robot Operating System 2 |
| TF2 | ROS2 坐标变换系统 |
| URDF | Unified Robot Description Format |
| Xacro | XML macro for URDF |
| SRDF | Semantic Robot Description Format |
| TCP | Tool Center Point |
| RGB-D | RGB + Depth |
| IK | Inverse Kinematics |
| FK | Forward Kinematics |
| D-H | Denavit-Hartenberg |
| PCL | Point Cloud Library |
| Open3D | 3D 数据处理开源库 |
| 6D Pose | 三维位置 + 三维姿态 |
| MoveIt2 | ROS2 机械臂运动规划框架 |
