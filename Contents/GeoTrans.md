# 几何变换
## 最邻近插值（ Nearest-neighbor Interpolation ）

使用最邻近插值将图像放大$1.5$倍吧！

最近邻插值在图像放大时补充的像素取最临近的像素的值。由于方法简单，所以处理速度很快，但是放大图像画质劣化明显。

使用下面的公式放大图像吧！$I'$为放大后图像，$I$为放大前图像，$a$为放大率，方括号是四舍五入取整操作：
<img src="../assets/nni_fig.png">
$$
I'(x,y) = I([\frac{x}{a}], [\frac{y}{a}])
$$


## 双线性插值（ Bilinear Interpolation ）

使用双线性插值将图像放大$1.5$倍吧！

双线性插值考察$4$邻域的像素点，并根据距离设置权值。虽然计算量增大使得处理时间变长，但是可以有效抑制画质劣化。

1. 放大后图像的座标$(x',y')$除以放大率$a$，可以得到对应原图像的座标$(\lfloor \frac{x'}{a}\rfloor , \lfloor \frac{y'}{a}\rfloor)$。

2. 求原图像的座标$(\lfloor \frac{x'}{a}\rfloor , \lfloor \frac{y'}{a}\rfloor)$周围$4$邻域的座标$I(x,y)$，$I(x+1,y)$，$I(x,y+1)$，$I(x+1, y+1)$：
   
   <img src="../assets/bli_fig.png">
   
3. 分别求这4个点与$(\frac{x'}{a}, \frac{y'}{a})$的距离，根据距离设置权重：$w = \frac{d}{\sum\ d}$

