---
title: Maven进阶学习
date: 2021-05-28 10:14:28
tags: 后端
categories: 动力结点后端课程
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721211728.png
---



# Dubbo那个讲的有些拉跨，先往下走嗷！！！



<!--more-->

- 主要解决的是多项目模块下，Maven如何通过父子工程的关系对于整个项目进行版本管理嗷！！

# 第一种方式：

## 场景描述：

- 多模块应用
- 可能使用到第三方的模块：Spring , MyBatis等

![image-20210528101947913](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528101955.png)

- Maven这个时候，非常适合管理嗷！！！（分布式就要实现这种模式）
- 大的项目需要一个老大来统一管理，统一版本号啊之类的。



## 创建父工程：

- 先创建一个父工程，不管quickstart也好，web也好，总之要一个Maven工程（实际上就是在新建空项目中新建一个Module为Maven项目）
- Maven父工程的要求：
  - packing标签的文本内容必须设置为pom：
    - packaging标签一般没有，没有的话默认为jar
    - 但是现在你是父工程，你一定要设置为pom
  - src要删除掉嗷！！！：
    - src , target , pom
    - target存打包后的文件的，可有可无，反正都要生成的。
    - src必须要删除掉，这就是为啥webapp和java工程都无所谓的原因了嗷！！！
- pom是项目对象模型，该文件是可以被集成的嗷！！！该文件可以被子模块儿来集成，本质上就是让子模块的pom文件来继承父工程的pom , 统一天下的pom！！！



## 创建子工程：

- 新建子工程必须也是Maven工程，quickstart , webapps都可以嗷！！！

![image-20210528105728878](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528105729.png)

- 选择的时候就要选择parent了嗷，记得改目录之类的嗷！！！

![](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528110156.png)

- 可以看看子模块中的pom文件这样就发生了改变嗷，由于其继承父模块，因此其这里的groupId和version都不用写嗷！！！
- Maven中的继承的含义和Java中差不多的嗷！！！
- 多个子Module去继承一个父亲Module就好了嗷！！！
- 继承可以反复继承，但是要注意，只要你要成为父工程，你一定要符合上面父工程的两点要求嗷！！！
- 一般应用的时候父工程子模块就行，主要统一标准两层就够用了，你搁那反复继承蹦迪没得意义嗷！！！



## 添加父亲：

- 创建工程的时候，哦豁，我忘了添加父子关系了orz

- 手动修改Pom文件嗷！！！
- ![image-20210528112023163](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528112023.png)
- 手动改配置关系，把配置文件改成继承的样子嗷！！！



## 这种方式的一些特性：

#### 依赖冗余：

- 添加统一的依赖到父工程中，所有的子模块就都有对应的依赖了嗷！！！
- 父工程通过这种方式帮助我们统一管理了依赖。
- 父工程添加的依赖，所有的子模块会无条件的继承。
- 这个时候可能造成**子工程中依赖冗余**，因为无条件继承了啊orz。怎么解决？
- 父工程加强管理嗷！！！把所有的依赖放到dependencyManagement标签中，进行依赖管理。然后你发现和之前相比，子模块中的依赖都没有了？？？

![image-20210528112728042](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528112728.png)

![image-20210528112801775](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528112801.png)

- 子模块不能无条件继承这些模块儿啦！！！子模块从夫模块中各取所需。

![image-20210528113041254](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528113041.png)

- 声明式依赖，这个时候不用写version咯，父模块给你规定了version， 你只需要你要啥就可以辽！！！子模块依赖父模块的版本号，这意味着，只要你改了父模块依赖的版本号，子模块版本号就会自动跟着改变嗷！！！
- 当然，如果子模块指定依赖的版本号，那么就不会继承父工程的版本号嗷！！！



#### 版本号管理：

- 本质上，父工程管理的是版本号嗷！！！
- 那就把版本号专门拿出来管理就好了嘛~

![image-20210528143704153](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528143704.png)

- 这里的标签是自定义的名称嗷，推荐使用项目名称-version这种形式来命名标签。也可以使用项目名称.version来命名嗷！！！当然也可以简化啊，你定义为mysql-version大家也都明白嗷！！！
- 这里定义完成之后，后面的dependencyManagement就可以修改其中的内容了嗷！！！

![image-20210528143836778](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528143836.png)

- 版本号分离了，这样更加方便管理嗷！！！
- SpringBoot也是用这种方式来管理的嗷！！！



# 第二种方式：

## 创建父工程

- 父工程的创建方式和前面是一样样的嗷！！！
- 父工程要遵循的规矩都是一样样的嗷！！！

## 创建子工程

- 子工程的创建：

![image-20210528144602968](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528144603.png)

- 这里是将子工程添加到父工程的里面了嗷！！！

![image-20210528144702290](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528144702.png)

- 然后兔兔子发现，我吐了这玩意儿我自己玩IDEA的时候遇见过，在新版的IDEA中没这个选项，第二种方式的这个操作是默认的orz。通过创建项目时候的通过**文件所在目录**来指定的，问题很大！！！如果你在新版的IDEA中不改，默认就是放在你的父Module的目录下面的嗷！！！
- 有一些小区别把，子模块的pom里面没有relative-path了。
- 父工程在新的IDEA版本中，总会有：

![image-20210528145103323](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528145103.png)

- 代表着它有哪些孩子工程嗷！！！

- 孩子要当爹了咋办？一样样的嗷，满足要求就可以有宝宝！！！
- 说白了第二种方式已经被新版的IDEA给取代了，确实没必要有这个选项嗷orz。



## 编译插件添加

- 建议还是添加以下编译的插件嗷，可能会出问题的地方我们尽量避免嗷！！！

![image-20210528150317806](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528150317.png)

- 你看看兔兔自己的默认的还是1.5呢orz

- 插件一样只在父工程里面添加就行了嗷：

![image-20210528150658407](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210528150658.png)

- 添加完自动就变啦！！！

```xml
  <build>
    <plugins>
      <!--jdk1.8编译插件-->
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>
  </build>
```

- 父工程不仅可以管理依赖，还可以管理插件嗷！！！

- 工作中这两种模式都有用到哦！！！第一种比第二种要多，第一种更加直观嗷。



# 第三种模式

- 一二种混杂使用，既有同级的，又有在包里的orz。
- 一般是你的大爷们（父工程）在第一层，你的孙子们（子工程）在父工程里面，这种嵌套方式嗷！！！































## 这种方式的一些特性：

- 参见上面嗷，一样样的，依赖管理啊，依赖声明啊，版本号管理啊，都和上面**一样**。





































## 小tips：

- 由于这次示例的时候，我尝试的次数有点多，对于工程结构有了更加深的理解。

- 如果你要删某个模块儿：
  - 先从项目工程中把它删了
  - 再去文件夹下把它删了
  - 再IDEA内点那个锤子重新构造工程
  - 最后再将其重新创建即可



- 如果你要修改某个模块儿：
  - 先把iml文件给他删了
  - 先从项目工程中把它删了（有可能删了iml之后项目就自动删除了）
  - 再IDEA内点那个锤子重新构造工程
  - 最后再将其重新导入项目中即可
- 注意以下，这样可能可以解决项目名字后面有个括号的问题。
- 修改了依赖之后，记得刷新以下全部的pom嗷！！！



- 爷爷可以找到所有的子子孙孙嗷！！！

