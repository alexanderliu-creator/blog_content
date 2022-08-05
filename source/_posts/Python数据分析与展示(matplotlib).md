---
title: Matplotlib相关内容
date: 2021-02-20 16:05:21
tags: [Python,美赛]
categories: [2021年美赛,Python]
---



# 是Matplotlib鸭!

---



<!--more-->





# Python数据分析与展示(matplotlib):

<https://matplotlib.org/gallery.html>

## 上面这个是可以看到相应的效果图去找库的使用的库，不懂的话可以上去查一下嗷！



可以仅仅通过.pyplot实现可视化的库，非常方便简单，相当于快捷方式，一般我们也是用这个子库。



# PLT中的一些功能：

- 简单绘图: .plot(list)
- 设置坐标轴: .xlabel   /   .ylabel
- 保存文件   .savefig('name',dpi = ... )(dpi可以修改输出质量，每个英寸含有多少像素点)
- .axis([a , b , c , d])   x坐标轴起始于a,终止于b ; y坐标轴起始于c,终止于d(确定x,y轴的范围)
- .subplot(nrows , ncols , plot_number)(1,2参数分区，3参数定位绘图区域)
- .show()展示画出来的图像

参照mooc5.py有一些用法，个人认为和matlab没啥大区别，只是matlab中的数组在python中只能通过numpy库来实现罢了，其余的用法大同小异。

- .plot函数的使用：

  - .plot(x,y,format_string,**kwargs):
    - x,y: 列表或者数组
    - format_string: 控制曲线的格式字符串，可选。（系统会自动自己选的哦，风格字符最好换一换哦，上网可以查得到，MOOC第二周也可以查得到，mooc6.py参考）
    - **kwargs: 另外一些参数
    - 可以同时绘制多条   e.g.   plt.plot(a,a\*1.5,   a,a\*2.5)
    - format_string使用时可以指定参数：
      - color = 'green'
      - linestyle = 'dashed'
      - marker = 'o'
      - markerfacecolor = 'blue'
      - markersize = 20
      - ...

- pyplot中文输出：

  - 第一种方法:
    - import matplotlib
    - matplotlib.rcParams['font.family'] = 'SimHei'(修改字体为黑体)
    - 随后输入中文就可以正常显示
    - rcParams还有font.style和font.size的属性
    - SimHei , Kaiti , LiSu , FangSong , Youyuan , STSong 都可以输出中文
  - 第二种方法（推荐，不用像上面那样全局设置，可以个性化定制）：
    - 因为上面第一个是全局引用，但用fontproperties则是可以改变某一个部分的属性。
    - 例子：plt.xlabel('横轴', fontproperties = 'SimHei',fontsize = 20)

- 文本显示函数：

  - 一些基本函数：

    - .xlabel
    - .ylabel
    - .title()
    - .text()
    - .annotate()

    可以见mooc7.py康康具体表示怎么样比较好一点哦

- pyplot子区域绘图:

  - .subplot2grid(GridSpec,CirSpec,colspan=1,rowspan=1)

    - GridSpec为一个元组，分成几乘几的区域
    - CirSpec为开始指定的区域
    - colspan或者是rowspan便是行或者列上延展几个单位的区域，如果不选择这个参数的话，就默认一个哦

    通过这种方式，就可以使得plot的个性化更加明显一点

  - 还有一个类：

    - import matplotlib.gridspec as gridspec

    - 用法:

      - gs = gridspec.GridSpec(3,3)
      - ax1 = plt.subplot(gs[0, :])
      - ax2 = plt.subplot(gs[1,:-1])

      

- 针对于数据，寻找优秀的展示方法，然后去matplotlib中查找对应的用法即可。



- 十六种常用的函数: 

  - 1. plot
    2. boxplot（箱型图）
    3. bar（条形图）
    4. barh（横向条形图）
    5. polar(极坐标图)
    6. pie(饼图)
    7. psd(功率谱密度图)
    8. specgram(谱图)
    9. cohere(X-Y的相关性函数)
    10. scatter(散点图)
    11. step(步阶图)
    12. hist(直方图)
    13. contour(等值图)
    14. vlines(垂直图)
    15. stem(柴火图)
    16. plot_data(数据日期)

    tips: 这里是告诉你什么数据要用什么图，重点不在于怎么绘制



- 引入一个十分有用的方法，面向对象的方法来进行绘制嗷！首先例如ax = subplots()

这样构造了一个对象ax , 然后用ax.plot()，这个时候调用的是生成的ax这个对象的方法，通过这个对象的方法来对于这个图像进行绘制！ax.set_title()也是其中面向对象的一个函数。



- 饼图：
  - plt.pie()这个函数用于绘制饼图
  - 基本参数: labels , sizes , explode , autopct , shadow , startangle , axis
  - 有比例的数据比较方便绘制，突出各元素的百分比时适合用饼图
- 直方图
  - plt.hist()
  - 关键参数:
    -  我传入的数组
    - 参数bin，直方图中直立的块儿的个数（等分在最小和最大值之间）
  - 可以很好的将数组中具体数据个数给展示出来，突出样本在某一范围内的数量时适合采用直方图（例如：班级数学期末考试成绩）
- 极坐标图的绘制：
  - 关键参数：
    - projection = 'polar'
    - theta , radii , left , height , width 
    - 在角度范围内展示数据的好方法！
- 散点图：
  - 具体的一些数据，让其在图中突出其重点的一些数据信息，统计的观测值适合采用散点图来展示。
- 折线图：
  - 时间维度可以使用折线图，有利于突出数值随着时间的变化。

