# 实习<五>遥感影像几何校正

实现但不限于多项式校正的几何校正方法，包括：控制点选取（读取）、几何校正模型计算、重采样内插等功能。

## 几何校正的流程
### 1. 选取控制点
选取控制点的方式有多种：
- Automatic GCP Collection
	- Matching GCP from Image
	- Matching GCP from GCP Library
- Manual GCP Collection
	- GCP from Images
	- GCP from Maps
	- GCP from GPS
实现时，简化为从ENVI控制点文件导入，最好能自己设计控制点采集界面。

### 2. 几何校正模型计算
多项式模型的解算可以借助矩阵运算库，完成最小二乘的解算。多项式校正参考[利用GDAL实现影像的几何校正](https://blog.csdn.net/zhouxuguang236/article/details/27965877)

### 3.重采样内插
调用GDAL重采样接口，完成图像的[重采样内插](https://blog.csdn.net/liminlu0314/article/details/6130064)。

## Tips
- 校正后影像的范围如何确定？
- 校正后影像的空间分辨率如何确定？
- 与商用软件对比处理效率，有没有提高的办法
- 尝试RPC校正

## 扩展功能
- 影像镶嵌
影像镶嵌可参考[Image Mosaic by GDALWarp](https://blog.csdn.net/liminlu0314/article/details/7536132)。
- RPC正射校正
参考[修改GDAL库支持RPC像方改正模型](https://blog.csdn.net/liminlu0314/article/details/24810593)

## Reference
- [使用GDAL工具对卫星数据进行影像配准](https://blog.csdn.net/liminlu0314/article/details/8272307)
- [使用GDAL工具对FY3系列卫星数据进行校正](https://blog.csdn.net/liminlu0314/article/details/9181959)

---
[Home](https://wanghp119.github.io/RSIP/) | [Return](#实习<五>遥感影像几何校正) | [Prev](./D4_Preprocess.md) | [Next](./D6_Classification.md)