4. 根据下式求得放大后图像$(x',y')$处的像素值：
$$
d_x = \frac{x'}{a} - x\\
  d_y = \frac{y'}{a} - y\\
  I'(x',y') = (1-d_x)\  (1-d_y)\  I(x,y) + d_x\  (1-d_y)\  I(x+1,y) + (1-d_x)\  d_y\  I(x,y+1) + d_x\  d_y\  I(x+1,y+1)
$$


## 双三次插值（ Bicubic Interpolation ）

使用双三次插值将图像放大$1.5$倍吧！

双三次插值是双线性插值的扩展，使用邻域$16$像素进行插值。

<img src="../assets/bci_fig.png">

各自像素间的距离由下式决定：
$$
\begin{align*}
d_{x_1} = |\frac{x'}{a\  x} - (x-1)|\quad 
d_{x_2} = |\frac{x'}{a\  x}- x| \quad 
d_{x_3} = |\frac{x'}{a\  x}- (x+1)|\quad 
d_{x_4} = |\frac{x'}{a\  x} - (x+2)|\\
d_{y_1} = |\frac{x'}{a\  y} - (y-1)|\quad 
d_{y_2} = |\frac{x'}{a\  y} - y| \quad 
d_{y_3} = |\frac{x'}{a\  y} - (y+1)| \quad 
d_{y_4} = |\frac{x'}{a\  y} - (y+2)|
\end{align*}
$$
权重由基于距离的函数取得。$a$在大部分时候取$-1$。大体上说，图中蓝色像素的距离$|t|\leq 1$，绿色像素的距离$1<|t|\leq 2$：
$$
h(t)=
\begin{cases}
(a+2)\ |t|^3 - (a+3)\ |t|^2 + 1 &\text{when}\quad |t|\leq 1  \\
a\ |t|^3 - 5\  a\ |t|^2 + 8\  a\  |t| - 4\  a&\text{when}\quad 1<|t|\leq 2\\
0&\text{else}
\end{cases}
$$
利用上面得到的权重，通过下面的式子扩大图像。将每个像素与权重的乘积之和除以权重的和。
$$
I'(x', y')=\frac{1}{\sum\limits_{j=1}^4\ \sum\limits_{i=1}^4\ h(d_{xi})\ h(d_{yj})}\ \sum\limits_{j=1}^4\ \sum\limits_{i=1}^4\ I(x+i-2,y+j-2)\ h(d_{xi})\ h(d_{yj})
$$

## 仿射变换（ Afine Transformations ）
仿射变换利用$3\times3$的矩阵来进行图像几何变换。变换的方式有平行移动、放大缩小、旋转、倾斜等。
原图像像素点坐标记为$(x,y)$，变换后的图像像素点坐标记为$(x',y')$。图像放大缩小矩阵为下式：
$$
\left(
\begin{matrix}
x'\\
y'
\end{matrix}
\right)=
\left(
\begin{matrix}
a&b\\
c&d
\end{matrix}
\right)\ 
\left(
\begin{matrix}
x\\
y
\end{matrix}
\right)
$$
另一方面，平行移动按照下面的式子计算：
$$
\left(
\begin{matrix}
x'\\
y'
\end{matrix}
\right)=
\left(
\begin{matrix}
x\\
y
\end{matrix}
\right)+
\left(
\begin{matrix}
t_x\\
t_y
\end{matrix}
\right)
$$
把上面两个式子盘成一个：
$$
\left(
\begin{matrix}
x'\\
y'\\
1
\end{matrix}
\right)=
\left(
\begin{matrix}
a&b&t_x\\
c&d&t_y\\
0&0&1
\end{matrix}
\right)\ 
\left(
\begin{matrix}
x\\
y\\
1
\end{matrix}
\right)
$$
但在实际操作的过程中，如果逐个计算原图像的像素的话，处理后的像素可能在原图像中没有对应的坐标。因此，我们有必要对处理后的图像中各个像素进行仿射变换逆变换，取得变换后图像中的像素在原图像中的坐标。仿射变换的逆变换如下：
$$
\left(
\begin{matrix}
x\\
y
\end{matrix}
\right)=
\frac{1}{a\  d-b\  c}\ 
\left(
\begin{matrix}
d&-b\\
-c&a
\end{matrix}
\right)\  
\left(
\begin{matrix}
x'\\
y'
\end{matrix}
\right)-
\left(
\begin{matrix}
t_x\\
t_y
\end{matrix}
\right)
$$
这回的平行移动操作使用下面的式子计算。$t_x$和$t_y$是像素移动的距离。
$$
\left(
\begin{matrix}
x'\\
y'\\
1
\end{matrix}
\right)=
\left(
\begin{matrix}
1&0&t_x\\
0&1&t_y\\
0&0&1
\end{matrix}
\right)\  
\left(
\begin{matrix}
x\\
y\\
1
\end{matrix}
\right)
$$

### 放大缩小

1. 使用仿射变换，将图片在$x$方向上放大$1.3$倍，在$y$方向上缩小至原来的$\frac{4}{5}$。
2. 在上面的条件下，同时在$x$方向上向右平移$30$（$+30$），在$y$方向上向上平移$30$（$-30$）。


### 旋转
1. 使用仿射变换，逆时针旋转$30$度。
2. 使用仿射变换，逆时针旋转$30$度并且能让全部图像显现（也就是说，单纯地做仿射变换会让图片边缘丢失，这一步中要让图像的边缘不丢失，需要耗费一些工夫）。

使用下面的式子进行逆时针方向旋转$A$度的仿射变换：
$$
\left(
\begin{matrix}
x'\\
y'\\
1
\end{matrix}
\right)=
\left(
\begin{matrix}
\cos(A)&-\sin(A)&t_x\\
\sin(A)&\cos(A)&t_y\\
0&0&1
\end{matrix}
\right)\ 
\left(
\begin{matrix}
x\\
y\\
1
\end{matrix}
\right)
$$

### 倾斜

1. 使用仿射变换，输出（1）那样的$x$轴倾斜$30$度的图像（$t_x=30$），这种变换被称为X-sharing。
2. 使用仿射变换，输出（2）那样的y轴倾斜$30$度的图像（$t_y=30$），这种变换被称为Y-sharing。
3. 使用仿射变换，输出（3）那样的$x$轴、$y$轴都倾斜$30$度的图像($t_x = 30$，$t_y = 30$)。

原图像的大小为$h\  w$，使用下面各式进行仿射变换：

* X-sharing
  $$
  a=\frac{t_x}{h}\\
  \left[
  \begin{matrix}
  x'\\
  y'\\
  1
  \end{matrix}
  \right]=\left[
  \begin{matrix}
  1&a&t_x\\
  0&1&t_y\\
  0&0&1
  \end{matrix}
  \right]\ 
  \left[
  \begin{matrix}
  x\\
  y\\
  1
  \end{matrix}
  \right]
  $$

* Y-sharing
  $$
  a=\frac{t_y}{w}\\
  \left[
  \begin{matrix}
  x'\\
  y'\\
  1
  \end{matrix}
  \right]=\left[
  \begin{matrix}
  1&0&t_x\\
  a&1&t_y\\
  0&0&1
  \end{matrix}
  \right]\ 
  \left[
  \begin{matrix}
  x\\
  y\\
  1
  \end{matrix}
  \right]
  $$
