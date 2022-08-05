---
title: JSP学习
date: 2021-04-21 17:24:55
tags: 后端
categories: 动力结点后端课程
---





## JSP规范学习，这一节内容紧接着Servlet，用到了Servlet的内容嗷。终于到SSM了。

<!--more-->



## JSP介绍

- JSP负责将数据处理结果写入响应体中。
- 适合于处理结果过多的情况，不然在Tomcat中写入数据结果，太多了非常复杂。
- JSP文件执行的时候，自动将文件所有内容都写入到响应体中，就很方便呀！！！
- 默认Web中如果没有index.html，就会自动寻找index.jsp来展示。



![image-20210421174322606](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210421174322606.png)

JSP中自带的一些属性，帮助你设置响应头的。



## JSP中写JAVA命令的书写:

### 如何声明JAVA语言：

![image-20210421175345147](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210421175345147.png)

- 可以写：
  - Java变量
  - 数学，关系，逻辑运算
  - 控制语句



### 如何导入将变量写入页面：

- 响应体中导入对应的标记，通知JSP将变量的值写入响应体中

![image-20210421175652150](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210421175652150.png)

- 可以写：
  - 变量值
  - 变量计算结果
- 输出标记`<%=num1+num2>`





### 如何导包：

- 快捷键：`Alt + Enter`，自动导入包。
- 包被导完之后：

![image-20210421190143086](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210421190143086.png)

- 会自动添加到上面去。
- 新增类的话要重启才能生效嗷！！！





### JSP中Java与页面数据交互：



#### if...else...的写法

- JSP将全部的执行块中的内容当成一个整体（执行语句块），变量之间可以相互调用。
- 正是由于是上面这个特性：

![image-20210421190752414](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210421190752414.png)

这个写法好骚啊orz.......

- 后面这个写法会被替代掉的。



#### for的写法

![image-20210421191152925](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210421191152925.png)

#### summary

Java是Java，Html是Html，两者串在一起写就好了嗷！！！



## JSP内置的常见Java对象

- 注意哈，JSP中这些对象是自带的，不用俺们专门去创建。

### request：

- 自动生成，你可以直接使用，用于读取请求参数对象

- `String userName = request.getParameter('userName')`
- `String password = request.getParameter('password')`

### session：

- 可以将共享数据添加到session中

- `session.setAttribute("key1",200);`
- `Integer value = (Integer)session.getAttribute("key1");`
- 不同的JSP之间可以通过同一个用户的Session实现通信。

### application:

- 同一个网站中的Servlet，JSP都可以通过当前网站的全局作用域对象及逆行数据共享
- `application.setAttribute("key1","hello world");`



## Servlet和JSP的联合调用

### Servlet

- 负责处理业务并得到处理结果（大厨）



### JSP

- 不负责处理业务，主要任务将Servlet中处理结果写入到响应体。（服务员）



### 相互调用

- 使用请求转发的方式，向Tomcat申请调用JSP
- 数据共享：
  - Servlet将结果添加到请求作用域对象
  - Servlet向Tomcat申请调用JSP文件
  - JSP运行时从请求作用域对象得到处理结果
- 实际使用例子：

![image-20210422093235737](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210422093235737.png)





![image-20210422093729405](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210422093729405.png)

- 调用Servlet，Servlet中自动调用JSP，JSP将处理结果存入响应体并返回响应体到浏览器。



### Maven中使用JSP

![image-20210423144430194](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210423144430194.png)



Maven中配置好对应的Tomcat后，Servlet的`./`的默认目录仍然是webapp目录嗷！！！就按照正常的方式来调用`JSP`文件就好。

















## JdbcUtil：

- 这个JDBC的一个连接类，很方便的
- 还可以借助这个类写出JdbcDao类，根据这个类可以写出很方便的工具类
- 这个工具类可以用于后期的调用，后期doGet方法中调用JdbcDao类是最方便的，但是需要两层包夹，所以要记得嗷！！！



## JSP运行原理：



