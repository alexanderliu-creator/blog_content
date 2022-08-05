---
title: Pandas相关内容
date: 2021-02-20 16:05:21
tags: [Python,美赛]
categories: [2021年美赛,Python]
---



# 是Pandas鸭!

---



<!--more-->



[Pandas官网嗷](http://pandas.pydata.org)

## Series类型:（一维数据类型）

- 一组数据和其索引构成:

  - ```python
    import pandas as pd
    b = pd.Series([9,8,7,6],index=['a','b','c','d'])
    ```

  - index为自定义索引

  - 如果不定义index的话系统会自动从0开始帮你定义

- 创建:

  - 从标量值创建(要给个index，对应的是同一个标量)

  - 从字典类型创建（直接传递给Series就好了）

    - 也可以重新给一个index, index与字典中的key相匹配，多余的赋值为NaN(None)

    可以看作index从字典中挑选相关的元素来构造了一个index的Series类型

  - 列表（上面就是例子）

  - ndarray（这个也可以，只要你构造好）

  - 其他函数

- 基本操作：

  - .index获得索引
  - .values获得数据
  - 索引有两个：默认的为从零开始的自动索引，另一个是你定义的索引，都可以使用，但是不能够混用。
  - 其余特点:
    - 索引方式都是用【】
    - numpy中运算和操作可作用于Series类型
    - 可以通过自定义索引的列表进行切片。
    - 自动索引和自定义索引会一同被切片
  - 对于Series类型的处Series理得到的都是Series类型，本质上是对于value进行处理，不改变index的对应关系
  - 可以使用字典中的 in / .get(index) / .

- 对齐问题：

  - Series + Series
  - 在两个索引中都存在的，value相加，只有一个存在，则index对应的值为NaN

- name属性：

  - series的对象和索引都可以有一个名字，名字存在.name和.index.name中

- Series对象可以随时修改并且立即生效

- Series本质是一维带“标签”的数组，操作时根据索引对齐。





## DataFrame类型：（二维数据类型）

- 索引+多列数据(index+column),时一个表格型的数据类型，行索引为index，列索引为axis

- 创建:

  - 二维ndarray
  - 一维ndarray, 列表，字典，元组或者Series构成的字典
  - Series类型
  - 其他DataFrame类型
  - 也可以从列表类型的字典创建（列表类型的字典每一个元素就是一列，然后根据元素个数去设置横向的index个数，不足的话也会自动补齐）

  反正就是两个维度上设置index嘛，方便读取数据，index就是行索引，columns则是设置的列索引，数据会根据行列索引自动补齐。

- 一些数值的读取:

  - .index
  - .columns
  - .values
  - d['column']
  - d.ix['index']
  - d['column']\['index']

- DataFrame本质上时二维带 “标签” 数组

 

 

## 以上两个东西的一个总结提高:

- 如何改变Series和DataFrame对象？
- 重新索引：
  - reindex()，直接给出你想要的index和column顺序即可
  - fill_value: 用于填充缺失位置的值
  - method: 填充方法
  - .columns.insert(pos,value)
- 索引类型的常用方法：
  - .append(idx)
  - .diff(idx)
  - .intersection(idx)
  - .union(idx)
  - .delete()
- 面向对象的方法：a.columns.operation() / a.index.operation()
- .drop()可以删除Series和DataFrame指定行或列的索引





## Pandas库中的运算:

- 运算规则： 根据补齐后的数据进行运算，运算默认产生浮点数。

- 有一些运算方法:

  - .add
  - .sub
  - .mul
  - .div

  采用这些方法可以很有效的添加一些参数，例如fill_value，使得多样性得以实现

- 不同维度的运算为广播运算，一维Series默认在轴1参与运算。如过要其他的与之进项运算的话，要用add后则会sub方法才能实现，应该写成axis = 0,就是每一列都减去对应的一列。

- 比较运算: 

  - 只有同维度才能进行比较运算
  - 比较运算若在不同维度则为广播运算，默认在1轴上

- Series =  index + 一维数据

- DataFrame =  行列索引 + 二维数据

- 数据的排序：

  - 数据的理解：从数据中摘要得到想要的数据：
    - 基本统计
    - 分布/累计统计
    - 数据特征：相关性，周期性等
    - 数据挖掘（形成知识）
  - 排序方法：
    - .sort_index()：对于索引进行排序(axis)，默认升序(ascending)（默认为0轴，也就是纵向方向）
    - Series.sort_values(axis,ascending)
    - DataFrame.sort_values(by,axis,ascending),by时通过某个索引进行排序
    - NaN同意放在排序的末尾

- 基本统计分析:

  - .sum  .count   .mean()   .median()   .var()   .std()   .max()   .min()   
  - 默认操作于0轴，可以这么理解，Series只有一个维度默认时零维，所以DataFrame有两个维度但是默认操作的也是零维度，所以是纵向的。
  - .argmin(),argmax()   计算最大/最小的索引（自动索引）
  - .idxmin(),idxmax()   计算最大/最小的索引（自定义索引）
  - .describe()   返回多维的统计信息，如果是series就返回series，如果是dataframe就返回dataframe.





## Pandas中累计统计分析函数:

- 统计分析函数：

  - .cumsum():  计算和
  - .cumprod()：计算积
  - .cummax()：计算最大值 
  - .cummin()： 计算最小值

- 滚动计算函数：

  - .rolling(w).sum()：   相邻w个元素的和
  - .rolling(w).mean()：   相邻w个元素的算数平均值
  - .rolling(w).var()：   相邻w个元素的方差
  - .rolling(w).std()：   相邻w个元素的标准差
  - .rolling(w).min().max()：   相邻w个元素的最小值和最大值

  默认都是从零轴开始广播运算，就是纵向的运算。





## 数据的相关分析:

- 相关性：正相关，负相关，不相关

- 原理：协方差 => Pearson相关系数

- 计算方法：

  - .cov(): 计算协方差矩阵
  - .corr(): 计算相关系数矩阵

  用series/dataframe.corr()就可以计算出对应的相关系数





