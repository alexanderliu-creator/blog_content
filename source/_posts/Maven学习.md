---
title: Maven学习
date: 2021-03-01 21:00:08
tags: 后端
categories: 动力结点后端课程
---



# 这里是包管理器Maven的学习，后端使用Java必不可少的一部分哦！！！



<!--more-->





# 初识Maven

Maven是很棒的java项目构建工具

[找到的很棒的网课资源](https://www.bilibili.com/video/BV1dp4y1Q7Hf?p=7)

这篇markdown主要是听讲过程中的一些摘要。采用的开发环境是java jdk是1.8版本，maven的版本是3.3.9版本。除此之外，完成了电脑上多个jdk版本的下载与配置，我的电脑上采用的jdk版本有jdk 15.0.2 和 jdk 1.8（也就是java8）。前面一个是最新的jdk版本，后面一个是稳定版本。

有关链接如下：

[多版本Java文件的环境配置](https://www.cnblogs.com/ooo888ooo/p/12588631.html)

[JDK环境搭建](https://zhuanlan.zhihu.com/p/80371586)

## Maven功能：

### Maven的好处

> 1. 管理jar文件
> 2. 可以自动下载jar文件和文档，源代码
> 3. 管理jar的直接的依赖
> 4. 管理你需要的jar版本
> 5. 帮你编译程序，把java编译为class
> 6. 帮你测试代码是否正确
> 7. 帮你打包文件，形成jar文件或者war文件
> 8. 帮你部署项目

### 构建功能

> 1. 清理：把之前项目的编译的东西删除掉，更新编译代码
> 2. 编译：把程序源代码编译为执行代码
> 3. 测试：同时执行多个测试代码
> 4. 报告：生成测试结果的文件
> 5. 打包：把项目中所有的class文件，位置文件等所有资源放在一个压缩文件中。
> 6. 安装：把5中生成的问价jar , war安装到本机仓库
> 7. 部署：把程序安装好可以执行





## Maven核心概念（POM）：

$Project\ Object\ Model$，项目对象模型

基本信息：

| modelVersion | Maven模型的版本，对于Maven2和Maven3来说，它只能是4.0.0       |
| ------------ | ------------------------------------------------------------ |
| groupId      | 组织id , 一般是公司域名的倒写，可以为：com.baidu             |
| artifactId   | 项目名称，也是模块名称，对应groupId中的子项目                |
| version      | 项目的版本号，自定义的。例如为：1.1.0，-SNAPSHOT为开发中的不稳定版本。 |

上面倒数三个为坐标，一定是唯一的，互联网唯一标识一个项目。坐标例子：

![image-20210227170325208](https://gitee.com/alexs-rabbit//blog/raw/master/20210227170332.png)

| packaging                | 项目打包的类型，可以是jar , war , rar , ear等       |
| ------------------------ | --------------------------------------------------- |
| dependencies和dependency | 项目中要使用的各种资源的说明                        |
| properties               | 一些配置的属性                                      |
| build                    | 相应的一些配置信息，例如指定java代码使用的jdk版本等 |



dependencies和dependency的例子：

![image-20210227185929880](https://gitee.com/alexs-rabbit//blog/raw/master/20210227185929.png)

相当于java中的import某个包，一样的先去本地查询，没有的话再去远程仓库查询。



properties的例子：

![image-20210227190122787](https://gitee.com/alexs-rabbit//blog/raw/master/20210227190122.png)







本质为一个文件pom.xml -- 项目对象模型，把一个项目当作一个模型来使用。**控制maven构建项目的过程**

### 约定的目录结构

maven项目的目录和文件的位置都是规定的

### 坐标

是唯一一个字符串，用来表示资源的。

具体坐标内容，上面的POM部分有做详细解释了。可以用这个坐标来定位下载驱动。

[可以搜索驱动和依赖的网站](www.mvnrepository.com)，可以在上面根据需求去获取对应的驱动。可以直接选择对应的发行版本，通过pom或者是jar的方式来获取对应的依赖文件。

### 依赖管理

管理你的项目可以使用的jar文件

### 仓库管理（了解）

存放资源的位置

### 生命周期（了解）

maven构建项目的过程

### 插件和目标（了解）

执行maven构建的时候用的工具是插件

### 继承

### 聚合

## Maven的安装和配置：

- bin：执行程序，主要是mvn.cmd
- conf：maven工具本身的配置文件，主要是settings.xml
- 配置环境变量，在系统的环境变量中，指定maven的安装目录。



## Maven核心概念：

> 1. 约定的目录结构，大家都遵循的一个规则
> 2. 每一个maven项目在磁盘中都是文件夹（项目）
>
> 工程目录/
>
> ​	/src
>
> ​		/main   #放置主程序的java代码和配置文件
>
> ​			/java	#程序的包和包里面的java文件
>
> ​			/resources	#java程序中要使用的配置文件
>
> ​		/test	 #防止测试程序代码和文件的（可以没有的）
>
> ​			/java	#测试的包和包里面的java文件
>
> ​			/resources	#java程序中测试使用的配置文件
>
> ​	/pom.xml(maven项目必有的核心文件)

## 构建Maven项目的过程：

1. 在你的项目下面（有/src和/pom.xml的目录下面启动cmd），然后执行`mvn compile`。

2. 下载很多的包，从中央仓库下载的，这些插件才能帮助maven正常的工作

3. 下载的都是jar文件

4. 下载的东西应该存放在: (默认本机仓库)

`C:\Users\Alexander Liu\.m2\repository`

5. 看到build success才下载完成
6. 最后会在文件夹中生成target文件，将所有文件编译为class文件并且储存在其中的classes文件中。

## Maven下载内容所在位置的更改：

1. maven的配置文件， maven安装目录：/ conf / setting.xml。
2. 修改localRepository

## 仓库：

1. 仓库是储存东西的，存放maven使用的jar和我们项目使用的jar

> maven使用的插件（各种jar）
>
> 我们项目使用的jar（第三方工具）

2. 分类：

> 本地仓库：本机上
>
> 远程仓库：
>
> > 1. 中央仓库（最权威的）
> > 2. 中央仓库的镜像
> > 3. 私服：在公司的内部，在局域网中使用的。

3. 仓库的使用：

> - 不需要认为参与，maven自己决定
>
> - 开发人员需要mysql驱动 ----> maven查找本地仓库 ----> maven查找私服 ----> maven查找镜像 ----> maven查找中央仓库







## Maven的常用命令，插件，寿命周期：

1. mvn clean

清楚上一次编译留下来的文件，也就是删除target文件夹

2. mvn compile（编译主程序）

编译main/java目录下的java为class文件，同时把class拷贝到target/classes目录下面，把main/resources目录下的所有文件都拷贝到target/classes目录下。

3. mvn test-compile（编译测试程序）

和上面一样，不过编译的文件不一样哦，是test/java，保存的位置位于target/test-classes目录下。

4. mvn test

执行生成的用于测试的class。它会把上面的三个步骤都执行一次。

5. mvn package

打包项目生成jar文件，只打包\src\main中的内容

6. mvn install

将上面的内容从上往下都执行一遍，然后把生成的jar包放置在你的本地仓库中。用坐标层次表示文件夹和文件位置，最终完成文件位置的确定。（通过加上对应的dependency其实就可以从仓库中取出对应的jar包。就可以用其中的方法了！！！）

7. mvn deploy

maven的生命周期，就是上面的七个步骤从头执行到尾。

这里有个很有意思的特性喔，执行下面的阶段时，其以上的阶段全部都会执行哦！！！

maven执行时，真正完成功能的是jar文件，本质为一些class。



> 1. 单元测试（测试方法）： junit , 是一个专门测试的框架，测试的是类中的方法，每一个方法都是独立测试的。方法是测试的基本单位。
>
>    maven借助单元测试，批量测试类中的大量方法。
>
> 2. 使用步骤:
>
>    1. 在pom.xml中加入junit单元测试相关的依赖，根据上面的方法去网站上进行查询。
>
>    2. 在maven项目中的src/test/java目录下去创建测试程序。推荐的创建类和方法的提示：
>
>       1. 测试类的名称： Test+测试的类名
>       2. 测试的方法名称： Test+测试的方法名称
>
>       ```java
>       @Test
>       public void testAdd(){
>       	// 测试HelloMaven的add方法是否正确
>       }
>       ```
>
>       测试方法的定义规则：
>
>       1. 方法必须是public
>       2. 方法必须没有返回值
>       3. 方法名称是自定义，推荐命名如上
>       4. 在方法的上面加入`@Test`





## 插件的配置：

有一个典型的例子：

![image-20210228233704009](https://gitee.com/alexs-rabbit//blog/raw/master/20210228233711.png)

配置例如jdk的版本信息等等

从上到下分别为：

- 插件的组织id
- 插件的名字
- 插件的版本
- 插件的信息：
  - 告诉maven我们写的代码在jdk1.8上编译
  - 我们的程序应该在jdk1.8上运行

插件不用记的，要用的时候查一下就好！！！

















# Idea配置并且使用Maven

### Idea中Maven的配置：

- 安装：不用系统内置的，用自己安装的，因为自己安装的方便配置。要用自己装的maven覆盖默认的maven。
- 所需配置项：
  - ![image-20210301084943639](https://gitee.com/alexs-rabbit//blog/raw/master/20210301084950.png)
  - ![image-20210301085017791](https://gitee.com/alexs-rabbit//blog/raw/master/20210301085017.png)
  - 上面配置当前项目的设置，下面配置以后新建项目的配置。
- ![image-20210301085341482](https://gitee.com/alexs-rabbit//blog/raw/master/20210301085341.png)
  - Maven Home directory: Maven的安装目录
  - User Settings File: Maven安装目录conf/setting.xml配置文件
  - Local Repository: 本地仓库位置
- ![image-20210301085854622](https://gitee.com/alexs-rabbit//blog/raw/master/20210301085854.png)
  - 调整默认配置，使得项目创建速度变快，而不用联网下载7M的那个文件。(VM Options)，这儿错了，那个值应该时                       -DarchetypeCatalog=internal , 而且设置完了要点apply。
- 注意，对于最上面两个都要设置一次哦，以后的新项目都可以使用这个maven哦。以后的项目都是基于maven创建的。





### 使用模板创建java项目：

1. maven-archetype-quickstart: 普通的java项目，勾选左上角的勾勾，然后选择这一项，来创建java项目。

2. ![image-20210301092135194](https://gitee.com/alexs-rabbit//blog/raw/master/20210301092135.png)

3. 完整的目录结构为：

   ![image-20210301092542466](https://gitee.com/alexs-rabbit//blog/raw/master/20210301092542.png)



### 使用模板创建web工程：

1. maven-archetype-webapp: 普通的web项目，勾选左上角的勾勾，然后选择这一项，来创建java项目。
2. 其他的和上面差不多嗷





### 其他的一些功能：

![image-20210301095601773](https://gitee.com/alexs-rabbit//blog/raw/master/20210301095601.png)

- 这里右边是可以直接双击去执行一些功能的哦！！！
- 下面的可以显示已经下载的插件和插件之间的依赖关系

![image-20210301095903603](https://gitee.com/alexs-rabbit//blog/raw/master/20210301095903.png)

- 这些写完之后可以直接引入servlet使用



### Idea导入Maven工程

- 如果重新写了配置，然后没有反应甚至报错的话：

![image-20210301201716375](https://gitee.com/alexs-rabbit//blog/raw/master/20210301201716.png)

- 右边选中相应的pom就可以在文件夹下导入对应的maven项目：

![image-20210301201912965](https://gitee.com/alexs-rabbit//blog/raw/master/20210301201913.png)

- 右边那一栏maven很多功能很有意思自己康康哦
- 导入maven项目：

![image-20210301202326550](https://gitee.com/alexs-rabbit//blog/raw/master/20210301202326.png)

调整完之后点击apply就导入了

- 或者用上上面那个图里面的加就可以导入maven项目了。





# 依赖管理

1. 依赖范围：用scope表示的：

   - 有：compile , test , provided ， 默认为compile。compile是一直到编译，provided就范围稍微小一点，编译测试用得到，打包安装用不到。compile范围中插件对应的jar会在packing的过程中保存在lib下面，这就是compile中 “自带” 的含义。provided的意思则是服务器自带，我打包部署的文件就不用我带了，反正你服务器能带我跑！！！
   - scope: 表示范围，就是在构建项目的某些阶段有效，例如

   ```xml
   <dependency>
   	<group>junit</group>
       ...
   	<scope>test<scope>
   </dependency>
   ```

   ![image-20210301203032255](https://gitee.com/alexs-rabbit//blog/raw/master/20210301203032.png)

2. 坐标都是从官网取的



# Maven常用操作

## maven的属性设置

- ![image-20210301204229504](https://gitee.com/alexs-rabbit//blog/raw/master/20210301204229.png)
  用于指定版本信息


## maven的全局变量

- 自定义属性：

  - 在<properties>通过自定义标签声明变量
  - 在pom.xml文件中的其他位置，使用${标签名}使用变量的值

  自定义全局变量一般是定义依赖的版本号，当使用多个相同版本号时，常常先定义版本号为全局变量。和C语言里面学的一样的，当有多个项目引入时，方便改版本号，很方便。

### 资源插件：

![image-20210301204753194](https://gitee.com/alexs-rabbit//blog/raw/master/20210301204753.png)

- 默认没有使用resources的时候，编译代码的时候，会把resources中的文件拷贝到class目录中。而对于/main/java中，不是Java的文件，则不做处理。
- 如果我们需要用到不是java的文件，则这些文件进不去class中哦，这个时候我们就要在build中加入resource中加入resources来告诉它我们需要操作这什么类型的文件。（其中java会自动编译哦！！！）



