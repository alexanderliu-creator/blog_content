---
title: Web服务器之HTTP协议与Tomcat服务器
date: 2021-03-29 16:55:53
tags: 后端
categories: 动力结点后端课程
---



# Tomcat学习，刚刚好和计网课程学的东西对应上了，学而致用哈哈哈哈刚刚好！！！



<!--more-->



# Http网络协议包

1. 二进制得到对应的数据，HTTP服务器很难从二进制数据得到相关的信息
2. 网络协议包有一组有规律的二进制数据，这组数据存在固定空间，存放特定信息。接收方就可以从对应的空间中得到对应的信息。（就是我们讲的HTTP协议嘛，很快的。）
3. HTTP网路协议包：
   1. 基于B/S结构下互联网通信过程中，传递的信息都是保存在HTTP网络协议包中
   2. HTTP协议包分类：
      - request
      - response



## HTTP请求协议包内部空间

- 按照自上而下区分：
  - 请求行：
    - url（请求地址）
    - method（请求方式）
  - 请求头（头部）：
    - 请求参数信息（如果是get，参数将会保存在这里）
  - 空白行（没有任何内容，起到隔离作用）
  - 请求体：
    - 请求参数信息（如果是post，参数将会保存在这里）

![image-20210329172014609](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210329172014609.png)

- 请求参数可以在输入栏以`?`的方式来添加



## HTTP响应协议包内部结构

- 按照自上而下区分：
  - 状态行：
    - HTTP状态码
  - 相应头（头部）:
    - content-type（告诉浏览器以何种方式来处理二进制的文件，类似于html页面啊，png照片啊之类的）
  - 空白行（没有内容，起到隔离作用）
  - 响应体：
    - 访问的静态文件的内容
    - 访问的静态文件的命令
    - 访问的动态资源文件运行结果



![image-20210329172819845](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210329172819845.png)





# Tomcat服务器

- 安装在服务端的软件，响应客户端的请求
- 服务器分类：
  - `JBOSS`服务器
  - `Glassfish`服务器
  - `Jetty`服务器
  - `Webphere`服务器
  - `Tomcat`服务器（可以用于模拟互联网通信）

## 服务器的下载：

- [官网](https://tomcat.apache.org/)
- 解压之后就算安装成功了。
- 只要`java_home`配置好就好了，其他不用配置的嗷。



## 服务器启动与关闭：

- startup，进入tomcat的bin目录下输入cmd进入控制台输入startup.bat即可启动。
- 关闭弹出来的小窗口，在cmd下输入shutdown.bat即可关闭，出现异常就对了？？？就是酱的。





# 一些基本概念：

## 重载与重写:

- 重载的目的是让方法在接收不同参数实现不同的功能。

- 重写：
  - 发生在继承的过程中
  - 子类对于父类实现细节进行重新定义
  - 规则：
    - 重写方法时，子类不能降低方法的访问权限
    - 可以扩大访问权限
    - 由private和final修饰的方法是不能够被重写的
  - 重写方法时，抛出的异常可以是父类抛出异常的全集，子集，空集。不能抛出父类方法里，没有声明的异常。（因为父类实例可以指向子类对象，抛出来父类不认识处理不了就会显得父类很呆）。
  - 重写方法返回值，返回值可以是返回类型的子类型。（父类可以返回子类，子类不可以返回父类）

## throw与throws:

- throws在方法名之后，通知开发人员可能抛出异常
- 一个throws后面可以携带多个异常。
- throw执行时抛出一个指定异常对象
- 一个throw一次只能携带一个异常对象
- throws必须处理，但是throw不一定要处理嗷！！！



## 接口与抽象类：

- 接口是一种特殊文件
- 作用：
  - 指定规则
  - 降低耦合度
- 使用规则：
  - 接口中属性默认都是静态常量属性
  - 接口中方法都是抽象的
  - 方法访问权限不能是private
  - 接口见可以多继承，但是不能相互实现
- 抽象类：
  - 作用：
    - 降低接口实现类与接口之间实现难度
  - 抽象类的作用是降低了接口实现类和接口之间的实现难度（抽象类就是一个过渡！！！）
  - 抽象类可以声明抽象方法，也可以生成具体方法，抽象类的抽象方法必须由子类重写
  - 抽象类有构造方法，但是不能使用



# Tomcat内部结构：

- bin文件夹：
  - 关闭
  - 开启
- conf文件夹：
  - 核心配置文件
  - server.xml是核心文件，里面包括对于端口啊，高并发啊之类的属性的调整。
- lib文件夹：
  - 中存放了tomcat需要用的jar包依赖
- logs文件夹：
  - 记录了相关的日志信息
- temp文件夹：
  - 存放一些临时文件
  - tomcat会自动销毁其中的文件
- webapps文件夹：
  - 找到相关的文件返回给客户端
  - 如果没有的话就告诉客户端没有
- work文件夹：
  - 工作空间



# 模拟一次互联网通信

- 在Tomcat的/webapps文件夹创建一个网站（myWeb)
- 网站不过是一个文件夹罢了。
- `localhost:8080/myWeb/资源名`这样即可向服务器请求对应的文件。
- URL格式：`网络协议包：//服务端计算机IP地址:Http端口号/网站名/资源名字`
- 点击回车之后浏览器会发送请求，服务器收到请求之后会找到相应资源发送回浏览器。



