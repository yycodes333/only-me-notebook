# OpenCV 图像处理学习手册（初学者版）

> 适合零基础到入门阶段系统学习。  
> 内容覆盖：基础常识、OpenCV 基本操作、图像预处理、颜色空间、阈值、滤波、几何变换、边缘、轮廓、形态学、掩膜、文件格式、实战流程与完整代码示例。  
> 本文可直接复制为 `Markdown` 文档使用。

---

# 目录

- [第 0 章：学习目标与建议](#第-0-章学习目标与建议)
- [第 1 章：图像处理与 OpenCV 基础常识](#第-1-章图像处理与-opencv-基础常识)
  - [1.1 什么是图像处理](#11-什么是图像处理)
  - [1.2 什么是 OpenCV](#12-什么是-opencv)
  - [1.3 为什么要学习 OpenCV 图像处理](#13-为什么要学习-opencv-图像处理)
  - [1.4 图像在计算机中本质是什么](#14-图像在计算机中本质是什么)
  - [1.5 常见图像类型](#15-常见图像类型)
  - [1.6 前景、背景、目标、噪声等术语解释](#16-前景背景目标噪声等术语解释)
  - [1.7 通道、像素、分辨率、尺寸、坐标说明](#17-通道像素分辨率尺寸坐标说明)
- [第 2 章：OpenCV 学习路线总览](#第-2-章opencv-学习路线总览)
- [第 3 章：环境安装与第一张图像](#第-3-章环境安装与第一张图像)
- [第 4 章：图像输入输出与窗口操作](#第-4-章图像输入输出与窗口操作)
- [第 5 章：图像基本属性、像素访问与修改](#第-5-章图像基本属性像素访问与修改)
- [第 6 章：ROI 感兴趣区域、裁剪与复制](#第-6-章roi-感兴趣区域裁剪与复制)
- [第 7 章：图像颜色空间与通道处理](#第-7-章图像颜色空间与通道处理)
- [第 8 章：图像大小调整与几何变换](#第-8-章图像大小调整与几何变换)
- [第 9 章：图像阈值处理与二值化](#第-9-章图像阈值处理与二值化)
- [第 10 章：图像平滑滤波与去噪](#第-10-章图像平滑滤波与去噪)
- [第 11 章：边缘检测](#第-11-章边缘检测)
- [第 12 章：轮廓检测与形状分析](#第-12-章轮廓检测与形状分析)
- [第 13 章：形态学处理](#第-13-章形态学处理)
- [第 14 章：掩膜 Mask、位运算与区域提取](#第-14-章掩膜-mask位运算与区域提取)
- [第 15 章：图像算术运算、融合与增强](#第-15-章图像算术运算融合与增强)
- [第 16 章：直方图与对比度增强](#第-16-章直方图与对比度增强)
- [第 17 章：透视变换与文档矫正](#第-17-章透视变换与文档矫正)
- [第 18 章：常见图像格式与 Alpha 通道](#第-18-章常见图像格式与-alpha-通道)
- [第 19 章：OpenCV 图像处理常见任务流程图](#第-19-章opencv-图像处理常见任务流程图)
- [第 20 章：完整代码案例](#第-20-章完整代码案例)
- [第 21 章：学习建议与阶段目标](#第-21-章学习建议与阶段目标)

---

# 第 0 章：学习目标与建议

如果你是初学者，建议你把 OpenCV 图像处理理解成三层：

1. **基础操作层**  
   会读取、显示、保存图像，会查看属性、访问像素、裁剪图像。

2. **预处理层**  
   会灰度化、二值化、滤波、颜色转换、形态学处理。

3. **分析层**  
   会边缘检测、轮廓检测、提取目标、分析形状、定位区域。

学习时不要只背函数名，而要始终围绕下面 4 个问题：

- 输入是什么图？
- 这个函数做了什么？
- 输出会变成什么图？
- 输出结果下一步能拿来做什么？

---

# 第 1 章：图像处理与 OpenCV 基础常识

## 1.1 什么是图像处理

图像处理，就是使用计算机对图像进行分析、修改、增强、提取信息。

常见任务：

- 调整大小
- 裁剪区域
- 去噪
- 二值化
- 找边缘
- 找轮廓
- 检测颜色
- 分割目标
- 文档矫正
- 目标定位

---

## 1.2 什么是 OpenCV

OpenCV 全称：

**Open Source Computer Vision Library**

即：

**开源计算机视觉库**

它是一个非常常用的图像处理与计算机视觉工具库，支持：

- Python
- C++
- Java 等语言

在 Python 中最常见的用法是：

```python
import cv2
```

OpenCV 能做的事包括：

- 读写图片和视频
- 图像预处理
- 特征检测
- 边缘检测
- 轮廓分析
- 颜色提取
- 透视变换
- 视频目标跟踪
- 人脸检测
- 传统计算机视觉任务

---

## 1.3 为什么要学习 OpenCV 图像处理

因为很多视觉任务的基础，都是图像处理。

例如：

- OCR 文字识别前，需要先二值化、去噪
- 文档扫描前，需要先找边缘和轮廓
- 颜色检测前，需要先转 HSV
- 目标计数前，需要先做阈值、形态学、轮廓分析

所以图像处理是很多上层任务的“前处理基础”。

---

## 1.4 图像在计算机中本质是什么

图像本质上是一个 **数值矩阵**。

- 灰度图：二维矩阵
- 彩色图：三维矩阵

例如：

```python
image.shape
```

可能得到：

```python
(480, 640, 3)
```

意思是：

- 高度 480
- 宽度 640
- 3 个通道

也就是 480 行、640 列、每个位置有 3 个数值。

---

## 1.5 常见图像类型

### 1）灰度图

只有一个通道，每个像素是一个亮度值：

- `0`：黑色
- `255`：白色
- 中间值：灰色

### 2）彩色图

常见是 3 通道图像。

在 OpenCV 中默认是：

- `B`：Blue 蓝
- `G`：Green 绿
- `R`：Red 红

### 3）二值图

通常只有两个值：

- `0`
- `255`

常用于：

- 阈值处理
- 轮廓检测
- 形态学处理
- mask 掩膜

### 4）带透明通道图像

有 4 个通道：

- B
- G
- R
- A（Alpha 透明度）

---

## 1.6 前景、背景、目标、噪声等术语解释

### 前景 foreground

你关心的区域或物体。

例如：

- 硬币图中的硬币
- 文档图中的纸张
- 颜色检测中的蓝色物体

### 背景 background

你不关心的部分。

例如：

- 硬币下面的桌面
- 文档四周的桌子
- 物体后面的墙面

### 目标 object / target

通常和前景类似，指需要识别、提取或分析的对象。

### 噪声 noise

图像中不希望出现的杂乱信息。

例如：

- 采集时产生的小点
- 压缩产生的杂色
- 光照不均造成的干扰

### 边缘 edge

图像中亮度或颜色变化明显的位置。

### 轮廓 contour

物体边界点组成的一条闭合或近似闭合曲线。

---

## 1.7 通道、像素、分辨率、尺寸、坐标说明

### 像素 pixel

图像中最小的单元。

### 通道 channel

表示一类颜色信息。

例如：

- 灰度图：1 个通道
- BGR 图：3 个通道
- BGRA 图：4 个通道

### 分辨率 resolution

通常指图像宽高，例如：

- `1920 × 1080`
- `640 × 480`

### 尺寸 size / shape

在 OpenCV 里常看：

```python
image.shape
```

### 坐标说明

访问像素时：

```python
image[y, x]
```

不是：

```python
image[x, y]
```

因为 NumPy 数组是：

- 先行（y）
- 后列（x）

---

# 第 2 章：OpenCV 学习路线总览

建议按下面顺序学习：

1. 图像读写与显示
2. 图像属性、像素访问与 ROI
3. 颜色空间与通道
4. 图像裁剪、缩放、旋转、平移、翻转
5. 阈值与二值图
6. 平滑滤波与去噪
7. 边缘检测
8. 形态学处理
9. 轮廓检测
10. 掩膜与位运算
11. 图像增强与融合
12. 直方图与均衡化
13. 透视变换
14. 综合实战

这条路线符合初学者由浅入深的学习方式。

---

# 第 3 章：环境安装与第一张图像

## 3.1 安装

```bash
pip install opencv-python
```

如果需要额外功能：

```bash
pip install opencv-contrib-python
```

---

## 3.2 第一张图像代码

```python
import cv2

image = cv2.imread('image.jpg')

cv2.imshow('Image', image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

## 3.3 解释

- `cv2.imread()`：读取图像
- `cv2.imshow()`：显示图像
- `cv2.waitKey(0)`：等待按键
- `cv2.destroyAllWindows()`：关闭所有窗口

---

# 第 4 章：图像输入输出与窗口操作

## 4.1 读取图像：`cv2.imread()`

### 语法

```python
image = cv2.imread(filename, flags)
```

### 常用参数

| 参数       | 说明     |
| ---------- | -------- |
| `filename` | 图像路径 |
| `flags`    | 读取方式 |

### 常用 flags

| 参数                   | 说明                     |
| ---------------------- | ------------------------ |
| `cv2.IMREAD_COLOR`     | 读取彩色图，默认         |
| `cv2.IMREAD_GRAYSCALE` | 读取灰度图               |
| `cv2.IMREAD_UNCHANGED` | 保留所有通道，包括透明度 |

### 返回值

返回 NumPy 数组，读取失败返回 `None`。

### 注意点

OpenCV 默认彩色图顺序是 **BGR**，不是 RGB。

---

## 4.2 显示图像：`cv2.imshow()`

```python
cv2.imshow(window_name, image)
```

| 参数          | 说明         |
| ------------- | ------------ |
| `window_name` | 窗口名       |
| `image`       | 要显示的图像 |

---

## 4.3 按键等待：`cv2.waitKey()`

```python
key = cv2.waitKey(delay)
```

- `0`：无限等待
- `1`：等待 1ms，常用于视频

返回值通常可配合：

```python
if key == ord('q'):
    ...
```

---

## 4.4 保存图像：`cv2.imwrite()`

```python
success = cv2.imwrite('result.jpg', image)
```

返回：

- `True`：成功
- `False`：失败

---

## 4.5 示例代码

```python
import cv2

image = cv2.imread('image.jpg')

if image is None:
    print('读取失败')
    exit()

cv2.imshow('Original', image)

key = cv2.waitKey(0)

if key == ord('s'):
    cv2.imwrite('saved.jpg', image)
    print('保存成功')

cv2.destroyAllWindows()
```

---

# 第 5 章：图像基本属性、像素访问与修改

## 5.1 图像属性

```python
print(image.shape)
print(image.dtype)
print(image.size)
```

### 含义

| 属性    | 说明                   |
| ------- | ---------------------- |
| `shape` | 图像高、宽、通道       |
| `dtype` | 数据类型，常见 `uint8` |
| `size`  | 总元素数               |

---

## 5.2 访问像素值

### 灰度图

```python
value = gray[y, x]
```

### 彩色图

```python
b, g, r = image[y, x]
```

---

## 5.3 修改像素值

```python
image[y, x] = [0, 0, 255]
```

表示修改为红色。

OpenCV 中BGR：

- `[255, 0, 0]` 蓝
- `[0, 255, 0]` 绿
- `[0, 0, 255]` 红

---

## 5.4 批量修改区域

```python
image[0:100, 0:100] = [0, 255, 0]
```

表示把左上角区域全部改成绿色。

---

## 5.5 示例代码

```python
import cv2

image = cv2.imread('image.jpg')

print('shape =', image.shape)
print('dtype =', image.dtype)
print('size =', image.size)

image[50, 50] = [0, 0, 255]
image[0:100, 0:100] = [0, 255, 0]

cv2.imshow('Modified', image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 6 章：ROI 感兴趣区域、裁剪与复制

## 6.1 ROI 是什么

ROI = Region of Interest，感兴趣区域。

意思是：

你不处理整张图，只处理其中一部分。

---

## 6.2 获取 ROI

```python
roi = image[y1:y2, x1:x2]
```

注意顺序：

- 先 y
- 后 x

---

## 6.3 裁剪图像

裁剪本质上就是取 ROI。

```python
crop = image[100:300, 200:400]
```

---

## 6.4 ROI 修改

```python
image[100:300, 200:400] = [255, 255, 255]
```

---

## 6.5 ROI 复制粘贴

```python
                                                                                                                                                                           
```

两个区域尺寸必须一致。

---

## 6.6 示例代码

```python
import cv2

image = cv2.imread('image.jpg')

roi = image[100:250, 100:250]
image[300:450, 300:450] = roi

cv2.imshow('ROI', roi)
cv2.imshow('Result', image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 7 章：图像颜色空间与通道处理

## 7.1 BGR、RGB、GRAY、HSV 说明

### BGR

OpenCV 默认彩色顺序：

- Blue
- Green
- Red

### RGB

更通用的颜色顺序：

- Red
- Green
- Blue

### GRAY 灰度图

只有一个通道，表示亮度。

### HSV

- `H`：Hue 色相
- `S`：Saturation 饱和度
- `V`：Value 亮度

HSV 常用于颜色识别。

---

## 7.2 颜色空间转换：`cv2.cvtColor()`

```python
result = cv2.cvtColor(image, code)
```

### 常用 code

| 代码                 | 说明       |
| -------------------- | ---------- |
| `cv2.COLOR_BGR2GRAY` | BGR 转灰度 |
| `cv2.COLOR_BGR2RGB`  | BGR 转 RGB |
| `cv2.COLOR_BGR2HSV`  | BGR 转 HSV |
| `cv2.COLOR_HSV2BGR`  | HSV 转 BGR |
| `cv2.COLOR_GRAY2BGR` | 灰度转 BGR |

---

## 7.3 通道拆分与合并

### 拆分

```python
b, g, r = cv2.split(image)
```

### 合并

```python
merged = cv2.merge([b, g, r])
```

---

## 7.4 示例代码

```python
import cv2

image = cv2.imread('image.jpg')

gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
b, g, r = cv2.split(image)

cv2.imshow('BGR', image)
cv2.imshow('Gray', gray)
cv2.imshow('HSV', hsv)
cv2.imshow('Blue Channel', b)
cv2.imshow('Green Channel', g)
cv2.imshow('Red Channel', r)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 8 章：图像大小调整与几何变换

## 8.1 resize 缩放：`cv2.resize()`

```python
resized = cv2.resize(src, dsize, fx=None, fy=None, interpolation=cv2.INTER_LINEAR)
```

### 参数说明

| 参数            | 说明                       |
| --------------- | -------------------------- |
| `src`           | 输入图像                   |
| `dsize`         | 目标尺寸 `(width, height)` |
| `fx`            | 水平方向缩放倍数           |
| `fy`            | 垂直方向缩放倍数           |
| `interpolation` | 插值方式                   |

### 常见插值

| 插值方式            | 用途           |
| ------------------- | -------------- |
| `cv2.INTER_NEAREST` | 最近邻，快     |
| `cv2.INTER_LINEAR`  | 默认，常用     |
| `cv2.INTER_AREA`    | 缩小时效果好   |
| `cv2.INTER_CUBIC`   | 放大时效果较好 |
| `cv2.INTER_NEAREST` | mask           |

---



### 插值 interpolation 详解

在 `cv2.resize()` 中，`interpolation` 表示**插值方式**。

图像缩放时，新的图像尺寸和原图尺寸不一样，因此新图中的像素点不一定能在原图中找到完全对应的位置。

这时就需要通过某种方法，根据原图已有像素值，计算出新图像中的像素值。

这个过程就叫：

> 插值。

简单理解：

```text
插值 = 根据已有像素，估算新像素
```

图像本质是由像素组成的矩阵。

例如原图大小是：

```
4 × 4
```



如果把它放大成：

```
8 × 8
```



原图中原本只有 16 个像素，现在新图需要 64 个像素。

多出来的像素原图里并不存在，所以 OpenCV 需要根据周围已有像素计算出这些新像素。

这个计算过程就是插值。

------

缩小图像时也需要插值。

例如原图大小是：

```
800 × 800
```



缩小成：

```
200 × 200
```



原图中大量像素需要被压缩合并。

此时新图中的一个像素可能代表原图中的一片区域，因此也需要通过插值方式决定这个像素值。

所以：





```
放大图像：需要生成新像素
缩小图像：需要合并或重新采样像素
```

#### 一个简单例子

假设一行灰度像素是：

```
10        20
```



现在要在它们中间生成一个新像素：

```
10    ?    20
```



这个 `?` 应该是多少？

一种简单方法是取平均值：

```
(10 + 20) / 2 = 15
```



于是得到：

```
10    15    20
```



这就是插值的基本思想。

#### 彩色图像中的插值

灰度图中，一个像素只有一个值：

```
128
```



彩色图中，一个像素有三个通道：

```
[B, G, R]
```



例如两个像素分别是：





```
pixel1 = [10, 50, 100]
pixel2 = [30, 70, 200]
```

如果要计算它们中间的新像素，可以分别对 B、G、R 三个通道计算：





```
B = (10 + 30) / 2 = 20
G = (50 + 70) / 2 = 60
R = (100 + 200) / 2 = 150
```

得到：

```
[20, 60, 150]
```



所以彩色图像插值时，本质上是分别对每个颜色通道进行计算。

### 

## 8.2 平移：`cv2.warpAffine()`

```python
M = np.float32([
    [1, 0, tx],
    [0, 1, ty]
])
translated = cv2.warpAffine(image, M, (w, h))
```

---

## 8.3 旋转：`cv2.getRotationMatrix2D()`

```python
M = cv2.getRotationMatrix2D(center, angle, scale)
rotated = cv2.warpAffine(image, M, (w, h))
```

---

## 8.4 翻转：`cv2.flip()`

```python
flipped = cv2.flip(image, flipCode)
```

| flipCode | 说明     |
| -------- | -------- |
| `0`      | 上下翻转 |
| `1`      | 左右翻转 |
| `-1`     | 同时翻转 |

---

## 8.5 示例代码

```python
import cv2
import numpy as np

image = cv2.imread('image.jpg')
h, w = image.shape[:2]

resized = cv2.resize(image, (w // 2, h // 2))

M_translate = np.float32([[1, 0, 100], [0, 1, 50]])
translated = cv2.warpAffine(image, M_translate, (w, h))

center = (w // 2, h // 2)
M_rotate = cv2.getRotationMatrix2D(center, 45, 1.0)
rotated = cv2.warpAffine(image, M_rotate, (w, h))

flipped = cv2.flip(image, 1)

cv2.imshow('Original', image)
cv2.imshow('Resized', resized)
cv2.imshow('Translated', translated)
cv2.imshow('Rotated', rotated)
cv2.imshow('Flipped', flipped)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 9 章：图像阈值处理与二值化

## 9.1 什么是阈值

阈值处理是根据某个阈值，把图像变成两类或多类。

最常见的是二值化。

---

## 9.2 基本阈值：`cv2.threshold()`

```python
ret, binary = cv2.threshold(src, thresh, maxval, type)
```

### 参数说明

| 参数     | 说明                                             |
| -------- | ------------------------------------------------ |
| `src`    | 输入灰度图                                       |
| `thresh` | 阈值                                             |
| `maxval` | 当像素满足阈值条件时赋予的值，二值化时常用 `255` |
| `type`   | 阈值类型                                         |

### 返回值

| 返回值   | 说明         |
| -------- | ------------ |
| `ret`    | 实际使用阈值 |
| `binary` | 输出图像     |

### 常见 type

| 类型                    | 说明                                                         |
| ----------------------- | ------------------------------------------------------------ |
| `cv2.THRESH_BINARY`     | 如果像素值大于阈值，则赋予 `maxval`，否则赋予 `0`。          |
| `cv2.THRESH_BINARY_INV` | 与 `cv2.THRESH_BINARY` 相反，如果像素值大于阈值，则赋予 `0`，否则赋予 `maxval`。 |
| `cv2.THRESH_TRUNC`      | 如果像素值大于阈值，则赋予阈值，否则保持不变。               |
| `cv2.THRESH_TOZERO`     | 如果像素值大于阈值，则保持不变，否则赋予 `0`。               |
| `v2.THRESH_TOZERO_INV`  | 与 `cv2.THRESH_TOZERO` 相反，如果像素值大于阈值，则赋予 `0`，否则保持不变。 |
| `cv2.THRESH_OTSU`       | 常应用于前景和背景灰度差异明显的图像，自动阈值               |

```
ret, binary = cv2.threshold(
    gray,
    0,
    255,
    cv2.THRESH_BINARY + cv2.THRESH_OTSU
)
```

这里虽然 `thresh` 写的是 `0`，但实际不会用这个 0。

解释：cv2.THRESH_BINARY + cv2.THRESH_OTSU组合用法

不是表示“执行两个阈值类型”，而是表示：

> 使用 `THRESH_BINARY` 这种二值化规则，同时让 OTSU 自动计算阈值。

也就是说：

```
THRESH_BINARY      决定：超过阈值后怎么赋值
THRESH_OTSU        决定：阈值由算法自动计算
```

它们不是同一类含义。

------



---

## 9.3 自适应阈值：`cv2.adaptiveThreshold()`

适合光照不均匀场景。

```python
binary = cv2.adaptiveThreshold(
    gray,
    255,
    cv2.ADAPTIVE_THRESH_GAUSSIAN_C,
    cv2.THRESH_BINARY,
    11,
    2
)
```

---

## 9.4 示例代码

```python
import cv2

image = cv2.imread('image.jpg')
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

ret, binary = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)
ret2, binary_inv = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY_INV)

adaptive = cv2.adaptiveThreshold(
    gray,
    255,
    cv2.ADAPTIVE_THRESH_GAUSSIAN_C,
    cv2.THRESH_BINARY,
    11,
    2
)

cv2.imshow('Gray', gray)
cv2.imshow('Binary', binary)
cv2.imshow('Binary Inv', binary_inv)
cv2.imshow('Adaptive', adaptive)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

`THRESH_OTSU` 和 `adaptiveThreshold()` 都能自动确定阈值，但不一样：

> `THRESH_OTSU` 是根据整张图自动找一个全局阈值；
> `adaptiveThreshold()` 是根据每个像素周围的小区域自动计算局部阈值。

---

# 第 10 章：图像平滑滤波与去噪

## 10.1 为什么要滤波

图像中常常有噪声。滤波的作用：

- 去噪
- 平滑图像
- 为边缘检测做预处理
- 让阈值和轮廓更稳定

---

## 10.2 均值滤波：`cv2.blur()`

```python
blurred = cv2.blur(image, (5, 5))
```

---

## 10.3 高斯滤波：`cv2.GaussianBlur()`

```python
gaussian = cv2.GaussianBlur(image, (5, 5), 0)
```

最常用，边缘检测前经常先做高斯模糊。

---

## 10.4 中值滤波：`cv2.medianBlur()`

```python
median = cv2.medianBlur(image, 5)
```

适合去椒盐噪声。

---

## 10.5 双边滤波：`cv2.bilateralFilter()`

```python
bilateral = cv2.bilateralFilter(image, 9, 75, 75)
```

平滑时尽量保留边缘。

---

## 10.6 示例代码

```python
import cv2

image = cv2.imread('image.jpg')

mean_blur = cv2.blur(image, (5, 5))
gaussian = cv2.GaussianBlur(image, (5, 5), 0)
median = cv2.medianBlur(image, 5)
bilateral = cv2.bilateralFilter(image, 9, 75, 75)

cv2.imshow('Original', image)
cv2.imshow('Mean Blur', mean_blur)
cv2.imshow('Gaussian', gaussian)
cv2.imshow('Median', median)
cv2.imshow('Bilateral', bilateral)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 11 章：边缘检测

## 11.1 什么是边缘

边缘是图像中灰度或颜色变化明显的位置。

例如：

- 物体和背景交界处
- 文字与纸张交界处
- 圆和背景之间的分界线

---

## 11.2 Canny 边缘检测：`cv2.Canny()`

```python
edges = cv2.Canny(image, threshold1, threshold2)
```

### 参数

| 参数         | 说明                 |
| ------------ | -------------------- |
| `image`      | 输入图像，通常灰度图 |
| `threshold1` | 低阈值               |
| `threshold2` | 高阈值               |

### 返回值

输出一张边缘图：

- 黑色：非边缘
- 白色：边缘

高于`threshold2`一定是边缘，低于`threshold1`一定不是边缘，再中间若是比较强变换的属于边缘

---

## 11.3 Sobel：`cv2.Sobel()`

```python
sobel_x = cv2.Sobel(gray, cv2.CV_64F, 1, 0, ksize=3)
sobel_y = cv2.Sobel(gray, cv2.CV_64F, 0, 1, ksize=3)
```

适合计算 x 或 y 方向的梯度变化。

---

## 11.4 Laplacian：`cv2.Laplacian()`

```python
lap = cv2.Laplacian(gray, cv2.CV_64F)
```

对噪声敏感，通常先模糊。

---

## 11.5 示例代码

```python
import cv2

image = cv2.imread('image.jpg')
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
blur = cv2.GaussianBlur(gray, (5, 5), 0)

edges = cv2.Canny(blur, 100, 200)

cv2.imshow('Original', image)
cv2.imshow('Gray', gray)
cv2.imshow('Edges', edges)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 12 章：轮廓检测与形状分析

## 12.1 什么是轮廓

轮廓是物体边界点组成的集合。

边缘检测得到的是“哪里像边缘”，  
轮廓检测得到的是“这一圈边界属于哪个物体”。

---

## 12.2 查找轮廓：`cv2.findContours()`

```python
contours, hierarchy = cv2.findContours(image, mode, method)
```

### 参数

| 参数     | 说明                         |
| -------- | ---------------------------- |
| `image`  | 输入图像，通常二值图或边缘图 |
| `mode`   | 轮廓检索模式                 |
| `method` | 轮廓近似方法                 |

### 常见 mode

| 模式                | 说明                 |
| ------------------- | -------------------- |
| `cv2.RETR_EXTERNAL` | 只找外轮廓           |
| `cv2.RETR_LIST`     | 找全部轮廓，不建层级 |
| `cv2.RETR_TREE`     | 找全部轮廓，建层级   |

### 常见 method

| 方法                      | 说明         |
| ------------------------- | ------------ |
| `cv2.CHAIN_APPROX_SIMPLE` | 压缩点，常用 |
| `cv2.CHAIN_APPROX_NONE`   | 保存所有点   |

---

## 12.3 画轮廓：`cv2.drawContours()`

```python
cv2.drawContours(image, contours, -1, (0, 255, 0), 2)
```

---

## 12.4 面积：`cv2.contourArea()`

```python
area = cv2.contourArea(contour)
```

常用于过滤小噪声。

---

## 12.5 周长：`cv2.arcLength()`

```python
perimeter = cv2.arcLength(contour, True)
```

---

## 12.6 外接矩形：`cv2.boundingRect()`

```python
x, y, w, h = cv2.boundingRect(contour)
```

---

## 12.7 多边形近似：`cv2.approxPolyDP()`

```python
approx = cv2.approxPolyDP(contour, epsilon, True)
```

常用：

```python
epsilon = 0.02 * cv2.arcLength(contour, True)
```

可用于形状判断：

- 3 个点：三角形
- 4 个点：矩形/正方形
- 多个点：圆形或多边形

---

## 12.8 示例代码

```python
import cv2

image = cv2.imread('image.jpg')
output = image.copy()

gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
blur = cv2.GaussianBlur(gray, (5, 5), 0)
edges = cv2.Canny(blur, 100, 200)

contours, _ = cv2.findContours(
    edges,
    cv2.RETR_EXTERNAL,
    cv2.CHAIN_APPROX_SIMPLE
)

for contour in contours:
    area = cv2.contourArea(contour)
    if area < 100:
        continue

    cv2.drawContours(output, [contour], -1, (0, 255, 0), 2)

    x, y, w, h = cv2.boundingRect(contour)
    cv2.rectangle(output, (x, y), (x + w, y + h), (255, 0, 0), 2)

cv2.imshow('Edges', edges)
cv2.imshow('Contours', output)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 13 章：形态学处理

## 13.1 形态学是什么

形态学处理主要用于处理前景区域的形状。

多用于：

- 去噪
- 连接断裂区域
- 填补小孔洞
- 分离粘连物体
- 提取边界

---

## 13.2 结构元素 kernel

很多形态学函数都需要一个 kernel：

```python
import numpy as np
kernel = np.ones((3, 3), np.uint8)
```

它可以理解成“操作窗口”。

---

## 13.3 腐蚀：`cv2.erode()`

```python
eroded = cv2.erode(binary, kernel, iterations=1)
```

作用：

- 白色区域变小
- 去掉小白点
- 将重合的部分去除

腐蚀的规则可以简单理解为：

> kernel 覆盖的区域内，只要有一个黑色 0，当前中心像素就变成黑色 0。
> 只有 kernel 覆盖区域全是白色 255，中心像素才保持白色 255。

---

## 13.4 膨胀：`cv2.dilate()`

```python
dilated = cv2.dilate(binary, kernel, iterations=1)
```

作用：

- 白色区域变大
- 连接断裂部分

膨胀的规则正好相反：

> kernel 覆盖的区域内，只要有一个白色 255，当前中心像素就变成白色 255。
> 只有 kernel 覆盖区域全是黑色 0，中心像素才是黑色 0。

---

## 13.5 开运算

```python
opened = cv2.morphologyEx(binary, cv2.MORPH_OPEN, kernel)
```

等价于：

- 先腐蚀
- 后膨胀

作用：

- 去除小白噪声

---

## 13.6 闭运算

```python
closed = cv2.morphologyEx(binary, cv2.MORPH_CLOSE, kernel)
```

等价于：

- 先膨胀
- 后腐蚀

作用：

- 填补小黑洞
- 连接小断裂

---

## 13.7 形态学梯度

```python
gradient = cv2.morphologyEx(binary, cv2.MORPH_GRADIENT, kernel)
```

相当于：

- 膨胀 - 腐蚀

作用：

- 提取边界

---

## 13.8 示例代码

```python
import cv2
import numpy as np

image = cv2.imread('image.jpg', cv2.IMREAD_GRAYSCALE)
_, binary = cv2.threshold(image, 127, 255, cv2.THRESH_BINARY)

kernel = np.ones((3, 3), np.uint8)

eroded = cv2.erode(binary, kernel, iterations=1)
dilated = cv2.dilate(binary, kernel, iterations=1)
opened = cv2.morphologyEx(binary, cv2.MORPH_OPEN, kernel)
closed = cv2.morphologyEx(binary, cv2.MORPH_CLOSE, kernel)
gradient = cv2.morphologyEx(binary, cv2.MORPH_GRADIENT, kernel)

cv2.imshow('Binary', binary)
cv2.imshow('Eroded', eroded)
cv2.imshow('Dilated', dilated)
cv2.imshow('Opened', opened)
cv2.imshow('Closed', closed)
cv2.imshow('Gradient', gradient)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 14 章：掩膜 Mask、位运算与区域提取

## 14.1 什么是 mask

mask 是一张用于“控制区域”的图。

常见语义：

- 白色区域：保留
- 黑色区域：去掉

---

## 14.2 颜色范围掩膜：`cv2.inRange()`

```python
mask = cv2.inRange(hsv, lower, upper)
```

作用：

把在颜色范围内的像素设为白色，其他设为黑色。

---

## 14.3 位运算函数

### 与运算

```python
result = cv2.bitwise_and(image, image, mask=mask)
```

作用：

只保留 mask 白色区域对应的原图内容。

### 或运算

```python
cv2.bitwise_or()
```

### 非运算

```python
cv2.bitwise_not()
```

### 异或运算

```python
cv2.bitwise_xor()
```

---

## 14.4 示例代码：提取蓝色区域

```python
import cv2
import numpy as np

image = cv2.imread('image.jpg')
hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)

lower_blue = np.array([100, 80, 80])
upper_blue = np.array([130, 255, 255])

mask = cv2.inRange(hsv, lower_blue, upper_blue)
result = cv2.bitwise_and(image, image, mask=mask)

cv2.imshow('Original', image)
cv2.imshow('Mask', mask)
cv2.imshow('Result', result)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 15 章：图像算术运算、融合与增强

## 15.1 加法：`cv2.add()`

```python
result = cv2.add(image1, image2)
```

比直接 NumPy 相加更安全，不容易溢出。

---

## 15.2 融合：`cv2.addWeighted()`

```python
result = cv2.addWeighted(image1, 0.7, image2, 0.3, 0)
```

解释：

- `image1` 权重 0.7
- `image2` 权重 0.3
- `0` 是偏移值

常用于：

- 图片叠加
- 半透明效果
- 水印合成

---

## 15.3 示例代码

```python
import cv2

image1 = cv2.imread('image1.jpg')
image2 = cv2.imread('image2.jpg')

image2 = cv2.resize(image2, (image1.shape[1], image1.shape[0]))
blend = cv2.addWeighted(image1, 0.7, image2, 0.3, 0)

cv2.imshow('Image1', image1)
cv2.imshow('Image2', image2)
cv2.imshow('Blend', blend)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 16 章：直方图与对比度增强

## 16.1 什么是直方图

直方图统计某个灰度值出现了多少次。

可以帮助你理解：

- 图像整体偏暗还是偏亮
- 对比度高不高
- 阈值该怎么选

---

## 16.2 计算灰度直方图

```python
hist = cv2.calcHist([gray], [0], None, [256], [0, 256])
```

| 参数      | 说明              |
| --------- | ----------------- |
| `[gray]`  | 输入图像列表      |
| `[0]`     | 通道索引          |
| `None`    | mask              |
| `[256]`   | 统计 256 个灰度级 |
| `[0,256]` | 像素范围          |

---

## 

---

## 16.3 直方图均衡化：`cv2.equalizeHist()`

```python
equalized = cv2.equalizeHist(gray)
```

作用：

增强灰度图对比度。

---

## 16.4 示例代码

```python
import cv2

image = cv2.imread('image.jpg', cv2.IMREAD_GRAYSCALE)
equalized = cv2.equalizeHist(image)

cv2.imshow('Gray', image)
cv2.imshow('Equalized', equalized)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 17 章：透视变换与文档矫正

## 17.1 透视变换是什么

透视变换可以把倾斜的平面图像“拉正”。

适合：

- 文档扫描
- 纸张校正
- 车牌矫正
- 平面目标矫正

---

## 17.2 核心函数

### 透视矩阵

```python
M = cv2.getPerspectiveTransform(src_pts, dst_pts)
```

### 执行变换

```python
result = cv2.warpPerspective(image, M, (width, height))
```

---

## 17.3 示例代码

```python
import cv2
import numpy as np

image = cv2.imread('document.jpg')

src_pts = np.float32([
    [100, 100],
    [500, 80],
    [520, 700],
    [80, 720]
])

dst_pts = np.float32([
    [0, 0],
    [400, 0],
    [400, 600],
    [0, 600]
])

M = cv2.getPerspectiveTransform(src_pts, dst_pts)
result = cv2.warpPerspective(image, M, (400, 600))

cv2.imshow('Original', image)
cv2.imshow('Perspective', result)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 第 18 章：常见图像格式与 Alpha 通道

## 18.1 JPG / JPEG

特点：

- 有损压缩
- 文件小
- 不支持透明通道Alpha
- 适合照片

---

## 18.2 PNG

特点：

- 无损压缩
- 支持透明通道 Alpha
- 文件通常比 JPG 大
- 适合图标、截图、透明背景图

---

## 18.3 BMP

特点：

- 基本不压缩
- 文件较大
- 图像数据保留完整

---

## 18.4 Alpha 是什么

Alpha 表示透明度。

常见范围：

- `0`：完全透明
- `255`：完全不透明

如果图片有 Alpha 通道，通常是 4 通道图像。

读取方式：

```python
image = cv2.imread('image.png', cv2.IMREAD_UNCHANGED)
```

---

# 第 19 章：OpenCV 图像处理常见任务流程图

## 19.1 轮廓检测任务流程

```text
原图
 ↓
灰度化
 ↓
高斯模糊
 ↓
阈值处理 / Canny
 ↓
形态学去噪
 ↓
findContours
 ↓
面积过滤
 ↓
画轮廓 / 画框 / 判断形状
```

---

## 19.2 颜色检测流程

```text
原图
 ↓
BGR 转 HSV
 ↓
设置颜色范围
 ↓
inRange 生成 mask
 ↓
形态学去噪
 ↓
bitwise_and 提取目标
 ↓
轮廓分析
```

---

## 19.3 文档扫描流程

```text
原图
 ↓
灰度化
 ↓
高斯模糊
 ↓
Canny 边缘检测
 ↓
寻找最大轮廓
 ↓
多边形近似找四角
 ↓
透视变换
 ↓
得到扫描结果
```

---

## 19.4 图像分割基础流程

```text
原图
 ↓
灰度化 / HSV 转换
 ↓
阈值或颜色范围提取
 ↓
形态学处理
 ↓
mask / 轮廓
 ↓
目标区域提取
```

---

# 第 20 章：完整代码案例

## 案例 1：基础图像读写与属性查看

```python
import cv2

image = cv2.imread('image.jpg')

if image is None:
    print('图像读取失败')
    exit()

print('shape:', image.shape)
print('dtype:', image.dtype)
print('size:', image.size)

cv2.imshow('Original', image)

key = cv2.waitKey(0)
if key == ord('s'):
    cv2.imwrite('saved.jpg', image)
    print('保存成功')

cv2.destroyAllWindows()
```

---

## 案例 2：ROI 裁剪、复制与修改

```python
import cv2

image = cv2.imread('image.jpg')

roi = image[100:250, 100:250]
image[300:450, 300:450] = roi
image[0:50, 0:200] = [0, 255, 0]

cv2.imshow('ROI', roi)
cv2.imshow('Result', image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

## 案例 3：颜色空间转换与通道显示

```python
import cv2

image = cv2.imread('image.jpg')

gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
b, g, r = cv2.split(image)

cv2.imshow('Original', image)
cv2.imshow('Gray', gray)
cv2.imshow('HSV', hsv)
cv2.imshow('B', b)
cv2.imshow('G', g)
cv2.imshow('R', r)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

## 案例 4：缩放、平移、旋转、翻转

```python
import cv2
import numpy as np

image = cv2.imread('image.jpg')
h, w = image.shape[:2]

resized = cv2.resize(image, (w // 2, h // 2))

M_translate = np.float32([[1, 0, 80], [0, 1, 40]])
translated = cv2.warpAffine(image, M_translate, (w, h))

M_rotate = cv2.getRotationMatrix2D((w // 2, h // 2), 30, 1.0)
rotated = cv2.warpAffine(image, M_rotate, (w, h))

flipped = cv2.flip(image, 1)

cv2.imshow('Original', image)
cv2.imshow('Resized', resized)
cv2.imshow('Translated', translated)
cv2.imshow('Rotated', rotated)
cv2.imshow('Flipped', flipped)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

## 案例 5：阈值 + 滤波 + 边缘检测

```python
import cv2

image = cv2.imread('image.jpg')
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

gaussian = cv2.GaussianBlur(gray, (5, 5), 0)
ret, binary = cv2.threshold(gaussian, 127, 255, cv2.THRESH_BINARY)
edges = cv2.Canny(gaussian, 100, 200)

cv2.imshow('Gray', gray)
cv2.imshow('Gaussian', gaussian)
cv2.imshow('Binary', binary)
cv2.imshow('Edges', edges)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

## 案例 6：轮廓检测与外接矩形

```python
import cv2

image = cv2.imread('image.jpg')
output = image.copy()

gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
blur = cv2.GaussianBlur(gray, (5, 5), 0)
edges = cv2.Canny(blur, 100, 200)

contours, _ = cv2.findContours(
    edges,
    cv2.RETR_EXTERNAL,
    cv2.CHAIN_APPROX_SIMPLE
)

for contour in contours:
    area = cv2.contourArea(contour)
    if area < 100:
        continue

    cv2.drawContours(output, [contour], -1, (0, 255, 0), 2)

    x, y, w, h = cv2.boundingRect(contour)
    cv2.rectangle(output, (x, y), (x + w, y + h), (255, 0, 0), 2)

cv2.imshow('Edges', edges)
cv2.imshow('Contours', output)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

## 案例 7：使用 HSV + Mask 提取颜色目标

```python
import cv2
import numpy as np

image = cv2.imread('image.jpg')
hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)

lower_blue = np.array([100, 80, 80])
upper_blue = np.array([130, 255, 255])

mask = cv2.inRange(hsv, lower_blue, upper_blue)
result = cv2.bitwise_and(image, image, mask=mask)

cv2.imshow('Original', image)
cv2.imshow('Mask', mask)
cv2.imshow('Result', result)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

## 案例 8：形态学去噪和边界提取

```python
import cv2
import numpy as np

gray = cv2.imread('image.jpg', cv2.IMREAD_GRAYSCALE)
_, binary = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)

kernel = np.ones((3, 3), np.uint8)

opened = cv2.morphologyEx(binary, cv2.MORPH_OPEN, kernel)
closed = cv2.morphologyEx(binary, cv2.MORPH_CLOSE, kernel)
gradient = cv2.morphologyEx(binary, cv2.MORPH_GRADIENT, kernel)

cv2.imshow('Binary', binary)
cv2.imshow('Opened', opened)
cv2.imshow('Closed', closed)
cv2.imshow('Gradient', gradient)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

## 案例 9：图像融合

```python
import cv2

image1 = cv2.imread('image1.jpg')
image2 = cv2.imread('image2.jpg')

image2 = cv2.resize(image2, (image1.shape[1], image1.shape[0]))
blend = cv2.addWeighted(image1, 0.7, image2, 0.3, 0)

cv2.imshow('Blend', blend)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

## 案例 10：透视变换与文档矫正

```python
import cv2
import numpy as np

image = cv2.imread('document.jpg')

src_pts = np.float32([
    [100, 120],
    [500, 100],
    [530, 700],
    [80, 720]
])

dst_pts = np.float32([
    [0, 0],
    [400, 0],
    [400, 600],
    [0, 600]
])

M = cv2.getPerspectiveTransform(src_pts, dst_pts)
result = cv2.warpPerspective(image, M, (400, 600))

cv2.imshow('Original', image)
cv2.imshow('Corrected', result)
cv2.waitKey(0)
cv2.destroyAllWindows()
```



---

下面我给你做一份**比较完整、系统、能直接拿去学和写笔记的透视变换讲解**。  
我会按这个顺序讲：

1. 透视变换是什么  
2. 为什么需要它  
3. 应用场景  
4. 适用于灰度/二值/彩色吗  
5. 核心函数与参数解释  
6. 使用流程  
7. 代码示例  
8. 常见错误和注意事项  
9. 和仿射变换的区别  
10. 最后做总结

---

# 1. 透视变换是什么

先一句最核心的话：

> **透视变换（Perspective Transform）就是把图像中“斜着拍到的平面区域”拉正成正视图。**

比如：

- 你用手机斜着拍一张纸
- 纸本来是矩形
- 但在照片里它看起来像一个歪四边形、梯形

透视变换就是把它重新校正成一个规则矩形。

---

## 1.1 为什么会有“歪”

因为相机拍照会产生**透视效果**：

```text
近大远小
```

所以现实中明明是平行的边，拍到图像里可能看起来会收拢。

例如一张纸本来是：

```text
+---------+
|         |
|         |
+---------+
```

斜着拍后可能变成：

```text
   +------+
  /      /
 /      /
+------+
```

这不是纸变形了，而是**观察角度**导致的成像变化。

透视变换就是把这种因为视角产生的形变校正掉。

---

# 2. 透视变换的作用目的

透视变换不是为了“让图更好看”这么简单，它通常有非常明确的处理目标。

它的主要目的有：

## 

```text
歪四边形 -> 标准矩形
```

---



---



---



---

- 



你问得非常关键：

> 适用对象是灰度、二值还是彩色？

答案是：

> **透视变换本身对图像类型没有限制。灰度图、二值图、彩色图都可以做。**

---

## 4.1 彩色图可以做

最常见。

```python
image = cv2.imread('document.jpg')
result = cv2.warpPerspective(image, M, (w, h))
```

此时 `image` 是三通道 BGR 彩色图，完全没问题。

---

## 4.2 灰度图可以做

也很常见。

```python
gray = cv2.imread('document.jpg', 0)
result = cv2.warpPerspective(gray, M, (w, h))
```

如果你只关心亮度信息，比如文档分析，灰度图就够了。

---

## 4.3 二值图也可以做

例如：

- 已二值化的文档
- 掩码 mask
- 分割结果

也能透视变换。

```python
binary = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)[1]
result = cv2.warpPerspective(binary, M, (w, h))
```

---



---



---

# 6. OpenCV 中核心函数

透视变换最核心的就是两个函数：

```python
cv2.getPerspectiveTransform()
cv2.warpPerspective()
```

---

# 7. `cv2.getPerspectiveTransform()` 详解

---

## 7.1 基本写法

```python
M = cv2.getPerspectiveTransform(src_pts, dst_pts)
```

---

## 7.2 作用

作用是：

> 根据原图四个点 `src_pts` 和目标图四个点 `dst_pts`，计算透视变换矩阵 `M`

---

## 7.3 参数解释

### 参数 1：`src_pts`

原图中的 4 个点。

比如原图中纸张的四个角：

```python
src_pts = np.float32([
    [100, 100],   # 左上
    [500, 80],    # 右上
    [520, 700],   # 右下
    [80, 720]     # 左下
])
```

---

### 参数 2：`dst_pts`

目标图中的 4 个点。

也就是你希望映射成什么样子。

例如你想输出一个 `400 × 600` 的标准矩形：

```python
dst_pts = np.float32([
    [0, 0],       # 左上
    [400, 0],     # 右上
    [400, 600],   # 右下
    [0, 600]      # 左下
])
```

---

## 7.4 返回值：`M`

返回的是一个 **3×3 透视变换矩阵**。

你可以把它理解成：

```text
描述“原图四边形如何拉到目标矩形”的规则
```

通常不需要手算这个矩阵，只需要会用。

---

# 8. `cv2.warpPerspective()` 详解

---

## 8.1 基本写法

```python
result = cv2.warpPerspective(image, M, (width, height))
```

---

## 8.2 作用

作用是：

> 按照透视矩阵 `M`，把输入图像 `image` 变换成输出图像

---

## 8.3 参数解释

### 参数 1：`image`

输入图像。

可以是：

- 彩色图
- 灰度图
- 二值图

---

### 参数 2：`M`

由 `cv2.getPerspectiveTransform()` 算出来的透视矩阵。

---

### 参数 3：`(width, height)`

输出图像大小。

注意这里是：

```python
(宽, 高)
```

不是 `(高, 宽)`。

例如：

```python
(400, 600)
```

表示输出图宽 400，高 600。

---

## 8.4 常见可选参数

完整一些会写成：

```python
result = cv2.warpPerspective(
    image,
    M,
    (width, height),
    flags=cv2.INTER_LINEAR,
    borderMode=cv2.BORDER_CONSTANT,
    borderValue=(0, 0, 0)
)
```

下面解释一下。

---

### `flags`

插值方式。  
意思是：变换后新像素如何计算。

常见有：

- `cv2.INTER_NEAREST`：最近邻插值  
- `cv2.INTER_LINEAR`：双线性插值，默认常用  
- `cv2.INTER_CUBIC`：三次插值，更平滑但更慢

一般：

- 彩色图、灰度图：常用 `INTER_LINEAR`
- 二值图/标签图/mask：常用 `INTER_NEAREST`

因为二值图如果用线性插值，边缘可能会出现灰色过渡。

---

### `borderMode`

当变换后某些区域超出原图范围时，边界如何填充。

常见：

- `cv2.BORDER_CONSTANT`：用固定值填充
- `cv2.BORDER_REPLICATE`：复制边缘像素

---

### `borderValue`

当 `borderMode=cv2.BORDER_CONSTANT` 时，填充值是多少。

例如：

```python
borderValue=(0, 0, 0)
```

表示超出部分填黑色。

如果是灰度图，也可以写：

```python
borderValue=255
```

填白色。

---



```python
import cv2
import numpy as np

# 读取原图
image = cv2.imread('document.jpg')

# 原图中的四个点：左上、右上、右下、左下
src_pts = np.float32([
    [100, 100],
    [500, 80],
    [520, 700],
    [80, 720]
])

# 目标图大小
width, height = 400, 600

# 目标图中的四个点
dst_pts = np.float32([
    [0, 0],
    [width, 0],
    [width, height],
    [0, height]
])

# 计算透视变换矩阵
M = cv2.getPerspectiveTransform(src_pts, dst_pts)

# 执行透视变换
result = cv2.warpPerspective(image, M, (width, height))

# 显示结果
cv2.imshow('Original', image)
cv2.imshow('Perspective Result', result)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 11. 点顺序为什么特别重要

这是透视变换里最容易出错的地方之一。

`src_pts` 和 `dst_pts` 的点必须一一对应，且顺序一致。

一般固定写法：

```text
左上
右上
右下
左下
```

例如：

```python
src_pts = np.float32([
    [100, 100],   # 左上
    [500, 80],    # 右上
    [520, 700],   # 右下
    [80, 720]     # 左下
])
```

```python
dst_pts = np.float32([
    [0, 0],       # 左上
    [400, 0],     # 右上
    [400, 600],   # 右下
    [0, 600]      # 左下
])
```

如果你把顺序打乱，比如把右下和左下写反，结果就可能：

- 翻转
- 扭曲
- 交叉拉伸
- 完全错位

---

# 12. 输出宽高怎么定

很多人只会随便写：

```python
(400, 600)
```

但更稳妥的做法是根据四边长度计算。

例如：

- 上边长度
- 下边长度
- 左边长度
- 右边长度

然后：

- 宽度取上下边较大值
- 高度取左右边较大值

这样输出比例更接近真实目标。

如果你输出比例乱写，图像会：

- 被拉宽
- 被压扁
- 看起来不自然

---

## 附录 A：常用函数速查表

| 模块       | 函数                      | 作用         |
| ---------- | ------------------------- | ------------ |
| 图像读写   | `cv2.imread()`            | 读取图像     |
| 图像显示   | `cv2.imshow()`            | 显示图像     |
| 按键等待   | `cv2.waitKey()`           | 等待键盘输入 |
| 图像保存   | `cv2.imwrite()`           | 保存图像     |
| 颜色转换   | `cv2.cvtColor()`          | 转换颜色空间 |
| 缩放       | `cv2.resize()`            | 调整大小     |
| 平移/旋转  | `cv2.warpAffine()`        | 仿射变换     |
| 翻转       | `cv2.flip()`              | 图像翻转     |
| 阈值       | `cv2.threshold()`         | 普通阈值     |
| 自适应阈值 | `cv2.adaptiveThreshold()` | 局部阈值     |
| 均值滤波   | `cv2.blur()`              | 均值模糊     |
| 高斯滤波   | `cv2.GaussianBlur()`      | 高斯模糊     |
| 中值滤波   | `cv2.medianBlur()`        | 中值去噪     |
| 边缘检测   | `cv2.Canny()`             | Canny 边缘   |
| 轮廓检测   | `cv2.findContours()`      | 查找轮廓     |
| 画轮廓     | `cv2.drawContours()`      | 绘制轮廓     |
| 轮廓面积   | `cv2.contourArea()`       | 计算面积     |
| 外接矩形   | `cv2.boundingRect()`      | 获取矩形框   |
| 腐蚀       | `cv2.erode()`             | 缩小白色区域 |
| 膨胀       | `cv2.dilate()`            | 扩大白色区域 |
| 形态学     | `cv2.morphologyEx()`      | 开闭运算等   |
| 掩膜       | `cv2.inRange()`           | 生成 mask    |
| 位运算     | `cv2.bitwise_and()`       | 按 mask 提取 |
| 融合       | `cv2.addWeighted()`       | 图像融合     |
| 直方图     | `cv2.calcHist()`          | 计算直方图   |
| 均衡化     | `cv2.equalizeHist()`      | 对比度增强   |
| 透视变换   | `cv2.warpPerspective()`   | 透视校正     |