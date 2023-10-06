<!--
 * @Description: 
 * @Version: 1.0
 * @Author: Wang Hongping
 * @Date: 2023-09-29 11:47:14
 * @LastEditors: Wang Hongping
 * @LastEditTime: 2023-09-30 17:19:32
-->

#  图像分类

## 问题八十八：k-平均聚类算法（k -means Clustering）

问题84至问题87的图像识别任务是需要预期输出的简单监督学习（supervised-training）中的一种简单情况。在这里我们通过不需要预期输出的无监督学习（unsupervised-training）来进行图像分类。

最简单的方法是 k-平均聚类算法（k -means Clustering）。

k-平均聚类算法在类别数已知时使用。在质心不断明确的过程中完成特征量的分类任务。

k-平均聚类算法如下：

1. 为每个数据随机分配类；
2. 计算每个类的重心；
3. 计算每个数据与重心之间的距离，将该数据分到重心距离最近的那一类；
4. 重复步骤2和步骤3直到没有数据的类别再改变为止。

在这里，以减色化和直方图作为特征量来执行以下的算法：

1. 对图像进行减色化处理，然后计算直方图，将其用作特征量；
2. 对每张图像随机分配类别0或类别1（在这里，类别数为2，以`np.random.seed (1)`作为随机种子生成器。当`np.random.random`小于`th`时，分配类别0；当`np.random.random`大于等于`th`时，分配类别1，在这里`th=0.5`）；
3. 分别计算类别0和类别1的特征量的质心（质心存储在`gs = np.zeros((Class, 12), dtype=np.float32)`中）；
4. 对于每个图像，计算特征量与质心之间的距离（在此取欧氏距离），并将图像指定为质心更接近的类别。
5. 重复步骤3和步骤4直到没有数据的类别再改变为止。

### 第一步：生成质心
在这里，实现步骤1​至步骤3​吧（步骤4​和步骤5​的循环不用实现）！将图像`test@@@.jpg`进行聚类。

答案：

```bash
assigned label
[[ 1493  7892  4900  2099  1828  9127  4534   895  1554  6750  5406  2674 0]
[  242 10338  3628  2176   587 12212  2247  1338   434 10822  4506   622 1]
[ 6421  5478   719  3766  5482  4294  2537  4071  5609  4823  2051  3901 0]
[ 3343  8134  4756   151  3787  7588  3935  1074  3595  8444  4069   276 0]]
Grabity
[[ 3752.3333  7168.      3458.3333  2005.3334  3699.      7003.
3668.6667  2013.3334  3586.      6672.3335  3842.      2283.6667]
[  242.     10338.      3628.      2176.       587.     12212.
2247.      1338.       434.     10822.      4506.       622.    ]]
```

答案 >> [answers/answer_88.py](https://github.com/yoyoyo-yo/Gasyori100knock/blob/master/Question_81_90/answers/answer_88.py)

### 第二步：聚类（Clustering）

在这里完成算法的步骤4和步骤5，进行聚类吧！

在这里预测类别为0和1，但顺序与问题85​至87​不同。

因此，k-平均聚类算法是一种完全按范围划分类别的方法。一条数据最后被划分到什么类别只有到最后才清楚。此外，必须预先知道类别的数量。

需要注意的是，k-平均聚类算法最初分配的类别对最后的结果有很大的影响。并且，数据量小的情况下极有可能失败。也就是说，数据量越大最后得到的数据分布越准确。

答案：

```bash
test_akahara_1.jpg  Pred: 0
test_akahara_2.jpg  Pred: 1
test_madara_1.jpg  Pred: 0
test_madara_2.jpg  Pred: 0
```

答案 >> [answers/answer_89.py](https://github.com/yoyoyo-yo/Gasyori100knock/blob/master/Question_81_90/answers/answer_89.py)

### 第三步：调整初期类别

使用k-平均聚类算法将8张`train@@@.jpg`完美地聚类吧！

在这里，通过变更`np.random.seed()`的值和`np.random.random() < th`中分割类别的阈值`th`来更好地预测图片的类别吧！由于`train@@@.jpg`的图像数量是问题89​的两倍，因此可以更容易地聚类。

这只能通过反复试验来完成。

答案：

```bash
train_akahara_1.jpg  Pred: 1
train_akahara_2.jpg  Pred: 1
train_akahara_3.jpg  Pred: 1
train_akahara_4.jpg  Pred: 1
train_akahara_5.jpg  Pred: 1
train_madara_1.jpg  Pred: 0
train_madara_2.jpg  Pred: 0
train_madara_3.jpg  Pred: 0
train_madara_4.jpg  Pred: 0
train_madara_5.jpg  Pred: 0
```

答案 >> [answers/answer_90.py](https://github.com/yoyoyo-yo/Gasyori100knock/blob/master/Question_81_90/answers/answer_90.py)

## ISODATA

## 监督分类