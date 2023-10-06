# Practical course of Remote Sensing Digital Image Pocessing遥感图像处理课程设计

[TOC]

## 目标 - Motivation
遥感图像处理是遥感科学与技术专业的专业基础课，其核心内容是若干遥感图像处理算法和原理。本课程设计的主要目标培养学生既懂遥感图像处理算法理论，也懂遥感图像处理算法实现的专业素养和动手能力的专业人才，其具体目标为：

1. 进一步强化、夯实学生对遥感图像处理方法和原理的理解
2. 培养遥感应用方法设计的能力
3. 培养遥感图像处理算法实现的编程能力。

## 课程设计要求
1. 独立完成课设的内容，意味着你不能直接使用第三方的源码；
2. 程序语言原则上使用C/C++，若你有更为擅长的语言，也可以使用Python、IDL、Matlab等；
3. 无论你使用哪种程序语言，都需要讲核心算法从底层实现起，这意味着你不得直接调用第三方库（如ENVI-IDL、OpenCV、Matlab等）已经封装的函数；
4. 课设的每一个算法，要有算法基本原理的讲解、算法实现的流程图、程序接口设计说明、源代码、算法处理的测试结果（与主流遥感图像处理软件结果对比）。
5. 按照规定的课设报告模板，提交课设报告，并提交源码。

## 课设考核说明
1. 课设成绩由4部分组成，分值比重为：基础题50%、 提升题20%、挑战题30%、课设报告20%。
2. 基础题为**必做题**，除了*选做部分*外，需要全部完成，才能得到50%分数
3. 提升题为选做题，任选2道完成即可得到20%分数
4. 挑战题留给学有余力的学生完成，任选1道完成，即可得到30%分数。若你选做挑战题，可以跳过提升题（不做）
5. 课设报告必须要封面、格式统一，才能得到20%分数
6. 无论源代码或报告内容出现抄袭，一律零分处理

## 课设内容
### 基础题 - Basic Algorithm
	本次课程的重点是图像处理算法，因此对GUI不做过多的要求。控制台程序和GUI界面程序均可以。但课设的所有算法，要能够组织到一起，可以在一个可执行程序下调用。

#### 1.图像增强-Enhancement
	- 线性拉伸
		- 输入拉伸的范围，将图像DN值做线性拉伸
		- 函数名：LinearStretch
	
	- 直方图匹配（直方图均衡化）
		- 输入一幅图像，输入参考图像或参考的直方图，将输入图像的直方图匹配到另一幅图像
		- 函数名：HistogramMatch
		- 函数可能需要重载，足以实现直方图均衡化

#### 2. 卷积滤波-Filter
	- 空域滤波
		- 输入图像和卷积核，输出结果图像
		- 函数名：ConvFilter

#### 3. Transformation
	- PCA 变换
		- 输入图像、输出PCA变化结果及中间矩阵
		- 函数名：Pca
		- 备注：最好能实现逆PCA变换
	
	- FFT（选做） 
		- 傅里叶变换，可以参考Eigen或fftw的内容
		- 输入图像，输出频域图像

#### 4. Feature Extraction
	- 边缘提取
		- 输入图像，实现一种差分方式的边缘提取算法，输出边缘二值图
		- 函数名：Edge
	- 特征点提取（选做）
		- 输入图像，实现一种特征点（SIFT/SOBEL/Forest）的提取算法
		- 函数名：FeatureExtraction

#### 5. 几何变换
	- 1. 重采样
		- 输入图像和重采样类型，输出重采样结果图像
		- 最近邻采样
		- 双线性内插采样
		- 三次立方卷积采样（选做）
	
	- 2. 仿射变换
		- 输入旋转角度、缩放因子，生成变换后的图像
		- AffineTrans
	
	- 3. 多项式校正
		- PolyTrans
	
	- 4. 正射校正（选做）

#### 6. Classification
	- k-means（选做）
	- ISODATA
	- SVM（选做）

### 提升题 - Advanced
#### 7. 数学形态学 [选做]
	- 输入二值图像，完成开运算、闭运算、膨胀、腐蚀的功能

