# 图像变换
[TOC]

## 问题三十二：傅立叶变换（Fourier Transform）

使用离散二维傅立叶变换（Discrete Fourier Transformation），将灰度化的`imori.jpg`表示为频谱图。然后用二维离散傅立叶逆变换将图像复原。

二维离散傅立叶变换是傅立叶变换在图像处理上的应用方法。通常傅立叶变换用于分离模拟信号或音频等连续一维信号的频率。但是，数字图像使用$[0,255]$范围内的离散值表示，并且图像使用$H\times W$的二维矩阵表示，所以在这里使用二维离散傅立叶变换。

二维离散傅立叶变换使用下式计算，其中$I$表示输入图像：
$$
G(k,l)=\frac{1}{H\  W}\ \sum\limits_{y=0}^{H-1}\ \sum\limits_{x=0}^{W-1}\ I(x,y)\ e^{-2\  \pi\  j\ (\frac{k\  x}{W}+\frac{l\  y}{H})}
$$
在这里让图像灰度化后，再进行离散二维傅立叶变换。

频谱图为了能表示复数$G$，所以图上所画长度为$G$的绝对值。这回的图像表示时，请将频谱图缩放至$[0,255]$范围。

二维离散傅立叶逆变换从频率分量$G$按照下式复原图像：
$$
I(x,y)=\frac{1}{H\  W}\ \sum\limits_{l=0}^{H-1}\ \sum\limits_{k=0}^{W-1}\ G(l,k)\ e^{2\  \pi\  j\ (\frac{k\  x}{W}+\frac{l\  y}{H})}
$$

上式中$\exp(j)$是个复数，实际编程的时候请务必使用下式中的绝对值形态[^1]：

[^1]: 这里应该有个公式的，但是它不知道去哪儿了。

如果只是简单地使用`for`语句的话，计算量达到$128^4$，十分耗时。如果善用`NumPy`的化，则可以减少计算量（答案中已经减少到$128^2$）。

| 输入 (imori.jpg) | 灰度化 (imori_gray.jpg) | 输出 (answers_image/answer_32.jpg) | 频谱图 (answers_image/answer_32_ps.py) |
| :--------------: | :---------------------: | :--------------------------------: | :------------------------------------: |
|  ![](imori.jpg)  |   ![](imori_gray.jpg)   |  ![](answers_image/answer_32.jpg)  |  ![](answers_image/answer_32_ps.jpg)   |

> 答案 
>
> - Python >> [answers_py/answer_32.py](answers_py/answer_32.py)
> - C++ >> [answers_cpp/answer_32.cpp](answers_cpp/answer_32.cpp)

## 问题三十三：傅立叶变换——低通滤波

将`imori.jpg`灰度化之后进行傅立叶变换并进行低通滤波，之后再用傅立叶逆变换复原吧！

通过离散傅立叶变换得到的频率在左上、右上、左下、右下等地方频率较低，在中心位置频率较高。

![lpf](../assets/lpf.png)

在图像中，高频成分指的是颜色改变的地方（噪声或者轮廓等），低频成分指的是颜色不怎么改变的部分（比如落日的渐变）。在这里，使用去除高频成分，保留低频成分的**低通滤波器**吧！

在这里，假设从低频的中心到高频的距离为$r$，我们保留$0.5\ r$​的低频分量。

| 输入 (imori.jpg) | 输出(answers_image/answer_33.jpg) |
| :--------------: | :-------------------------------: |
|  ![](imori.jpg)  | ![](answers_image/answer_33.jpg)  |

> 答案
>
> - Python >> [answers_py/answer_33.py](answers_py/answer_33.py)
> - C++ >> [answers_cpp/answer_33.cpp](answers_cpp/answer_33.cpp)

## 问题三十四：傅立叶变换——高通滤波

将`imori.jpg`灰度化之后进行傅立叶变换并进行高通滤波，之后再用傅立叶逆变换复原吧！

在这里，我们使用可以去除低频部分，只保留高频部分的**高通滤波器**。假设从低频的中心到高频的距离为$r$，我们保留$0.2\ r$​的低频分量。

| 输入 (imori.jpg) | 输出(answers_image/answer_34.jpg) |
| :--------------: | :-------------------------------: |
|  ![](imori.jpg)  | ![](answers_image/answer_34.jpg)  |

> 答案
>
> - Python >> [answers_py/answer_34.py](answers_py/answer_34.py)
> - C++ >> [answers_cpp/answer_34.cpp](answers_cpp/answer_34.cpp)

## 问题三十五：傅立叶变换——带通滤波

将`imori.jpg`灰度化之后进行傅立叶变换并进行带通滤波，之后再用傅立叶逆变换复原吧！

在这里，我们使用可以保留介于低频成分和高频成分之间的分量的**带通滤波器**。在这里，我们使用可以去除低频部分，只保留高频部分的高通滤波器。假设从低频的中心到高频的距离为$r$，我们保留$0.1\  r$至$0.5\  r$的分量。  

| 输入 (imori.jpg) | 输出(answers_image/answer_34.jpg) |
| :--------------: | :-------------------------------: |
|  ![](imori.jpg)  | ![](answers_image/answer_34.jpg)  |

> 答案
>
> - Python >> [answers_py/answer_35.py](answers_py/answer_35.py)
> - C++ >> [answers_cpp/answer_35.cpp](answers_cpp/answer_35.cpp)

## PCA

## MNF