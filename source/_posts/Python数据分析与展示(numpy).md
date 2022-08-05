---
title: Numpy相关内容
date: 2021-02-20 16:05:21
tags: [Python,美赛]
categories: [2021年美赛,Python]
---



# 是Numpy鸭!

---



<!--more-->



# Python数据分析与展示(numpy):

### 1.

### MOOC第一周内容:

###  anaconda的使用，继承环境，很好用

### 2. 

### MOOC第二周内容:

- numpy这个内置的软件有很多的功能用于数据统计的（详情见mooc1.py）:
  - 1. 一个强大的N维数组对象ndarray
    2. 广播功能函数
    3. 整合C/C++等代码的工具
    4. 线性单数，傅里叶变换，随机数生成等
  
- 数据的分类：一维，二维，高维（通过二维的键值对等来实现的）

- ndarray:
  - 要求所有元素类型相同，数组下标从0开始。

  - axis: 保存数据的维度    rank: the num of axis

  - .ndim   rank

  - .shape   对象的尺度，对于矩阵就是n行m列

  - .size   ndarray中对象元素的个数，相当于.shape中n*m的值

  - .dtype   元素类型

  - .itemsize   每个元素的大小

  - bool , intc , intp , int8 , int16 , int32 , int64（还包括uint,float,complex等等） 这些都是ndarray的元素类型

  - 非同质对象确实可以构成ndarray数组，但是无法有效发挥NumPy的又是，尽量避免使用

  - 注意哈,ndarray传入的东东只能是一个列表，这个列表内可以含有别的内容和列表。

  - 创建ndarray:

    - 1. x = np.array(list/tuplr,dtype=np.float32)
      2. 你不指定dtype时，ndarray会自动去设置
      3. 创建数组时，还可以使用列表和元组混合类型搭配嗷！

    - 1. np.arrage(n),类似于range(n)
      2. np.ones(shape)  生成一个全1数组
      3. np.zeros(shape)  生成一个全0数组
      4. np.full(shape,val)   根据shape生成一个array，每个元素值都是val
      5. np.eye(n)   创建一个n*n单位矩阵，对角线为1，其余为0
      6. np.ones_like(a), np.zeros_like(a),np.full_like(a,val):

      根据数组a的shape生成一个（全0，全1，全val）的数组

      7. np.linspace(),np.concatenate():

      第一个是用于等距离的填充数据形成数组，第二个是用于将两个或者多个数组合并成一个新的数组

      linspace(1,10,4,endpoint=False): endpoint指明了最后一个元素算不算在内，然后均分成多个元素

      

      mention: 这里的shape是一个元组类型，指明了几行几列。

  - 维度变换：

    - .reshape(shape)（实现数组的copy，可以对于元素机型重新分组）
    - .resize(shape)(实际上改变了原数组)
    - .swapaxes(ax1,ax2)(交换两个axis)
    - .flatten()(降维打击，直接降维到一维)
    - .astype(new_type)(创建一个新数组)
    - .tolist()(数组变换为列表)

  - ndarray的操作:

    - 索引与切片：
      1. 对于一维数组来说：索引一模一样
      2. 对于一维数组来说：切片a[1:4:2]从[1,4)以步长为2的元素
      3. 多维数组的索引：a[1,2,3]从外往内去index，操作起来就很方便，同时a[-1,-2,-3]也可以这么操作
      4. 多维数组的切片：a[:,1,::2]: 不关心第一个维度，第二个维度我只要第二个，第三个维度上我要以2为步长提取元素，两个冒号的逻辑对于每一层数组都是存在的

  - ndarray数组的运算：

    - 插播一下，惯用生成数组的方法: np.arange(24).reshape((2,3,4))
    - .mean()获得a中所有元素的平均值
    - .abs()
    - .sqrt()
    - .square()
    - .log2(x) , .log(x)(自然对数)
    - .ceil(x)    .floor(x)
    - .rint(x)   对于每个元素进行四舍五入
    - .modf(x)   将小数与整数以两个个独立数组形式返回
    - .sin(x)   等三角函数都有
    - .exp(x)
    - .sign(x)   计算各元素的符号值

    注意下: 实测用法应该为   a = np.square(另一数组名)

    numpy的理念： 将数组当成实际上一个数字来进行处理，使逻辑清晰化

    - numpy二元函数的计算:
      - +-*/**
      - np.maximun(x,y) / np.minimun(x,y)
      - np.mod(x,y)
      - np.copysign(x,y)
      - < > <= >= !=(对于对应元素进行比较并且得出结果)
    - 本质上ndarray弥补了python中没有数组的缺憾

- CSV文件：

  - 写入CSV:

    - np.savetxt(frame,array,fmt='%.18e',delimiter=None)

    frame: 文件，字符串或产生器，可以是.gz文件

    array: 写入文件的数组

    fmt: 写入文件的格式， 例如： %d   %.2f   %.18e

    delimiter: 分割字符串，默认是任何空格

    tips: 由于CSV的广泛使用性，为了保存成CSV文件，delimiter设置为','

  - 读取CSV:

    - np.loadtxt(frame,dtype=np.float,delimiter=None,unpack = False)

    frame: 文件，字符串或产生器，可以是.gz文件(处理大文件的时候可能用到)

    - delimiter一般为',',读入的如果是csv文件的话
    - unpack如果是True,读入属性将分别写入不同变量

  - CSV局限性：

    - 只能存取一维或者二维的数据

- 多维数据的存取：

  - a.tofile(frame,sep='',format='%s')

  frame: 文件，字符串

  sep: 数据分割字符串，如果是空串，写入文件为二进制

  format: 写入数据的格式

  - a.fromfile(frame,dtype,count=-1,sep='')

  count=-1默认读如整个文件

  - 注意写入文件的时候会造成维度信息的丢失，因此读取出来的时候要用.reshape来还原
  - 可以再写一个文件，存取多余的信息，来使得存入的文件得以还原
  - 在存储和提取大文件时使用

- Numpy的便捷文件存取：

  - np.save(fname,array)   np.savez(fname, array)

  可以是可以，但是文件的拓展名为.npy或者是.npz，如果不介意当然可以使用嗷！

  - 读取的时候，np.load(fname)(可以完全还原哦！)

- numpy中的随机数库:

  - 使用的时候： np.random.函数名（参数）
  - rand(d0,d1,d2,...,dn)   随机数均匀分布
  - randn(d0,d1,...,dn)   随机数符合正态分布
  - randint(low[,high,shape])   从low到high抽取   e.g.np.random.randint(100,200,(3,4))
  - seed(s)   e.g.   np.random.seed(10)随后再去调用上面的函数

  - 高级函数： 
    - 使用的时候： np.random.函数名（参数）
    - shuffle(a)   对于数组a第一轴进行堆积排列，改变数组x
    - permutation(x)   同上，不过产生一个新的数组而不改变原数组
    - choice(a[,size,replace,p])
  - 其他一些用法
    - uniform(low,high,size)   --均匀分布
    - normal(loc,scale,size)   --正态分布
    - poisson(lam,size)   ---泊松分布

- numpy中的统计函数

  - 使用: np.*

  - 常用: sum()   mean()   average(默认是等概率，可以改weights来实现加权计算)   std(标准差)   var(方差)

    mention: 对于以上的五个函数来说 (axis是标配参数，默认为none，即为所有元素，但是你也可以指定axis，对其中一个维度进行计算)

  - np.random的统计函数：

    - min,max
    - argmin(a),argmax(a): 计算最大or最小的降一维后的下标
    - unravel_index(index,shape): 根据shape将一维下标转换为多维下标

    e.g. np.unravel_index(np.argmax(b),   b.shape)

    - ptp(a): 计算最大与最小的差
    - median(a): 计算中值
    - 梯度函数： np.gradient(f)，计算数组f中元素的梯度，当f为多维时，返回每个维度梯度。（多维数组会生成多个维度的值，反映了每个维度上的梯度，反映了元素的变化率）（非常非常有用，后面会学到嗷！）
  
- 图像处理：

  - 图像的本质： 不同的RGB值构成的，三维数组构成，位置加上
  - 图像处理：
    - from PIL import Image
    - 可以调用: Image.open('path')
    - 经常的用法：a = np.array(Image.open("path"))
    - 基本流程：
      - 打开图像
      - 对于RGB进行运算
      - 对于运算后的数组保存为图像类型
    - 详情见mooc3.py



