<!--
 * @Description: 
 * @Version: 1.0
 * @Author: Wang Hongping
 * @Date: 2023-09-30 10:06:42
 * @LastEditors: Wang Hongping
 * @LastEditTime: 2023-09-30 15:53:18
-->
# 图像滤波
[TOC]

## 高斯滤波（Gaussian Filter）

使用高斯滤波器（$3\times3$大小，标准差$\sigma=1.3$）来对`imori_noise.jpg`进行降噪处理吧！

高斯滤波器是一种可以使图像**平滑**的滤波器，用于去除**噪声**。可用于去除噪声的滤波器还有中值滤波器（参见问题十），平滑滤波器（参见问题十一）、LoG滤波器（参见问题十九）。

高斯滤波器将中心像素周围的像素按照高斯分布加权平均进行平滑化。这样的（二维）权值通常被称为**卷积核（kernel）**或者**滤波器（filter）**。

但是，由于图像的长宽可能不是滤波器大小的整数倍，因此我们需要在图像的边缘补$0$。这种方法称作**Zero Padding**。并且权值$g$（卷积核）要进行[归一化操作](https://blog.csdn.net/lz0499/article/details/54015150)（$\sum\ g = 1$）。

按下面的高斯分布公式计算权值：
$$
g(x,y,\sigma)=\frac{1}{2\  \pi\ \sigma^2}\  e^{-\frac{x^2+y^2}{2\  \sigma^2}}
$$

标准差$\sigma=1.3$的$8-$近邻高斯滤波器如下：
$$
K=\frac{1}{16}\  \left[
 \begin{matrix}
   1 & 2 & 1 \\
   2 & 4 & 2 \\
   1 & 2 & 1
  \end{matrix}
  \right]
$$

## 中值滤波（Median Filter）

使用中值滤波器（$3\times3$大小）来对`imori_noise.jpg`进行降噪处理吧！

中值滤波器是一种可以使图像平滑的滤波器。这种滤波器用滤波器范围内（在这里是$3\times3$）像素点的中值进行滤波，请在这里也采用Zero Padding。

## 均值滤波器

使用$3\times3$的均值滤波器来进行滤波吧！

均值滤波器使用网格内像素的平均值。

## Motion Filter

使用$3\times3$的Motion Filter来进行滤波吧。

Motion Filter取对角线方向的像素的平均值，像下式这样定义：
$$
\left[
\begin{matrix}
\frac{1}{3}&0&0\\
0&\frac{1}{3}&0\\
0  & 0&  \frac{1}{3}
\end{matrix}
\right]
$$

## MAX-MIN滤波器

使用MAX-MIN滤波器来进行滤波吧。

MAX-MIN滤波器使用网格内像素的最大值和最小值的差值对网格内像素重新赋值。通常用于**边缘检测**。

边缘检测用于检测图像中的线。像这样提取图像中的信息的操作被称为**特征提取**。

边缘检测通常在灰度图像上进行。

## 差分滤波器（Differential Filter）

使用$3\times3$的差分滤波器来进行滤波吧。

差分滤波器对图像亮度急剧变化的边缘有提取效果，可以获得邻接像素的差值。

纵向：
$$
K=\left[
\begin{matrix}
0&-1&0\\
0&1&0\\
0&0&0
\end{matrix}
\right]
$$
横向：
$$
K=\left[
\begin{matrix}
0&0&0\\
-1&1&0\\
0&0&0
\end{matrix}
\right]
$$

## Sobel滤波器

使用$3\times3$的Sobel滤波器来进行滤波吧。

Sobel滤波器可以提取特定方向（纵向或横向）的边缘，滤波器按下式定义：

纵向：
$$
K=\left[
\begin{matrix}
1&2&1\\
0&0&0\\
-1&-2&-1
\end{matrix}
\right]
$$
横向：
$$
K=\left[
\begin{matrix}
1&0&-1\\
2&0&-2\\
1&0&-1
\end{matrix}
\right]
$$

## Prewitt滤波器

使用$3\times3$的Prewitt滤波器来进行滤波吧。

Prewitt滤波器是用于边缘检测的一种滤波器，使用下式定义：

纵向：
$$
K=\left[
\begin{matrix}
-1&-1&-1\\
0&0&0\\
1&1&1
\end{matrix}
\right]
$$
横向：
$$
K=\left[
\begin{matrix}
-1&0&-1\\
-1&0&1\\
-1&0&1
\end{matrix}
\right]
$$

## Laplacian滤波器

使用Laplacian滤波器来进行滤波吧。

Laplacian滤波器是对图像亮度进行二次微分从而检测边缘的滤波器。由于数字图像是离散的，$x$方向和$y$方向的一次微分分别按照以下式子计算：
$$
I_x(x,y)=\frac{I(x+1,y)-I(x,y)}{(x+1)-x}=I(x+1,y)-I(x,y)\\
I_y(x,y) =\frac{I(x, y+1) - I(x,y)}{(y+1)-y}= I(x, y+1) - I(x,y)
$$
因此二次微分按照以下式子计算：
$$
\begin{align*}
&I_{xx}(x,y) \\
=& \frac{I_x(x,y) - I_x(x-1,y)}{(x+1)-x} \\
=& I_x(x,y) - I_x(x-1,y)\\
         =&[I(x+1, y) - I(x,y)] - [I(x, y) - I(x-1,y)]\\
         =& I(x+1,y) - 2\  I(x,y) + I(x-1,y)
\end{align*}
$$
同理：
$$
I_{yy}(x,y)=I(x,y+1)-2\  I(x,y)+I(x,y-1)
$$
特此，Laplacian 表达式如下：
$$
\begin{align*}
&\nabla^2\ I(x,y)\\
=&I_{xx}(x,y)+I_{yy}(x,y)\\
=&I(x-1,y) + I(x,y-1) - 4 * I(x,y) + I(x+1,y) + I(x,y+1)
\end{align*}
$$
如果把这个式子表示为卷积核是下面这样的：
$$
K=
\left[
\begin{matrix}
0&1&0\\
1&-4&1\\
0&1&0
\end{matrix}
\right]
$$

## Emboss滤波器

使用Emboss滤波器来进行滤波吧。

Emboss滤波器可以使物体轮廓更加清晰，按照以下式子定义：
$$
K=
\left[
\begin{matrix}
-2&-1&0\\
-1&1&1\\
0&1&2
\end{matrix}
\right]
$$

## LoG滤波器

LoG即高斯-拉普拉斯（Laplacian of Gaussian）的缩写，使用高斯滤波器使图像平滑化之后再使用拉普拉斯滤波器使图像的轮廓更加清晰。

为了防止拉普拉斯滤波器计算二次微分会使得图像噪声更加明显，所以我们首先使用高斯滤波器来抑制噪声。

 LoG  滤波器使用以下式子定义：
$$
\text{LoG}(x,y)=\frac{x^2 + y^2 - s^2}{2 \  \pi \  s^6} \  e^{-\frac{x^2+y^2}{2\  s^2}}
$$
