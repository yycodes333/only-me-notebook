```
		学习资源：
				https://www.runoob.com/opencv/opencv-image-thresholding.html
				https://blog.csdn.net/qq_55621259/article/details/125269490
		
		
		
```

# 3.4 OpenCV图像处理

## 3.4.1 简介

**OpenCV** 全称是**Open Source Computer Vision Library**，即：**开源计算机视觉库**

1. 它是一个非常常用的图像处理与计算机视觉工具库，支持：

   1. Python
   2. Java
   3. C++等语言

2. OpenCV 是由多个模块组成的，每个模块都提供了不同的功能，以下是 OpenCV 中最常用的一些模块：

   1. **cv2.core**: 核心模块，包含了图像处理的基础功能（如图像数组的表示和操作）。

      **功能:** 提供 OpenCV 的核心功能，包括基本数据结构、矩阵操作、绘图函数等。

      1. **Mat:** OpenCV 中用于存储图像和矩阵的基本数据结构。
      2. **Scalar:** 用于表示颜色或像素值。
      3. **Point、Size、Rect:** 用于表示点、尺寸和矩形。
      4. **基本绘图函数:** `cv.line()`、`cv.circle()`、`cv.rectangle()`、`cv.putText()` 等。

      **应用场景:**

      1. 图像的基本操作（如创建、复制、裁剪）。
      2. 绘制几何图形和文本。

   2. **cv2.imgproc**: 图像处理模块，提供图像的各种操作，如滤波、图像变换、形态学操作等。

      **功能:** 提供图像处理功能，包括图像滤波、几何变换、颜色空间转换等。

      1. **图像滤波:** `cv.blur()`、`cv.GaussianBlur()`、`cv.medianBlur()` 等。
      2. **几何变换:** `cv.resize()`、`cv.warpAffine()`、`cv.warpPerspective()` 等。
      3. **颜色空间转换:** `cv.cvtColor()`（如 BGR 转灰度、BGR 转 HSV）。
      4. **阈值处理:** `cv.threshold()`、`cv.adaptiveThreshold()`。
      5. **边缘检测:** `cv.Canny()`、`cv.Sobel()`、`cv.Laplacian()`。

      **应用场景:**

      1. 图像平滑、锐化、边缘检测。
      2. 图像缩放、旋转、仿射变换。
      3. 图像二值化、颜色空间转换。

   3. **cv2.highgui**: 图形用户界面模块，提供显示图像和视频的功能。

      **功能:** 提供高层 GUI 和媒体 I/O 功能，用于图像的显示和交互。

      1. **图像显示:** `cv.imshow()`、`cv.waitKey()`、`cv.destroyAllWindows()`。
      2. **视频捕获:** `cv.VideoCapture()`、`cv.VideoWriter()`。
      3. **鼠标和键盘事件:** `cv.setMouseCallback()`。

      **应用场景:**

      1. 显示图像和视频。
      2. 捕获摄像头或视频文件。
      3. 处理用户交互（如鼠标点击、键盘输入）。

   4. **cv2.video**: 提供视频处理的功能，如视频捕捉、视频流的处理等。

      **功能:** 提供视频分析功能，包括运动检测、目标跟踪等。

      1. **背景减除:** `cv.createBackgroundSubtractorMOG2()`、`cv.createBackgroundSubtractorKNN()`。
      2. **光流法:** `cv.calcOpticalFlowPyrLK()`。
      3. **目标跟踪:** `cv.TrackerKCF_create()`、`cv.TrackerMOSSE_create()`。

      **应用场景:**

      1. 视频中的运动检测。
      2. 目标跟踪（如行人、车辆跟踪）。

   5. **cv2.features2d**: 特征检测与匹配模块，包含了角点、边缘、关键点检测等。

      **功能:** 提供特征检测和描述功能。

      1. **特征检测:** `cv.SIFT_create()`、`cv.ORB_create()`、`cv.SURF_create()`。
      2. **特征匹配:** `cv.BFMatcher()`、`cv.FlannBasedMatcher()`。
      3. **关键点绘制:** `cv.drawKeypoints()`。

      **应用场景:**

      1. 图像特征提取和匹配。
      2. 图像拼接、物体识别。

   6. **cv2.ml**: 机器学习模块，提供了多种机器学习算法，可以进行图像分类、回归、聚类等。

      **功能:** 提供机器学习算法。

      1. **支持向量机 (SVM):** `cv.ml.SVM_create()`。
      2. **K 均值聚类 (K-Means):** `cv.kmeans()`。
      3. **神经网络 (ANN):** `cv.ml.ANN_MLP_create()`。

      **应用场景:**

      1. 图像分类、聚类分析。

   7. **cv2.calib3d** : 相机校准和 3D 重建模块

      **功能:** 提供相机校准和 3D 重建功能。

      1. **相机校准:** `cv.calibrateCamera()`、`cv.findChessboardCorners()`。
      2. **3D 重建:** `cv.solvePnP()`、`cv.reprojectImageTo3D()`。

      **应用场景:**

      1. 相机标定（用于去除镜头畸变）。
      2. 3D 重建（如从 2D 图像恢复 3D 信息）。

   8. **cv2.objdetect** : 目标检测模块。

      **功能:** 提供目标检测功能。

      1. **aar 特征分类器:** `cv.CascadeClassifier()`（用于人脸检测）。
      2. **HOG 特征分类器:** 用于行人检测。

      **应用场景:**

      1. 人脸检测、行人检测。

   9. **cv2.dnn** : 深度学习模块。

      **功能:** 提供深度学习功能，支持加载和运行预训练的深度学习模型。

      1. **模型加载:** `cv.dnn.readNetFromCaffe()`、`cv.dnn.readNetFromTensorflow()`。
      2. **前向传播:** `net.forward()`。

      **应用场景:**

      1. 图像分类、目标检测、语义分割。

   ## 3.4.2 安装

   在使用 OpenCV 之前，首先需要在你的开发环境中安装它。

   安装 OpenCV 的方法取决于你使用的操作系统以及开发语言，我们以 Python 环境下的安装为例，因为 Python 是 OpenCV 最常用的编程语言之一。

   使用 pip 安装 OpenCV 是最简单和直接的方法，你只需要在命令行中输入以下命令：

   ```bash
   pip install opencv-python
   ```

   该命令会自动从 PyPI（Python 包索引）下载并安装 OpenCV 的 Python 版本。

   如果需要额外的模块（例如 contrib 模块），可以安装 opencv-contrib-python：

   ```
   pip install opencv-contrib-python
   ```

   这个版本包含了更多 OpenCV 的模块和功能，适合需要深度学习、物体识别等复杂任务的开发者。

   opencv-python 和 opencv-contrib-python 包是 OpenCV 的不同版本，它们分别包含了核心模块和扩展模块，一般来说，opencv-contrib-python 包含更多功能，适合需要高级应用的用户。

   如果安装速度慢，可以切换 pip 源（例如使用清华源）：

   ```bash
   pip install opencv-python -i https://pypi.tuna.tsinghua.edu.cn/simple
   ```

   安装完成后，可以通过以下代码验证 OpenCV 是否安装成功：

   ```python
   import cv2
   print(cv2.__version__)
   ```

   若成功输出版本号则证明安装成功！



