---
title: 美赛小分析文章(25)
date: 2021-02-21 16:05:21
tags: 美赛
categories: 2021年美赛
---



# 美赛模拟赛或正赛中的一个小模块~

---



<!--more-->



为了便于表达下述关系矩阵，设数据中音乐家的数量为$\theta$，$\theta$的取值为5603。

我们从已知数据中，摘取得到了不同音乐家之间的关系矩阵$\bf{M}$。以音乐家的人数\theta作为矩阵的行数和列数。如果第$i$位音乐家对于第$j$位音乐家产生了影响，那么矩阵中$\bf M_{ij}$的值为1，否则为0。
$$
\bf{M} =  \{m_{ij}\}
$$


由此可知，关系矩阵的大小为$\theta \cdot \theta$，而其中的为1的元素个数，仅仅为4万多个，矩阵显然为稀疏矩阵。为了从矩阵中提取相对应的信息，我们采用了$Funk\_SVD$方法，来对于稀疏关系矩阵进行分解，探索关系矩阵背后隐藏的信息。

采用$Funk\_SVD$方法，意味着我们需要将原始的矩阵$M(\theta\cdot\theta)$拆分为两个矩阵$P(\theta\cdot{K})$和矩阵$Q(K\cdot\theta)$，括号内相乘的两个数分别表示矩阵的行数和列数，在操作时确定。分解后，我们考察分解结果结果是否准确。

首先，对于原始矩阵中不为0的位置$m_{ij}$，其在分解后对应的向量为$m^{'}_{ij}$，其满足的关系为：
$$
\bf {m^{'}_{ij}} = \sum_{k=1}^{K}p_{ik}q_{kj}
$$
对于整个矩阵而言，总的损失用均方差(SSE)来衡量:
$$
SSE = E^2 = \sum_{i=1}^{\theta}\sum_{j=1}^{\theta}(\bf m_{ij}-m^{'}_{ij})^2
$$
我们最小化上述的$SSE$，能够以最小的扰动完成对于原始评分矩阵的分解。分解过后，我们只需要通过计算$M^{'}$来完成对于原始评分矩阵的填充。为了求解使得$SSE$最小的矩阵$P$与$Q$，我们采用随机梯度下降法，对于上文中的目标函数$SSE$进行求解，最终得到了我们分解后的矩阵$P$和$Q$。

我们对于原始关系矩阵通过$Funk\_SVD$进行了不同的分解，当分解的因素为1时，我们我们对于分解后得到的矩阵$P$进行了归一化，最终得到了以下图像：

![image-20210205162228899](https://gitee.com/alexs-rabbit/picture/raw/master/20210205162228.png)

图中为分解后得到的矩阵$p$经过归一化后的值，我们定义其为音乐影响指标，表达的是他（她）对于其他音乐家的影响程度。图中的具体的点代表的是某个具体的音乐家，横坐标表示他（她）拥有的follower的人数，纵坐标表示它对应的音乐影响系数。可以看出，横纵坐标之间存在着联系。同时，我们也对于分解后得到的矩阵$Q$进行了归一化， 并且得到了下面的图像：

![image-20210205162209322](https://gitee.com/alexs-rabbit/picture/raw/master/20210205162209.png)

这里的y坐标，表示的是分解后得到的矩阵$q$经过归一化后，对应的值，这个值我们定义为被音乐影响系数，体现的是音乐家收到其他音乐家的影响程度。



除了将矩阵拆分为一个因素之外，我们还尝试了将矩阵拆分为三个因素，得到的$P$和$Q$我们分别进行了归一化处理，并且得到了以下的图像：

![image-20210205155220916](https://gitee.com/alexs-rabbit/picture/raw/master/20210205155220.png)

![image-20210205155229625](https://gitee.com/alexs-rabbit/picture/raw/master/20210205155229.png)

上下两图分别展示了，拆分后的矩阵$P$与$Q$归一化后的结果。我们发现，在上下两图中，拆分后得到的三个因素，与只用一个因素进行矩阵拆分后的结果相比，拆分后的三个因素表现出来都是正相关或是负相关的关系。这告诉我们，将原始矩阵以一个因素进行拆分足以表示数据的信息，也告诉我们，将原始矩阵以一个因素拆分并且进行分析是可靠的，可行的。

我们对于