1. 步骤：
   - Http服务器将JSP文件内容编辑成一个接口实现类
   - Http服务器将接口实现类编译为.class文件
   - Http服务器负责创建class的实例对象，这个实例对象就是Servlet实例对象
   - Http通过Servlet实例对象调用_jspService方法，将JSP文件内容写入到响应体中
2. 编辑与编译的位置：
   - C盘中Users中对应的user里面的Idea中system文件夹有个tomcat对应的网站空间，里面就储存对应的生成.java和.class文件。（本质上就是）
   - 标准文件：在work文件夹下看到过







## JSP-EL技术：

- 定义：
  - 首先是一个Jar包
  - 降低JSP文件开发时Java命令的开发强度
  - Tomcat服务器自身就携带了这个Jar包
- 对应的依赖，Tomcat在使用过程中会自动到自己的lib中找到并且使用对应的jar包。
- JSP文件作用：
  - 代替响应对象，将Servlet中doGet和doPost的执行结果写入响应体中。
- JSP开发主要步骤：
  - 从指定的作用域对象读取处理结果
  - 将得到的数据进行强制类型转换
  - 转换后的数据写入到响应体
- JSP本质也是一个Servlet呀，Servlet与JSP之间的数据通信和共享使用任意一种共享数据方式都可以的嗷！！！



### 命令格式：

- 获得全局对象

```jsp
${applicationScope.sid}
${sessionScope.sname}
${requestScope.home}
```

- JSP中非常方便取出对应的数据，写入到对应的位置上。
- `${作用域对象别名.共享数据}`
- 命令作用：
  - 表达式命令格式
  - 专门在JSP文件上使用
  - 取得全局变量中的值并输出到响应体中





### EL中作用域对象别名：

- 可以使用的作用域对象：
  - ServletContext application：全局作用域对象
  - HttpSession session：会话作用域对象
  - HttpServletRequest request：请求作用域对象
  - 只能在JSP文件中出现的对象：PageContext pageContext：当前页作用域对象，是JSP文件独有的作用域对象。在当前域对象存放的共享数据仅能在当前JSP文件中使用，是不能共享给其他的Servlet或其他JSP文件。真实开发中，主要用于JSTL标签与JSP文件之间的数据共享。`JSTL ------> pageContext`。

- 别名：
  - application ----> applicationScope.共享数据名
  - session ----> sessionScope.共享数据名
  - request ----> requestScope.共享数据名
  - pageContext ----> pageScope.共享数据名

![image-20210424143758658](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210424143758658.png)



### 应用对象属性咋办捏？？？

![image-20210425171644527](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210425171644527.png)



- 属性名的大小写完全一致，这里直接取到对象把里面的属性拿出来即可。
- 引用对象属性写入响应体中：
  - 格式：`${作用域别名.共享数据名.属性名}`
- 作用：从作用域对象指定共享数据关联的引用对象的属性值，并自动将属性的结果写入响应体中。
- 属性名和类的属性名必须大小写完全一致，内部运行的机制是反射机制。



**缺陷：没有提供遍历集合的方法，因此无法从作用域对象中读取集合内容。**





### EL表达式简化版：

- 命令格式：
  - `${共享数据名}`
- 











### 反射机制：

- 映射一个类文件有什么用：
  - 读取这个类的信息和相关方法
  - 动态实例对象的作用：



## EL表达式简化版

- 命令格式：`${共享数据名}`
- 命令作用：EL表达式允许开发时省略作用域别名。
- 可能会有问题：
  - 不同作用域对象中相同名字的属性就很惨
- 这个时候，就会“猜”，猜是哪个共享变量。
- 多线程特征：
  - 原子性
  - 可见性
  - 有序性
- 工作原理：
  - 先去PageContext里面找
  - 找不到就去request找
  - 找不到就去session找
  - 找不到就去application找
  - 找不到就返回null
- 隐患：
  - 容易降低程序执行速度
  - 容易导致数据定位错误
- 应用场景：
  - 简化从PageContext共享数据并输出的难度。
- 尽管存在很多问题，但是在开发过程中，为了节省时间，一般都是用简化版。



## 支持运算表达式

- 前提：在JSP文件中有时需要将读取的结果写入响应体中
- 例子：

