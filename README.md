# 数字图像处理实习题
![face](./docs/pictures/face.png)

## 目录
- [一、课设目标](#一课设目标) 
- [二、课设要求](#二课设要求)
- [三、课设安排](#三课设安排)
- [四、课设内容](#四课设内容)
- [五、参考资料](#五参考资料)
- [六、问题讨论](#六问题讨论)

## 一、课设目标
1. 强化对数字图像处理算法原理的**理解**
2. 培养解决图像处理应用问题的**设计能力**
3. 培养数字图像处理算法的**编程能力**
4. 培养**交流沟通和书面表达能力**

## 二、课设要求
1. 使用C++, Python等语言实现题目的算法；
2. 可以调用gdal, Qt，opencv, egien，numpy等库来实现图像io，矩阵存储和图像显示，但算法不可直接调用开源库；
3. 每个算法提供测试程序，能直接运行得到结果；
4. 完成每个算法原理讲解、实现流程图、程序接口设计说明、源代码、算法处理的测试结果（可与ENVI,ERDAS等专业软件处理结果对比）。
5. 编码需符合编码规范。工程文件的命名、组织、函数的命名、变量的命名必须为英文，简洁而明晰，并添加适当注释以确保程序的可读性。
6. 课程设计结束后**1 周内**，依照[**课设报告模板**](./docs/contents/%E8%AF%BE%E8%AE%BE%E6%8A%A5%E5%91%8A.docx)撰写课设报告。
7. 课设报告中尽量避免引用大段的代码，保留界面截图和运行结果即可。
8. 提交纸质课设报告一份，课设报告和代码工程的电子档各一份，以班级为单位上交。

### 2.课程考核
- 实习成绩组成：

|序号|内容|分值占比|
|:-:|:-:|:-:|
|1| 算法题（2道）|50%|
|2|综合题（1道）|30%|
|3|实习报告|20%|
- **课设报告封面、格式要统一**
- **严禁抄袭，一经发现，记为零分**

[**返回目录**](#目录)
---

## 三、课设时间安排
|次数|时间|周次|机房|备注|
|:---:|:---:|:---:|:---:|---|
|1|10.07|周六 下午|科教八201||
|2|10.19|周四 下午|科教八201||
|3|10.26|周六 下午|科教八201||
|4|11.03|周五 上午|科教八201||
|5|11.13|周一 下午|科教八201|中期检查|
|6|11.14|周二 下午|科教八201||
|7|11.17|周五 上午|科教八201||
|8|11.17|周五 下午|科教八201|终期检查|

[**返回目录**](#目录)
---

## 四、课设内容
本次课程的重点是图像处理算法，因此对GUI不做过多的要求，即控制台程序和GUI界面程序均可。但课设的所有算法，要能够组织到一起，需要在一个可执行程序下调用。当然我们更希望看到美观的GUI界面程序。课设内容包括以下功能：

### Tutorial
教程内容包括：安装，读取、显示图像、操作像素、拷贝图像、保存图像、练习题
- [Opencv C++ Tutorial](./Tutorial/README_opencv_cpp_install.md)
- [Opencv Python Tutorial](./Tutorial/README_opencv_py_install.md)

### 算法题（要求每人独立至少完成2题）
#### 1. [图像增强与显示](./Contents/Enhancement.md)
- 直方图
  - 直方图标准化
  - 直方图均衡化
  - 直方图匹配
- 2%线性拉伸
- 灰度化
- 二值化
  - 阈值二值化
  - 大津二值化
- 池化
  - 平均池化
  - 最大池化

#### 2. [图像滤波](./Contents/Filtering.md)

- 高斯滤波
- 中值滤波
- 均值滤波
- 运动滤波
- MAX-MIN滤波
- 差分滤波
  - Sobel滤波器
  - Prewitt滤波器
  - Laplacian滤波器
- Emboss滤波器（浮雕）
- LoG滤波器

#### 3. [几何变换](./Contents/GeoTrans.md)

- 仿射变换
- 图像插值
  - 最邻近插值
  - 双线性插值
  - 双三次插值

#### 4. [图像变换](./Contents/Transformation.md)
- HSV变换
- 傅里叶变换
- PCA变换

#### 5. [边缘检测](./Contents/EdgeDetection.md)
- Canny边缘检测
  - 边缘强度
  - 边缘细化
  - 滞后阈值
- 霍夫变换
  - 直线检测
  - NMS
  - 逆变换

#### 6. [特征提取](./Contents/Features.md)
- 特征算子
  - HOG
  - SIFT
  - Harris
  - Hessian
- 匹配
  - 模式匹配

#### 7. [图像分类](./Contents/Classification.md)
- K-Means
- ISODATA
  
### 综合题（2-3人一组，完成其中1题）
#### 1. 几何校正
- 需求
	遥感图像自动配准程序。输入两幅图像，提取特征点，完成特征点的匹配，计算校正参数，完成图像重采样，实现几何校正。
- 功能分析
	- 特征点提取：SIFT 或 其他算子
	- 特征点匹配：RANSAC匹配方法
	- 几何校正参数计算：参数计算
	- 图像重采样：几何校正

#### 2. 图像拼接
- 需求
	输入多幅相邻的图像，按临近位置关系，将其拼接为一幅完整的图像。如相机拍摄全景照片的功能。
- 功能分析
  	- 特征点提取
  	- 特征点匹配
  	- 色调均衡
  	- 几何校正

#### 3. 目标识别
- 需求
	
  
- 功能分析


---


[**返回目录**](#目录)
---

## 五、参考资料
### 1. 开源库
- [Qt入门](https://blog.csdn.net/Louis_815/article/details/54286544)
- [GDAL](http://www.gdal.org)
- [GDAL Document](https://gdal.org/gdal.pdf)
- [OpenCV](www.opencv.org)
- [CImg](www.cimg.eu)
- [CxImage](https://www.codeproject.com/Articles/1300/CxImage)
- [Eigen](<http://eigen.tuxfamily.org/index.php?title=Main_Page>)
- Eigen的配置和安装，请参考<https://blog.csdn.net/abcjennifer/article/details/7781936>。
- Eigen的使用，可以参考<https://zhuanlan.zhihu.com/p/31111908>。

---

### 2.功能参考
- [遥感图像数据I/O](./contents/D2_RasterIO.md)
	要求实现常用遥感图像数据（GeoTiff，IMG，Pix，jpeg，bmp、Envi）的读、写操作，读取并显示遥感图像的基本信息及统计信息。

- [遥感图像的显示](./contents/D3_ImageDisplay.md)
	要求实现遥感图像的灰度显示、彩色合成显示、具备放大、缩小、屏幕适应的显示方式，具备线性拉伸、均衡化等增强显示方式。

- [遥感影像预处理](./contents/D4_Preprocess.md)
	要求实现图像滤波、PCA分析、影像融合等功能。

- [遥感影像几何校正](./contents/D5_Geocorrection.md)
	实现但不限于多项式校正的几何校正方法，包括：控制点选取（读取）、几何校正模型计算、重采样内插等功能。

- [遥感影像分类](./contents/D6_Classification.md)
	实现监督分类和非监督分类的算法至少各一种。

- [遥感图像处理过程](https://blog.csdn.net/liminlu0314/article/details/8757262)

---

## 六、问题讨论
如果你有关于本门课程设计的任何疑问、建议、指正，欢迎在[讨论专区](https://github.com/cugwanghp/RSIP23/issues)中留言，我们将第一时间回复。

[**返回目录**](#目录)
---
