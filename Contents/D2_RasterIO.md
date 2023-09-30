# 实习<二>遥感图像数据I/O
要求实现常用遥感图像数据（GeoTiff，IMG，Pix，jpeg，bmp、Envi）的读、写操作，读取并显示遥感图像的基本信息（行、列、波段、投影）及统计信息（均值、方差、最值、中值、直方图）。

## 目录
- [1.GDAL配置](#1-GDAL配置)
- [2.GDAL读取图像](#2-GDAL读取图像)
- [3.GDAL统计信息](# 3-GDAL统计信息)
- [4. Thinking](#4-Thinking)
- [5. Reference](#5-Reference)

## 1. GDAL配置
### Compile GDAL
[GDAL编译Windows平台下64位的方式](https://blog.csdn.net/liminlu0314/article/details/6144899)

## 2. GDAL读取图像
[GDAL源码剖析（七）之GDAL RasterIO使用说明](https://blog.csdn.net/liminlu0314/article/details/7072224)

[GDAL源码剖析（七）之GDAL RasterIO使用说明-续](https://blog.csdn.net/liminlu0314/article/details/8301585)

## 3. GDAL统计信息
[GDAL计算栅格图像统计值相关说明](https://blog.csdn.net/liminlu0314/article/details/8495274)

## 4. Thinking
### 4.1 当图像处理的时间较长时，是否有考虑到给算法增加一个进度条呢？
GDAL支持了2种进度条，一种是console进度条。
![console](http://hi.csdn.net/attachment/201101/10/0_12946620634QxM.gif)

一种是带界面的进度条
![GUI](http://hi.csdn.net/attachment/201202/20/0_13297429073Lbt.gif)

以下是为GDAL算法增加进度条的方法，值得借鉴。详情请参考：[GDAL进度条扩展](https://blog.csdn.net/liminlu0314/article/details/7276954)和[GDAL库进度信息编写示例](https://blog.csdn.net/liminlu0314/article/details/51019220)


## 5. Reference
- [GDAL官方帮助](http://gdal.org/index.html)

---
[Home](https://wanghp119.github.io/RSIP/) | [Return](#目录) | [Prev](./D1_RSIP_Frame.md) | [Next](./D3_ImageDisplay.md)