![image-20210426172605223](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210426172605223.png)



- 支持运算符：
  - 数学运算
  - 关系运算:
    - ![image-20210426172710489](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210426172710489.png)
    - ![](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210426172815694.png)
  - 逻辑运算：![image-20210426172947383](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210426172947383.png)
- JSP只做输出，一般不做复杂运算，复杂的你给Servlet做哇







## EL中的内置对象



1. 

- 格式：`${param.请求参数名}`
- 作用：从请求对象中读取当前请求包中的请求参数内容，并将请求参数写入响应体
- ![image-20210426173209674](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210426173209674.png)
- ![image-20210426173316970](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210426173316970.png)



2. 

- 格式：`$paramValues.请求参数名（下标）`
- 作用：如果浏览器发送的请求参数是一个请求参数关联多个值，使用paramValues读取请求参数下指定位置的值写入响应体
- ![image-20210426173531749](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210426173531749.png)
- 和前端对上了！！！一个key对应的是一个数组，这个就是取到这个数组并且读取数组里面的值
- ![image-20210426173648768](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210426173648768.png)











## EL表达式常见异常

- 属性名写错了：

- ![image-20210426185919190](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210426185919190.png)
- ![image-20210426185944857](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210426185944857.png)

- 常见异常：`javax.el.PropertyNotFoundException`













## 随机查询业务

![image-20210426190700980](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210426190700980.png)

- 如果sql里面只有八个字段，0.9 -> 9 该怎么办？？？其实已经设置好了，重新从第一个开始。
- 一组标签一次只能有一个radio被选中，如果radio标签的name属性相同，就认为是同一组QAQ。可以根据id组装name嘛，对不对？





## 在线阅卷业务

- 本质上是为同一个用户服务的两个Servlet之间的数据共享。
- cookie有可能暴露出答案来，用session比较稳健。
- 只有一个地方能够使用`getSession()`，就是登陆页面，其他页面都要用false的，来防止用户恶意登陆。
- 从数据库中随机查出来的题目存入到Session中，然后用户提交的过程中，把用户提交的答案和你Session中的标准答案做对比然后打分。



























## 一些小技巧：

- 为了使得遍历更加方便，通常`List`加入泛型，例如：`List<Student>`，就方便用`for(Student stu:stuList)`这样来遍历了。
- disabled和readOnly属性实际上用户都不能动，但是有个区别就是有disabled的选项是不能够在表单中被提交的。但是readOnly选项对应的数据用户不能够修改但是可以随着表单提交。（这里类似于题目的编号，QuestionID是不饿能够修改，但是要跟着一起提交的。）
- Map中拿出来的对象都是Object，如果需要使用的话可能还需要强制类型转换。

![image-20210424142040948](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210424142040948.png)



- `Alt + Insert `可以插入补充例如构造方法等。或者右键generate也可以快速生成嗷！！！
- 









## 常考面试题：

1. JSP运行原理

- JSP运行原理：

  - Http服务器将JSP文件内容编辑成一个接口实现类

  - Http服务器将接口实现类编译为.class文件
  - Http服务器负责创建class的实例对象，这个实例对象就是Servlet实例对象
  - Http通过Servlet实例对象调用_jspService方法，将JSP文件内容写入到响应体中

  

- 编辑与编译的位置：

  - C盘中Users中对应的user里面的Idea中system文件夹有个tomcat对应的网站空间，里面就储存对应的生成.java和.class文件。（本质上就是）
  - 标准文件：在work文件夹下看到过





2. IDEA中项目是如何迁移的，以及IDEA如何使用连接数据库的功能。[b站教程](https://www.bilibili.com/video/BV1Yz411B7Pk?p=140&spm_id_from=pageDriver)。





3. 新建新的工作空间：[新的工作空间的建立](https://www.bilibili.com/video/BV1Yz411B7Pk?p=144&spm_id_from=pageDriver)。一个新的工作空间就像一个数据库，里面有很多的文件夹。





4. 面试题目集合[b站JavaWeb面试题目集合](https://www.bilibili.com/video/BV1Yz411B7Pk?p=155)



