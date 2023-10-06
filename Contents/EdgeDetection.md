# 边缘提取
[TOC]

- Canny边缘检测
  - 边缘强度
  - 边缘细化
  - 滞后阈值
- 霍夫变换
  - 直线检测
  - NMS
  - 逆变换
- 数学形态学
  - 膨胀 Dilate
  - 腐蚀 Erode
  - 开运算 - Opening
  - 闭运算 - Closing

## 问题四十一：Canny边缘检测：
Canny边缘检测法过程为：

1. 使用高斯滤波；
2. 在$x$方向和$y$方向上使用Sobel滤波器，在此之上求出边缘的强度和边缘的梯度；
3. 对梯度幅值进行非极大值抑制（Non-maximum suppression）来使边缘变得更细；
4. 使用滞后阈值来对阈值进行处理。
   
上面就是图像边缘检测的方法了。在这里我们先完成第一步和第二步。
### 第一步：边缘强度计算

按照以下步骤进行处理：
1. 将图像进行灰度化处理；

2. 将图像进行高斯滤波（$5\times5$，$s=1.4$）；

3. 在$x$方向和$y$方向上使用Sobel滤波器，在此之上求出边缘梯度$f_x$和$f_y$。边缘梯度可以按照下式求得：
   $$
   \text{edge}=\sqrt{{f_x}^2+{f_x}^2}\\
   \text{tan}=\arctan(\frac{f_y}{f_x})
   $$

4. 使用下面的公式将梯度方向量化：
   $$
   \text{angle}=\begin{cases}
   0 & (\text{if}\quad -0.4142 < \tan \leq 0.4142)\\
   45 & (\text{if}\quad 0.4142 < \tan < 2.4142)\\
   90  &(\text{if}\quad |\tan| \geq 2.4142)\\
   135  &(\text{if}\quad -2.4142 < \tan  \leq -0.4142)
   \end{cases}
   $$

请使用`numpy.pad()`来设置滤波器的`padding`吧！

| 输入 (imori.jpg) | 输出(梯度幅值) (answers_image/answer_41_1.jpg) | 输出(梯度方向) (answers_image/answer_41_2.jpg) |
| :--------------: | :--------------------------------------------: | :--------------------------------------------: |
|  ![](imori.jpg)  |       ![](answers_image/answer_41_1.jpg)       |       ![](answers_image/answer_41_2.jpg)       |


### 第二步：边缘细化
边缘梯度的非极大值抑制，顾名思义就是判断边缘梯度在沿着边缘方向的邻域内是否为极大值，不是极大值则去除其边缘标记，以达到边缘细化的目的。非极大值边缘细化的原理是根据边缘方向$\text{edge}(x,y)$的值，判断当前梯度若不大于其周围两个像素梯度，则设置其梯度为0。
   $$
   \text{neighbor}(x,y)=\begin{cases}
   (x-1,y)\quad (x+1,y) & (\text{if}\quad \text{edge}(x,y) = 0)\\
   (x-1,y-1)\quad x(x+1,y+1) & (\text{if}\quad \text{edge}(x,y) = 45)\\
   (x,y-1)\quad (x,y+1)  &(\text{if}\quad \text{edge}(x,y) = 90)\\
   (x-1,y+1)\quad (x+1,y-1)  &(\text{if}\quad \text{edge}(x,y) = 135)
   \end{cases}
   $$
如果$\text{edge}(x,y)$和$\text{neighbor}(x,y)$不是最大值，则设置$\text{edge}(x,y)=0$。


| 输入 (imori.jpg) | 输出 (answers_image/answer_42.jpg) |
| :--------------: | :--------------------------------: |
|  ![](imori.jpg)  |  ![](answers_image/answer_42.jpg)  |

> 答案 
>
> - Python >> [answers_py/answer_42.py](answers_py/answer_42.py)
> - Python >> [answers_cpp/answer_42.cpp](answers_cpp/answer_42.cpp)

### 第三步：滞后阈值

在这里我们进行 Canny 边缘检测的最后一步。

[^TODO]: 这里原文中英文有错误

在这里我们将通过设置高阈值（HT：high threshold）和低阈值（LT：low threshold）来将梯度幅值二值化。

1. 如果梯度幅值$edge(x,y)$大于高阈值的话，令$edge(x,y)=255$；
2. 如果梯度幅值$edge(x,y)$小于低阈值的话，令$edge(x,y)=0$；
3. 如果梯度幅值$edge(x,y)$介于高阈值和低阈值之间并且周围8邻域内有比高阈值高的像素点存在，令$edge(x,y)=255$；

