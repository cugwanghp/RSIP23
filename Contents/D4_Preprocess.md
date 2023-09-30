# 实习<四>遥感影像预处理
要求实现图像滤波、PCA分析、影像融合等功能。

## 图像滤波
### 空域滤波
实现图像的卷积运算，接口通用，将滤波核设置为参数。
- 均值滤波
- 差分滤波，sobel，robert等
参考[基于GDAL的一个通用的3×3模板函数](https://blog.csdn.net/liminlu0314/article/details/8316156)

### 频域滤波（选做）
借助于[FFTW3](www.fftw.org)完成傅里叶变换，实现高通、低通滤波

## PCA（选做）
主成分分析的原理参考教材。
- PCA的实现参考[基于GDAL实现的PCA变换](https://blog.csdn.net/liminlu0314/article/details/8957009)
- MTL库的使用参考[使用MTL库求解矩阵特征值和特征向量](https://blog.csdn.net/liminlu0314/article/details/8957155)
- 另一个PCA库参考[libPCA](https://sourceforge.net/projects/libpca/)

## 影像融合
实现简单的IHS加权融合，算法参见教材。

## Tips
- 空域滤波，如何处理图像边界部分

---
[Home](https://wanghp119.github.io/RSIP/) | [Return](#实习<四>遥感影像预处理) |  [Prev](./D3_ImageDisplay.md) | [Next](./D5_Geocorrection.md)