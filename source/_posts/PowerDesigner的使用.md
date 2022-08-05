---
title: PowerDesigner的使用
date: 2021-04-11 20:36:44
tags: [Database,后端]
categories: 自学内容或课程作业
---





# 这玩意儿好用的很！！！

<!--more-->





## 数据库课程中常用

- 常用模型：

![image-20210411204221330](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411204221330.png)

分别读英语概念模型，逻辑模型和物理模型



### 概念模型：

- 实体和各种联系啊，继承啊之类的

![image-20210411204333014](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411204333014.png)

- 实体设置：

![image-20210411204404372](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411204404372.png)

![image-20210411204433756](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411204433756.png)

名字是中文名，而这里的英文名是后面生成代码的过程中用到的，建议一次取好，无论中英问最好不要重复。

- 关系设置：

![image-20210411204559638](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411204559638.png)

可以看上面的鸡爪爪，康康关系对不对嗷。

- 最终结果：

![image-20210411204637737](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411204637737.png)

- 效果如上



### 模型检查：

![image-20210411204737054](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411204737054.png)

这里可以检查模型设计好之后的关系啊之类的是否有问题，非常方便。



### 模型转换：

![image-20210411204852242](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411204852242.png)

设计好的逻辑模型，可以选中后点击对应的按钮，**直接转成想要的模型！！！**





### 逻辑模型：

主要是中转的过程，例如多对多的关系，逻辑模型会自动多生成一张表，但是这个表中只有一个外键的关系。如果简单当然可以由概念数据模型直接转换为物理，复杂的话，可能还要对于中间的逻辑模型（例如中间生成的那张表），进行一些补充。结果如下：



![image-20210411205138153](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411205138153.png)

这里的Identifier_1（下面这个貌似用处不大），[相应解释和操作](https://blog.csdn.net/g6465465/article/details/42372817)



### 物理模型：

和上面一样，可以选中逻辑或者概念模型生成物理模型，效果如下：

![image-20210411205958396](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411205958396.png)

这里转换成了物理实体了嗷！！！可以检查或者进行微调。注意生成过程中：

![image-20210411210111217](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411210111217.png)

- 选择对应的数据库，名字和code，code为生成对应表的英文名。

### SQL语句导出：

![image-20210411210217366](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411210217366.png)

左上角Database选项中选择生成database，然后选定位置生成SQL脚本即可，设计大功告成

- 注意，SQL代码有一些可能是用不上的，例如一开始有好多drop语句，如果你本来就没有对应的表就不用drop的嗷！！！
- mysql里面可以用source调用sql脚本的嗷！！！
- 这里sql的版本是你在上一步生成物理模型的时候选中的嗷！！！





### 个性化设置可以自己根据需求康康！！！

![image-20210411210538884](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411210538884.png)

- 里面好多设置好好玩的，可以康康嗷！！！