在这里，我们使高阈值为100，低阈值为20。顺便说一句，阈值的大小需要边看结果边调整。

上面的算法就是Canny边缘检测算法了。

| 输入 (imori.jpg) | 输出 (answers_image/answer_43.jpg) |
| :--------------: | :--------------------------------: |
|  ![](imori.jpg)  |  ![](answers_image/answer_43.jpg)  |

> - 答案 
>   - Python >> [answers_py/answer_43.py](answers_py/answer_43.py)
>   - C++ >> [answers_cpp/answer_43.cpp](answers_cpp/answer_43.cpp)

## 问题四十四：霍夫变换（Hough Transform）直线检测
霍夫变换，是将坐标由直角座标系变换到极座标系，然后再根据数学表达式检测某些形状（如直线和圆）的方法。当直线上的点变换到极座标中的时候，会交于一定的$r$、$t$的点。这个点即为要检测的直线的参数。通过对这个参数进行逆变换，我们就可以求出直线方程。

### 第一步：霍夫变换
方法如下：

1. 我们用边缘图像来对边缘像素进行霍夫变换。
2. 在霍夫变换后获取值的直方图并选择最大点。
3. 对极大点的r和t的值进行霍夫逆变换以获得检测到的直线的参数。

在这里，进行一次霍夫变换之后，可以获得直方图。算法如下：

1. 求出图像的对角线长$r_{max}$；

2. 在边缘点$(x,y)$处，$t$取遍$[0,179]$，根据下式执行霍夫变换：
   $$
   r_{ho}=x\  \cos(t)+y\  \sin(t)
   $$

3. 做一个$180\times r_{max}$大小的表，将每次按上式计算得到的表格$(t,r)$处的值加1。换句话说，这就是在进行投票。票数会在一定的地方集中。

这一次，使用`torino.jpg`来计算投票之后的表。使用如下参数进行 Canny 边缘检测：高斯滤波器$(5\times5,s = 1.4)$，$HT = 100$，$LT = 30$。

| 输入 (thorino.jpg) |   输出 (answers/answer_44.jpg)   |
| :----------------: | :------------------------------: |
|  ![](thorino.jpg)  | ![](answers_image/answer_44.jpg) |

> 答案 
>
> - Python >> [answers_py/answer_44.py](answers_py/answer_44.py)
> - C++ >> [answers_cpp/answer_44.cpp](answers_cpp/answer_44.cpp)

###第二步：NMS

我们将在这里进行第2步。

在上一步结果的表格中，在某个地方附近集中了很多票。这里，执行提取局部最大值的操作。

这一次，提取出投票前十名的位置，并将其表示出来。

NMS 的算法如下：
1. 在该表中，如果遍历到的像素的投票数大于其8近邻的像素值，则它不变。
2. 如果遍历到的像素的投票数小于其8近邻的像素值，则设置为0。

| 输入 (thorino.jpg) |   输出 (answers/answer_45.jpg)   |
| :----------------: | :------------------------------: |
|  ![](thorino.jpg)  | ![](answers_image/answer_45.jpg) |

> 答案 
>
> - Python >> [answers_py/answer_45.py](answers_py/answer_45.py)
> - C++ >> [answers_cpp/answer_45.cpp](answers_cpp/answer_45.cpp)

### 第三步：霍夫逆变换

这里是将上一步得到的极大值进行霍夫逆变换之后画出得到的直线。在这里，已经通过霍夫变换检测出了直线。

算法如下：
1. 极大值点(r,t)通过下式进行逆变换：
   $$
   y = - \frac{\cos(t)}{\sin(t) } \  x +  \frac{r}{\sin(t)}\\
   x = - \frac{\sin(t)}{\cos(t) } \  y +  \frac{r}{\cos(t)}
   $$

2. 对于每个局部最大点，使$y = 0-H -1$，$x = 0-W -1$，然后执行1中的逆变换，并在输入图像中绘制检测到的直线。请将线的颜色设置为红色$(R,G,B) = (255, 0, 0)$。

| 输入 (thorino.jpg) |   输出 (answers/answer_46.jpg)   |
| :----------------: | :------------------------------: |
|  ![](thorino.jpg)  | ![](answers_image/answer_46.jpg) |