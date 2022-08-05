---
title: ML入门
date: 2022-01-16 19:11:14
tags: 大三自学
---



# 对于ML基础课程的学习

1. Regression or classification
   - Regression： 连续，回归问题，根据很多值的情况去拟合，判断其中某个值的情况
   - Classification：分类，只有多个特定的tag，根据input来判断属于某一个tag
2. Supervised Learning or unsupervised learning
   - Supervised: Training set knows the answers !!!
   - Unsupervised: don't know the results -> 例如聚簇
3. Linear Regression（ax + b去拟合数据而已嘛）
4. Cost Function：衡量预判结果和真实结果的差距。常用的误差函数：
   - 误差平方代价函数：
     - 为了使这个值不受个别极端数据影响而产生巨大波动，采用类似方差再取二分之一的方式来减小个别数据的影响。
     - 误差平方代价函数是线性回归常用的Cost Function
   - Target : we want to find a hypothesis that make the cost lowest. -> find the minimum of the cost function.
     - 使用高中的知识，对于函数上的一个点，如果它不是极值点/最值点的时候，导数不为0。我们可以采用导数的方法，找到最小的导数，然后把当前的参数点，向导数最小的方向“挪动”一点。
   - 上面提到的这种方式就是Gradient descent：
     - 思想：不断改变参数，直到Cost Function尽量小
     - 初始参数不重要的，因为最终都可以移动到的嗷！
     - 走到的不一定是全局最优点，但是是局部最优点
5. Gradient Descent
   - $a$是我们常说的学习率，也就是“迈步子下山”的步子的大小  
     - 本质就是每次往“最优解”走一步，然后再次计算“最优解”的方向，再向最优解走。。。
     - 如果太大，可能导致无法收敛嗷！！！
     - 如果处于极值点，算法其实结果就不会变了，结果就是相对稳定的了嗷！！！(因为算法调整的那一项为0了嗷！！！)
     - convex function只有一个全局最优解（一个极值点也是最值点），这种时候的结果一定是正确的结果嗷！！！
6. Matrix and Vector
   - 矩阵运算不可交换
   - 矩阵运算可以结合律
   - 单位矩阵乘以某个矩阵就是那个矩阵，交换律对于I成立嗷！！！ 
   - 多元回归可以表达为两个矩阵的乘积嗷！
7. Inversion of a matrix and transposition of a matrix:
   - 矩阵的逆运算和矩阵的转置
   - Only n by n matrix has inversion.
8. 梯度下降（Gradient descent）处理多元线性回归

   - 更新规则：对于每一个变量求偏导，进行梯度下降
   - 两个重要的技巧：
     1. 特征缩放（Feature Scaling）：特征取值差异过大，人为采取类似于除以最大值来处理。比较合适的范围是[-3,3]或者[-1/3,1/3]
     2. Mean normalization：
        - (x - average value of x in training set) / (max value - min value)
   - learning rate：
     - 一步走多远？通过J($\theta$)和迭代次数的变化，可以判断大小。如果上升就是小了，如果下降就对了，如果相对稳定了则就收敛了嗷！
     - 太大了会冲过最低点，太小了收敛太慢了嗷！
     - 一般每隔10倍尝试一个值嗷！！！0.003 ， 0.03 ， 0.3 ， 3 ， 30 ， 300
9. 特征和多项式回归：

   - 比如我们使用了长，宽，和面积作为三个参数，面积可能非常大，因此特征缩放和重要嗷！！！
10. 正规方程（Normal Equation）：

    - 求损失函数的最小值（回到了高中知识，导数为0。）
    - 类似的，看XMind，通过矩阵计算的方式，我们就能够直接得到我们的$\theta$矩阵
    - 不可逆？工具可以解决嗷！
11. Logistic Regression

    - 普通的一次函数受远端的特殊点影响大，不太适合于classification的问题。（Classfication可以用Logistic函数嗷！！！）
    - Logistic函数由于其特殊的函数原因，通常被我们用于classification
    - Decision boundary（决策边界）：$\theta$x，放在了Logistic函数对应的位置上嗷！！！大于0，那整体函数的值就大于0.5，我们就判断它是1嗷！！
    - Cost Function：和Linear Regression的代价函数不一样，Why？Logistic最好找的是一个凸函数，非凸函数找最值点不方便嗷！$Cost(h_{\theta},y) = -ylog(h_{\theta}(x) - (1-y)log(1-h_{\theta}(x)))$，然后一样通过梯度下降操作，一样进行缩放等。
    - 体会一下，我们函数的输出是一个0-1之间的值，我们需要人为Classification简化并且输出0或者1，但是损失函数是可以用上面的cost函数来计算的嗷！
12. 高级优化：很多算法很高级，一般都比gradient descent下降的快。虽然复杂，但是效果好嗷，调用就好嗷，包括矩阵计算调用已有的方法。
13. 多元分类问题：把数据分为自己这一类和除自己类的其它类，使用这种方法来训练多个分类器（Classifiers）。然后每次训练取最好的那个那个分类器就好的嗷！
14. 过拟合问题：

    - 拟合不好称为Underfit or High bias，训练的好称为Just right，训练过了就称为Overfit or High variance

    - 导致的原因：特征太多，训练数据太少了orz

    - 解决过拟合问题：

      1. Reduce number of features:
         - Manually select which features to keep

         - Model selection algorithm(later in course)

      2. Regularization:

         - Keep all the features , but reduce magnitude / values of parameters(Idea of regularization)
         - Works well , when we have a lot of features , each of which contributes a bit to predicting y
15. Cost Function and regularization

    - 惩罚项 -> 减小参数的值，一般来说，参数越小，曲线就越平滑嗷！！！（越不容易过拟合）
    - 常数项$\theta_0$一般不用正则化嗷！！！

    - 在代价函数后面加上惩罚项，每个维度都要加！！！并且前面有个系数$\lambda$，这个参数用来平衡训练效果（拟合精度）和过拟合精度。
    - 线性回归正则化：
      - 和上面一样加入正则项，我们可以求出它的参数更新公式，加深理解。
      - 矩阵正规化：解决上面提到的不可逆问题嗷！！！
    - Logistic正则化：
      - 一样的，在损失函数后面加上惩罚项
      - 然后根据损失函数求导就可以得到梯度！有了梯度就可以下降！
16. 神经网络：
    - **复杂非线性假设**可以用神经网络来解决嗷！例如CV这种东西，计算机眼中都是像素，如果采用老式的方法，计算量巨大，不可能用上面的Linear Regression and Logistic Regression来实现嗷！！！