# IDEA管理Tomcat

## 添加Tomcat服务器：

- 步骤：
  - 告诉idea，去管理哪个服务器
  - 建立一个开关：可以用开关来开启或者关闭Tomcat
- IDEA配置Tomcat步骤：

![image-20210331220512107](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210331220512107.png)



![image-20210331220617507](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210331220617507.png)



## 添加Tomcat的开关：

![image-20210401172159186](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210401172159186.png)

![image-20210401172335345](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210401172335345.png)

- Remote：远程开关
- Local：本机开关
- 选择local对于Tomcat进行配置就好了。

## 开关使用：

![image-20210401172637527](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210401172637527.png)

- 三角形代表网站不能被修改
- Debug可以实时发生修改。



# IDEA建立网站：



![image-20210401191301071](C:/Users/Alexander Liu/AppData/Roaming/Typora/typora-user-images/image-20210401191301071.png)



![image-20210401191332613](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210401191332613.png)



接下来就可以建立网站：

- 网站名字可以为中文
- 交给Tomcat管理的时候再起个英文名就行

## Java工程和Module的区别：

- Java工程下只有一个src
- 网站下则有两个文件夹，这才对：
  - 一个放静态资源文件
  - 一个放动态资源文件

![image-20210401191904644](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210401191904644.png)

- lib没有的话自己建嘛，jar包要在工程结构里面找到对应的Modules然后导入对应的Jar包

## 注意！新的IDEA中发生了很大的变动

[IDEA改动后如何创建Web项目](https://www.zhihu.com/question/344062503)

- 要向上面的lib包中添加依赖。

![image-20210402084108642](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210402084108642.png)

- 这里用了Tomcat，如果不用Maven要手动导入这些依赖库的

- 类似于你需要mysql驱动也可以在这里添加的

![image-20210402091308338](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210402091308338.png)

- 静态文件放在web下面



![image-20210402091401928](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210402091401928.png)

- 在刚刚的按钮界面，找到deployment，点击添加把刚刚写好的网站文件导入，注意下面有个`Application context`，这个是部署的目录的名字，自己取一个英文的就好了，然后点击按钮打开即可看到部署的页面。
- 注意一下，这里导入的Tomcat和之前手动打开不一样嗷，貌似只有这里部署的你写的Web才能在这里跑起来，在上面讲的`Tomcat`中的`webapps`底下写的web页面貌似消失了？？？