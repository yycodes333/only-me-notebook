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

OpenCV 中：

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
roi = image[100:200, 100:200]
image[300:400, 300:400] = roi
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

---

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

| 参数     | 说明             |
| -------- | ---------------- |
| `src`    | 输入灰度图       |
| `thresh` | 阈值             |
| `maxval` | 最大值，常用 255 |
| `type`   | 阈值类型         |

### 返回值

| 返回值   | 说明         |
| -------- | ------------ |
| `ret`    | 实际使用阈值 |
| `binary` | 输出图像     |

### 常见 type

| 类型                    | 说明           |
| ----------------------- | -------------- |
| `cv2.THRESH_BINARY`     | 大于阈值设为白 |
| `cv2.THRESH_BINARY_INV` | 大于阈值设为黑 |
| `cv2.THRESH_TRUNC`      | 截断           |
| `cv2.THRESH_TOZERO`     | 小于阈值变 0   |
| `cv2.THRESH_OTSU`       | 自动阈值       |

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

---

## 13.4 膨胀：`cv2.dilate()`

```python
dilated = cv2.dilate(binary, kernel, iterations=1)
```

作用：

- 白色区域变大
- 连接断裂部分

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
- 不支持透明通道
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

# 第 21 章：学习建议与阶段目标

## 21.1 第一阶段：基础操作熟悉

你应该会：

- 读取图像
- 显示图像
- 保存图像
- 查看图像属性
- 修改像素
- ROI 裁剪

---

## 21.2 第二阶段：预处理掌握

你应该会：

- 灰度化
- BGR 转 HSV
- 阈值处理
- 滤波去噪
- 形态学处理

---

## 21.3 第三阶段：分析能力建立

你应该会：

- 边缘检测
- 轮廓检测
- 外接矩形
- 面积过滤
- 简单形状识别
- mask 提取目标

---

## 21.4 第四阶段：综合任务练习

建议做这些小项目：

1. 检测图片中的圆形物体数量
2. 提取指定颜色目标
3. 给物体画外接矩形
4. 识别简单几何形状
5. 做一个简易文档扫描校正
6. 对文字图做二值化和去噪

---

## 21.5 最重要的学习原则

不要孤立地背函数。

要养成下面这种思维：

```text
我现在拿到的是原图？
还是灰度图？
还是二值图？
还是边缘图？
下一步最适合做什么？
```

例如：

```text
原图
→ 灰度图
→ 高斯模糊
→ Canny
→ findContours
→ contourArea 过滤
→ boundingRect 画框
```

只要你能看懂这种流程，OpenCV 基础就真的入门了。

---

# 附：建议补充学习模块

如果你学完本文，还可以继续扩展：

- 视频读取与摄像头操作
- 模板匹配
- 特征点检测
- 霍夫直线与霍夫圆
- 背景建模
- 人脸检测
- OCR 前处理
- 图像分割进阶
- 深度学习与 OpenCV 联动

---

# 一句话总结

> OpenCV 图像处理的核心，不是记住很多函数，而是理解“图像从原图经过一步步处理，最终变成可分析的结果图”的整个流程。

如果你愿意，我下一步可以继续直接帮你生成：

1. **这份 md 的“目录跳转增强版”**
2. **按“每周学习计划”拆分的 OpenCV 学习文档**
3. **把这份内容再扩展成“教材式版本”，每章后加练习题和思考题**

如果你要，我可以直接继续输出下一版。