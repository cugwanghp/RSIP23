# 特征提取
[TOC]

## Hessian角点检测[^0]

[^0]: 见[这里](http://vision.stanford.edu/teaching/cs231a_autumn1112/lecture/lecture11_detectors_descriptors_cs231a.pdf)。

角点检测是检测边缘上的角点。角点是曲率变大的点，下式定义了高斯曲率：
$$
K=\frac{\det(H)}{(1+{I_x}^2+{I_y}^2)^2}
$$

其中：

* $\det(H)=I_{xx}\ I_{yy}-{I_{xy}}^2$；
* $H$表示Hessian矩阵。图像的二次微分（通过将Sobel滤波器应用于灰度图像计算得来）。对于图像上的一点，按照下式定义：
  * $I_x$：应用$x$方向上的Sobel滤波器；
  * $I_y$：应用$y$方向上的Sobel滤波器；
  * $H=\left[\begin{matrix}I_{xx}&I_{xy}\\I_{xy}&I_{yy}\end{matrix}\right]$

在Hessian角点检测中，$\det{H}$将极大点视为j角点。

如果中心像素与其$8-$近邻像素相比值最大，则中心像素为极大点。

解答中，角点是$\det(H)$为极大值，并且大于$\max(\det(H))\cdot 0.1$的点。

## Harris角点检测
Harris 角点检测算法如下：

1. 对图像进行灰度化处理；

2. 利用Sobel滤波器求出海森矩阵（Hessian matrix）：[^1]
   
   [^1]: 这一步的数学原理可见[这里](https://blog.csdn.net/lwzkiller/article/details/54633670)。
   
   $$
   H=\left[\begin{matrix}{I_x}^2&I_xI_y\\I_xI_y&{I_y}^2\end{matrix}\right]
   $$
   
3. 将高斯滤波器分别应用于${I_x}^2$、${I_y}^2$、$I_x\ I_y$；

4. 计算每个像素的$R = \det(H) - k\ (\text{trace}(H))^2$。通常$K$在$[0.04,0.16]$范围内取值.

5. 满足 $R \geq \max(R) \cdot\text{th}  $的像素点即为角点。

参数如下：

* 高斯滤波器：$k=3, \sigma=3$；
*  $K = 0.04, \text{th} = 0.1$。

## 方向梯度直方图（HOG）
HOG（Histogram of Oriented Gradients）是一种表示图像特征量的方法。特征量是表示图像的状态等的向量集合。

在图像识别（图像是什么）和检测（物体在图像中的哪个位置）中，我们需要：

1. 从图像中获取特征量（特征提取）；
2. 基于特征量识别和检测（识别和检测）。

由于深度学习通过机器学习自动执行特征提取和识别，所以看不到 HOG，但在深度学习变得流行之前，HOG 经常被用作特征量表达。

通过以下算法获得HOG：

1. 图像灰度化之后，在x方向和y方向上求出亮度的梯度：

   * $x$方向：
     $$
     g_x=I(x+1,y)-I(x-1,y)
     $$

   * $y$方向：
     $$
     g_y=I(x,y+1)-I(x,y-1)
     $$

2. 从$g_x$和$g_y$确定梯度幅值和梯度方向：

   * 梯度幅值：[^1]

     [^1]: 这里公式原文写的是：$mag = sqrt(gt ** 2 + gy ** 2)$。可能是笔误。

     $$
     mag=\sqrt{{g_x}^2+{g_y}^2}
     $$

   * 梯度方向：
     $$
     ang=\arctan{\frac{g_y}{g_x}}
     $$

3. 将梯度方向$[0,180]$进行9等分量化。也就是说，对于$[0,20]$量化为 index 0，对于$[20,40]$量化为 index 1……

4. 将图像划分为$N \times N$个区域（该区域称为 cell），并作出 cell 内步骤3得到的 index 的直方图。ただし、当表示は1でなく勾配角度を求める。

5. C x  C个 cell 被称为一个 block。对每个 block 内的 cell 的直方图通过下面的式子进行归一化。由于归一化过程中窗口一次移动一个 cell 来完成的，因此一个 cell 会被归一化多次，通常$\epsilon=1$：
   $$
   h(t)=\frac{h(t)}{\sqrt{\sum\ h(t)+\epsilon}}
   $$

以上，求出 HOG 特征值。

### 第一步：梯度幅值・梯度方向
这一问，我们完成步骤1到3。

为了使示例答案更容易看出效果，`gra`是彩色的。此外，`mag`被归一化至$[0,255]$。

### 第二步：梯度直方图

在这里完成 HOG 的第4步。

取$N=8$，$8 \times 8$个像素为一个 cell，将每个 cell 的梯度幅值加到梯度方向的index处。

解答为按照下面的顺序排列索引对应的直方图：
$$
\begin{matrix}
1&2& 3\\
4& 5& 6\\
7& 8 &9
\end{matrix}
$$

### 第三步：直方图归一化

在这里完成 HOG 的第5步。

取$C=3$，将$3\times 3$个 cell 看作一个 block，进行直方图归一化，通常$\epsilon=1$：
$$
h(t)=\frac{h(t)}{\sqrt{\sum\ h(t)+\epsilon}}
$$
在此，我们得到HOG特征量。

### 第四步：可视化特征量

在这里我们将得到的特征量可视化。

如果将特征量叠加在灰度化后的`imori.jpg`上，可以很容易看到（蝾螈的）外形。

一个好的可视化的方法是这样的，为 cell 内的每个 index 的方向画一条线段，并且值越大，线段越白，值越小，线段越黑。
