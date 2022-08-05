---
title: 美赛小分析文章(22)
date: 2021-02-21 16:05:21
tags: 美赛
categories: 2021年美赛
---



# 美赛模拟赛或正赛中的一个小模块~

---



<!--more-->



为了便于表达下述关系矩阵，设数据中音乐家总数为$\theta$，根据数据集给出的所有音乐家的数量为5603，$\theta$的取值为5603。

我们从已知数据中，摘取得到了不同音乐家之间的关系矩阵$\bf{M}$。
$$
\bf{M} =  \{m_{ij}\},1\le i,j\le\theta
$$
以音乐家的人数$\theta$作为矩阵的行数和列数。如果第$i$位音乐家对于第$j$位音乐家产生了影响，那么矩阵中$\bf M_{ij}$的值为1，否则为0。

由此可知，关系矩阵有$\theta^2$个元素，而其中不为零元素个数，仅占 1.3% 左右，矩阵显然为稀疏矩阵。



**下面这里发生了改变哦！！！**



根据上述矩阵，我们构造有向图，以influence关系为有向边，将矩阵转换为有向图。转换为有向图后的效果如下图所示：



![image-20210206092439077](https://gitee.com/alexs-rabbit/picture/raw/master/image-20210206092439077.png)

由于网络较为复杂，此图显示的是id为335的音乐家以及其直接的follower，有向箭头表示影响关系，类似于上图所示网络，我们得到了音乐家之间复杂的关系网络。为了分析题目中的"音乐影响指标" ，我们对于网络中的每一个顶点（也就是对应的艺术家），赋予权重。我们采用$PageRank$方法，这种算法可以有效地根据构造的有向图，求出各个顶点的权值。我们对于音乐家i进行简要分析，简述$PageRank$的原理。



$PageRank$方法的原理如下:

初始阶段，我们为有向图的所有页面设置相同的$PageRank(pr)$值，经过若干轮的迭代计算，每个页面会获得最终的$pr$值，随着每一轮计算的进行，当前的$pr$值都会不断得到更新。

迭代更新权值过程中，我们采用$PageRank$公式来对于每个顶点的权值就行迭代，我们假设顶点i的权重值为$pr(i)$，顶点$j$的PR值$pr(j)$。这里的$j$是指向i的所有顶点中的某个顶点。$c(j)$指的是顶点j$的出度，也就是$$j$指向其他顶点的边的个数。d在这里的含义被定义为阻尼系数，表示的是，一个音乐家影响到某个follower之后，仍然会继续向follower的follower传播影响的概率。
$$
pr(i) = (1-d)+d(\frac{pr(j_1)}{c(j_1)}+\cdots+\frac{pr(j_n)}{c(j_{n})})
$$
经过多次迭代更新，算法结束之后，我们得到了每个音乐家的权重，并绘制了相应的图像：

![image-20210206093510702](https://gitee.com/alexs-rabbit/picture/raw/master/image-20210206093510702.png)

我们从图中可以发现，绝大多数的音乐家都处于follower较少并且权重较小的状态。少数音乐家，follower很多，影响较大，如图中最右边的孤立点为我们耳熟能详的beatles乐队，十分合理。同时，也有少数音乐家follower虽然少，但是他们直接或间接影响了其他follower特别多并且权重特别高的音乐家，导致这一部分音乐家的权重非常高，这也是符合我们的认识的。经过取样分析，我们认为$PageRank$的权重划分较为合理，效果较好。综合考虑后，我们将$PageRank$的权重定为每个音乐家的音乐影响系数。