#### 8. Change detection（选做）
	- 差值法变化检测
	- 比值法变化监测
	- 分类后比较

#### 9. Fusion（选做）
	- IHS融合
		- 输入配准好的全色图像和多光谱，完成通道替换的IHS融合
		- 函数名：FusionIHS
	- PCA融合
		- 输入同IHS融合
		- 函数名：FusionPCA

#### 10. Mosaic（选做）
	输入多幅图像，按地理位置拼接为一幅图像。

### 3.挑战题 - Application Challenge
#### 11. GeoCorrect
	- 1. 需求
		遥感图像自动配准程序。输入两幅图像，提取特征点，完成特征点的匹配，计算校正参数，完成图像重采样，实现几何校正。
	- 2. 功能分析
		- 特征点提取：SIFT 或 其他算子
		- 特征点匹配：RANSAC匹配方法
		- 几何校正参数计算：参数计算
		- 图像重采样：几何校正
#### 12. 快速大气校正
	- 1. 需求
	输入卫星成像时刻、成像姿态、轨道参数、定标系数和程辐射等，带入大气辐射传输的方程，计算表观反射率。
	- 2. 功能分析
		- DN转换为辐射亮度
		- 程辐射计算
		- 大气校正
#### 13. 温度反演

```
- 1. 需求
	输入热红外图像，完成一系列温度反演算法，输出温度图。
- 2. 功能分析
	- 地表比辐射率计算
	- 温度反演算法
```

#### 14. 土地利用分类

```
- 1. 需求
	输入多光谱图像，完成土地利用分类功能，包括：监督分类、分类后处理、分类精度评价。
- 2. 功能分析
	-ROI选取
	-ROI评价
	-分类算法
	-分类后处理
	-分类精度评价
```

## 课设环境配置-Environment

	尽管图像处理的算法，你可以从底层开始写起，但我们还是希望你能够借助已有的“梯子”，写出“专业”的图像处理算法。因此，你可能会用到如下几个开源的程序库，我们简单说明如下：

1. [GDAL](www.gdal.org)
  GDAL - Geospatial Data Abstraction Libray 是大名鼎鼎的空间数据抽象接口，其主要的功能是提供了一系列抽象的接口，用于多大上百种栅格数据和矢量数据的IO，同时GDAL还提供了不少的图像处理算法。

	GDAL的编译版本，可以从<http://www.gisinternals.com/>下载而得。当然，你可以下载源代码，在自己的编译器下[编译GDAL库](http://trac.osgeo.org/gdal/wiki/BuildHints)。

	GDAL的使用，推荐参考GDAL 官方文档和帮助，这里最权威和准确，同时，也可以查看李民录在CSDN关于[GDAL的博客](https://blog.csdn.net/liminlu0314)。但最终还是以代码接口为准。

  ---

2. [Eigen](<http://eigen.tuxfamily.org/index.php?title=Main_Page>)

	图像数据天然的是矩阵，图像处理会涉及到矩阵运算。矩阵运算的库有lapack, bias, eigen等，此处我们推荐轻量级的矩阵运算库Eigen，其主要目的是用于存放数据。Eigen 适用范围广，支持包括固定大小、任意大小的所有矩阵操作，甚至是稀疏矩阵；支持所有标准的数值类型，并且可以扩展为自定义的数值类型；支持多种矩阵分解及其几何特征的求解；它提供了许多专门的功能，如非线性优化，矩阵功能，多项式解算器，快速傅立叶变换等。

   Eigen的配置和安装，请参考<<https://blog.csdn.net/abcjennifer/article/details/7781936>>。

   Eigen的使用，可以参考<<https://zhuanlan.zhihu.com/p/31111908>>。

  ---

3. [OpenCV](<http://opencv.org/>)

	[OpenCV](<http://opencv.org/>)是一个著名的计算机视觉的开源算法库，在完成课设相关的内容过程中，可以借鉴和参考其相关的[算法和思想](<https://docs.opencv.org/3.4/d6/d00/tutorial_py_root.html>)。

4. 程序架构示意图

	![pic](./pictures/workflow.png)