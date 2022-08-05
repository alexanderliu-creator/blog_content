---
title: Spring框架学习
date: 2021-05-01 11:34:55
tags: 后端
categories: 动力结点后端课程
---





# 这是SSM中Spring框架的学习，MyBatis记得复习嗷！！！



<!--more-->





# IoC学习



# 第一节课开始：

- Spring全家桶：
  - spring
  - spring mvc
  - spring boot
  - spring cloud
- Spring核心技术：
  - ioc - 控制反转
  - aop - 管理对象之间的关系
  - ioc , aop能够实现模块之间，类之间的解耦合
- 文档获取：
  - spring官网
  - 我们学的是Spring Framework

## 简介：

- 轻量：
  - 核心jar总共在3M左右，占有资源少，运行效率高，不依赖于其他的jar。
- 针对接口编程，解耦合：
  - ioc控制反转
- AOP编程的支持
- 方便集成各种优秀框架（例如：Spring集成Mybatis）



- 体系结构：

![image-20210504102910329](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210504102910329.png)



- 框架怎么学：
  - 框架能干啥
  - 框架的语法，需要一定的步骤支持
  - 框架的内部实现，原理是什么
  - 通过学习，可以实现一个框架











## IoC控制反转：

- Inversion of Control
- 描述的想法是：
  - 把对象的创建，赋值和管理交给**代码之外的容器**实现，也就是对象的创建是由其他外部资源来完成的。
  - 控制：
    - 创建对象，对象的属性赋值，对象之间的关系管理。
  - 正转：
    - 开发人员主动使用new构造方法创建对象，开发人员主动管理对象。
  - 反转：
    - 把原来开发人员管理，创建对象的权限转移给代码之外的容器来实现，由容器代替开发人员来管理对象，创建对象，给属性赋值。
  - 容器：
    - 一个服务器软件，一个框架
- 为什么使用ioc：目的是减少对于代码的改动，也能够实现不同的功能。（实现解耦合）





### Java中创建对象的方式：

- 构造方法
- 反射
- 序列化
- 克隆
- IoC：容器创建对象
- 动态代理



- IoC的体现：
  - Servlet就是一个体现，Servlet对象就是由服务器来创建的，不是我们手动创建的对象。所以Tomcat也被称为容器，里面存放的有Servlet对象，有Listener，Filter对象。

### IoC技术实现：

- di：dependecty injection，依赖注入，IoC实现的一种方式。
- 只需要在程序中要使用的对象名称就可以了，对象的操作是由容器内部来实现的。
- Spring也是使用的di技术，底层本质上使用的反射机制来创建对象，因此可以通过名称来获得对象。



### 对象创建：

- [Maven中创建](https://www.bilibili.com/video/BV1nz4y1d7uy?p=7&spm_id_from=pageDriver)

- 实现步骤：

  - 创建maven项目
  - 加入maven依赖
  - 创建类：
    - 接口和他的实现类
    - 和没有使用框架一样，就是普通的类
  - 创建spring需要使用的配置文件：
    - 申明类的信息，由spring创建管理
  - 测试spring创建的对象

- Maven中依赖的添加（天坑）：

  - [添加dependency的完整性](https://blog.csdn.net/u014320421/article/details/105676119/?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-1&spm=1001.2101.3001.4242)
  - Maven具体添加内容：

  ```xml
    <dependencies>
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.11</version>
        <scope>test</scope>
      </dependency>
  
      <!-- https://mvnrepository.com/artifact/org.springframework/spring -->
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>5.2.5.RELEASE</version>
      </dependency>
  
  
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>5.2.5.RELEASE</version>
      </dependency>
  
  
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.5.RELEASE</version>
      </dependency>
  
  
  
      <dependency>
      	<groupId>org.springframework</groupId>
      	<artifactId>spring-context-support</artifactId>
      	<version>5.2.5.RELEASE</version>
      </dependency>
  
  
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-expression</artifactId>
        <version>5.2.5.RELEASE</version>
      </dependency>
  
  
    </dependencies>
  ```

  

- 配置resources文件夹下的文件时：

  - 若没有Spring.xml这个选项：[解决方法](https://blog.csdn.net/wt_better/article/details/78162332)
  - 配置文件中的内容：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  
      <!--告诉spring创建对象
          声明bean：就是告诉spring要创建某个类的对象
          id：对象的自定义名称，唯一值，Spring通过这个名称找到对象
          class：类的全限定名称（不能是接口，因为spring是反射机制创建对象，必须使用类）
          Spring就会自动创建对象(使用对象的无参构造方法)并且放入到Map中，spring框架有一个map存放对象的
          一个bean标签声明一个对象。
      -->
      <bean id="someService" class="com.tuzi.service.impl.SomeServiceImpl"></bean>
  
  </beans>
  
  <!--
      spring的配置文件
      beans: 是根标签，spring把java对象称为bean
      spring-beans.xsd是约束文件
  -->
  ```

  - getBean()中参数是配置文件中你设置的id值域，通过这个值，可以获得对应的对象。
  - 测试实例：

  ```java
  public void test02(){
      String config = "beans.xml";
      //ApplicationContext表示Spring容器，可以通过这个容器获取对象
      //classpath表示从类路径中加载spring的配置文件，获得这个全局变量
      ApplicationContext temp = new ClassPathXmlApplicationContext(config);
      //执行上面这条语句的时候实际上就创建了对象的啦。
  	
      //获得全局变量中你要的那个对象（通过id获取）
      SomeService service = (SomeService) temp.getBean("someService");
  
      service.doSome();
  }
  ```

- 创建全局Map对象的时候，所有你声明在spring的配置文件中的对象实际上都配置好了嗷！！！

### 对象方法：

- getBeanDefinitionNames()：获取对象的名字数组
- getBeanDefinitionCount()：获取对象数目



### 创建非自定义类的对象：

- 创建存在的某个类的对象（不是自己写的类，已经存在的）
- 当然是可以创建对象的，只要别人写好的类，获得对应的类，就能获得对应的对象。
- 你可以导入系统中某些块儿，如Date类的绝对路径，Spring就能够创建这个Date类。
- 记住，Spring创建对象时候使用的默认是无参数的构造方法。





## Spring给Java对象属性赋值：



### Set方式赋值

- 实现方式：
  - 使用标签和属性完成，基于xml的di实现。
  - 使用Spring中的注解，完成属性赋值，叫做基于注解的id实现。
- di语法分类：
  - set注入：Spring调用set方法，80%都是使用这种。
  - 构造注入：Spring中调用类的参数构造方法，创建对象，在构造方法中完成赋值。
- 配置文件的默认名字：
  - `applicationContext`

- Spring中规定Java基本数据类型和String都是简单类型
- set注入：
  - Spring中调用类的set方法，可以在set方法中完成赋值
  - 具体实操：
    - ![image-20210504123419534](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210504123419534.png)
    - Spring就会自动根据这些信息去给对应的属性赋值。
    - ![image-20210504123727694](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210504123727694.png)
    - 注意这里的value必须都是用字符串来表示的。

- 命名规则中一个属性就应该对应一个set和一个get。如果你的类中没有Set方法，当前的赋值行为是不可以正常执行的。必须要有Set方法嗷。
- Spring只负责执行对应的Set赋值方法，具体方法里面是如何执行，甚至方法里面赋值与否，是由你来决定的嗷！！！
- Set过程：
  - Spring调用无参构造构造对象
  - Spring使用对应的set方法来实现属性的赋值
  - 就算没有这个属性，只要符合设值注入的规则，Spring都可以正常执行嗷，只看有没有对应的Set方法！！！本质上就是调用Set方法而已，不管有没有这个属性的。
- 不管什么属性，这里赋值的使用用的都要加上引号，这是xml文件的规则。
- 不管是不是自定义类，只要有Set方法，就可以调用Set方法嗷！！！



#### 引用类型Set赋值：

- ![image-20210504130433054](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210504130433054.png)
- ![image-20210504130540362](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210504130540362.png)
- value + ref



#### 构造注入方式：

- 有参构造，构造的时候注入嗷！！！
- `constructor-arg`标签，属性：
  - name：形参名
  - index：从左往右0,1,2的顺序
  - value：形参类型是简单类型的
  - ref：形参类型是应用类型

##### name注入：

- ![image-20210504211540492](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210504211540492.png)
- 这里的顺序很智能，上下的bean顺序没有必然的上下顺序嗷。



##### index注入：

![image-20210504212309083](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210504212309083.png)

- constructor的上下顺序也可以调整位置的嗷。
- 省略index，直接给value也可以，不过这个时候上下的顺序就不可以换咯！！！



##### File构造注入：

- 如果知道File的话，就可以构造注入了嗷。



##### 结论：

用name可读性更高，推荐用name嗷。







































### JUnit简介：

- 单元测试，单独测试不同的方法
- 首先需要加入JUnit的依赖
- 测试方法的定义：
  - 首先应该是public方法
  - 没有返回值，void
  - 方法名称自定义，建议名称是test+你要测试方法的名称
  - 方法没有参数
  - 方法上面加入@Test，这种方法是可以单独执行的，不用main方法











# 第一节课总结：



## 什么是Spring：

- 一个框架，核心技术是ioc和aop，实现解耦合。
- Spring是一个容器，其中存放的是Java对象，我们将对象放入到容器中。
- 核心思想：想用啥你就在配置文件中声明啥就好啦！！！
- 值是可以在配置文件指定关系时（如ref）动态替换的，降低了类之间的耦合度。



## 怎么使用Spring：

- 项目中用到的对象放入到容器中
- 让容器完成对象的创建，对象之间的管理
- 我们程序从容器中获取要使用的对象



## 什么样的对象放入容器：

- dao , service , controller , 工具类
- Spring中对象默认是单例的，不能有重名的对象



## 什么对象不放入容器：

- 实体类对象，实体类数据来自数据库。
- Servlet
- Listener
- Filter

- 上面这些是Tomcat中的，不要放入Spring中



## 把对象放入容器中：

- xml配置文件
- \<bean>标签
- 注解



## 使用Spring的步骤:

- 加依赖
- 创建类
- 创建Spring配置文件，使用bean声明对象
- 通过ApplicationContext接口和ClassPathXmlApplicationContext的方法getBean()







# 第二节课开始：

## 自动注入：

- 引用类型有效，针对某些规则对于引用类型赋值，不用你再给引用类型赋值。

- 规则：

  - byName:

    - 按照名称注入，java类中引用类型的属性名和spring容器中bean的id一样。且数据类型是一致的，就可以赋值。
    - 语法：

    `<bean id="xx" class="yyy" autowire="byName">`

  - byType:

    - Java类中引用类型的数据类型和spring中bean的class属性是同源关系，这样的bean可以赋值给引用类型。

    - 同源的类型：

      - java类中引用类型和bean的class是一样的。
      - java类中引用类型和bean的class是父子类关系。
      - java类中引用类型和bean的class是接口实现类关系。

- 以上多种都存在的时候，会抛出异常的，只有single matching的时候才能够正确赋值。



## 多配置文件的方式：

- 项目规模比较大的时候，使用多个配置文件
- 优势：
  - 每个文件的大小会比一个文件小很多
  - 避免多人竞争带来的冲突。
    - 比如你的项目有多个模块，一个模块一个配置文件。不同人就可以操作不同的配置文件。
    - 也可以按照类的功能划分，比如操作数据库的就单独一个配置文件，不同人也是操作不同类型的类的配置文件。

### 包含关系的配置关系：

- 有一个主配置文件，只包含其他文件，不包含bean对象，说白了就是用来处理关系的。

![image-20210505103021567](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210505103021567.png)

- 注意一下这里的classpath，指的是从编译完成的classes目录下找对应的class，所以要注意文件的目录结构。
- 先加载总的文件，总的文件根据路径，把所有相关的文件都加载进来成为一整个文件，和原来的没啥区别！！！
- 在包含的配置文件中，可以使用通配符(*：表示任意字符)：
  - ![image-20210505103330195](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210505103330195.png)
  - 主的配置文件名称不能够在通配符的范围内，不然就会产生“死循环”。
  - 使用通配符的文件必须要放在同一个目录中，不然就会失败。





## 注解的使用：

- 注解本身是一个类嗷！！！
- 基于注解的di：
  - 基于注解完成Java的床创建，属性赋值。
- 使用注解的步骤：
  - 加入maven的依赖：spring-context，在你加入spring-context的同时，间接加入spring-aop的依赖。使用注解必须加入aop依赖
  - 类中加入spring注解（多个不同功能的注解）
  - 在spring的配置文件中加入一个组件扫描器的标签，说明注解在你的项目中的位置。
- 学习的注解：
  - component
  - respotory
  - service
  - controller
  - value
  - autowired
  - resources

- 实现步骤：
  - 加入依赖
  - 创建类，加入注解
  - 创建spring的配置文件
  - 使用注解创建对象



### Component：

- value就是对象的名称，也就是bean的id。
- value的值是唯一的，创建的对象在整个Spring容器中就只有一个。
- 注解位置：在类的上面。
- 添加了注解之后，配置文件中还要添加component-scan标签，扫描遍历base-package所指定的包，把包中和子包中的所有类，找到类中的注解，按照注解创建对象或给属性赋值。
- 加入component-scan标签后，配置文件的变化：
  - 加入一个新的约束文件spring-context.xsd
- 默认调用的也是无参构造方法

![image-20210505110731933](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210505110731933.png)

- 其中第二种最常用，Spring中和@Component功能一致，创建对象的注解还有：

  - @Repository：用在持久层上面的注解
  - @Service：用在service的实现类上面，创建service对象，做业务处理的
  - @Controller：用在控制器上面，创建控制器对象的。

  以上三个注解的使用语法和Component一样的，但是他们各自还有额外功能。这三个注解是给我们的项目对象分层的。

- 什么时候使用Component呢？

  - 不是上面的三种就用Component就好了嘛

### 指定扫描多个包：

- comonent-scan使用多次去扫描
- 或者使用(;或者,)分隔多个包名
- 指定父包

![image-20210505113007512](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210505113007512.png)



### 简单类型赋值:

- 使用Value注解

- value是string类型的，表示简单类型的属性值
- 位置：
  - 在属性定义上面，无须set方法，推荐使用
  - 在set方法上面
- 可以省略Value中的value，这是项目中的常用方式，非常方便直观嘛。
- 一般都是放在属性之上，要注意哈，@Component之类的注解是不能少的，不然对象根本创建不出来好吧。
- 本质上Spring内部维护了一个Map，通过你给key的值返回对应的value（对象）给你嗷！！！





### 引用类型赋值：

- Autowired：Spring框架提供的注解，实现引用类型的赋值。支持byName和byType
- @Autowired默认使用的是byType自动注入。
- ![image-20210505114932252](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210505114932252.png)
- 默认搜索容器中的School对象并且赋值给School对象。
- 不管你用注解也好，bean标签也好，都本质上是去创建对象！！！注解是和配置文件等价的嗷！！！
- 如果使用的是byName的方式，需要做的是：
  - 在属性上面加入@Autowired
  - 在属性上面夹入@Qualifier(value="bean的id")
  - 注意是上面两个要同时加入到属性上面嗷！！！最好按照上面这个顺序来写嗷！！！

#### Required属性

- required = true是默认的，表示引用类型赋值失败时报错，这个required是在@Autowired后面的属性。假的话，不报错哦，引用类型赋值为Null而已。
- 设置为真比较合适，可以尽早暴露出来程序的错误，假的话有问题可能显示不出来嗷。





#### Resource：

- JDK中的注解，Spring框架提供了对于这个注解的功能支持，可以使用它给引用类型赋值，也是自动注入原理，支持两种模式，默认是byName。
- 位置：
  - 在属性定义的上面，无须Set方法，推荐使用。
  - 在Set方法上面
- 过程：
  - 先使用byName自动注入
  - 如果byName失败了，这个时候再使用byType
- 如果只想使用byName怎么办？？？
  - 增加一个属性name
  - name的值是bean的id
  - `@Resource(name="mySchool")`



### Summary：

- 经常改的就用配置文件（非常灵活，代码和它的值是分开的，解耦合了嗷！！！缺点是费时费力，灵活的话代码就低嘛，而且代码和值分开不容易查看）
- 不经常改的就用注解（很快很方便，生产力很高，代码和注解在一起也方便查看嗷！！！缺点是灵活性就差一点，注解的方式现在是一种趋势嗷。而且注解和代码混在一起，代码就有点难顶。）
- 注解为主，配置文件为符。





## 属性配置文件的导入和使用：

![image-20210505153753178](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210505153753178.png)

- 先读入文件的内容

![image-20210505153902319](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210505153902319.png)

- 导入配置文件之后就可以用占位符来通过key获取到value



## 注解总结：

- 常用注解：
  - @Component , 创建对象
  - @Repository , 创建dao对象，用来访问数据库的
  - @Service，创建service对象，处理业务逻辑的，可以有事务功能
  - @Controller , 创建控制器对象，接收请求，显示处理结果的。
  - @Value，简单类型的赋值
  - @Autowired，Spring框架中引用对象的赋值解释，
  - @Resource，JDK中的注解，自动注入给引用类型赋值
- 注解使用：
  - 加入依赖：spring-context
  - 在类中加入注解
  - 在spring的配置文件中，加入组件扫描器
- 自动注入：
  - 由spring根据某些规则给引用类型完成赋值
  - byName根据名称注入，属性名和bean中的id一样，类型一样的赋值
  - byType根据类型注入，有三种方式匹配方式（与class是同源关系的）





### IoC的解耦合解的是啥？

- 实现业务对象之间的解耦合，例如service和dao对象之间的解耦合，关系变得比较松散。
- 对象的赋值通过配置文件来赋值，由容器spring来负责对象的创建和对象之间关系的管理，类与类之间没有本身直接的联系，这样就起到了解耦合的作用。











# AOP学习

- 动态代理：
  - 程序在整个运行过程中根本就不存在目标类的代理类，目标对象只是由代理生成工具在程序运行时由JVM根据反射等机制动态生成的。代理对象和目标对象的代理关系在程序运行的时候才确立。
  - 原有程序不改变的情况下给程序增加功能。（功能增强）
- 方式：
  - JDK动态代理：
    - JDK的Proxy
    - JDK的Method
    - JDK的InovcationHandler
    - 要求目标类必须实现接口（必须有接口）
  - CGLIB动态代理：
    - 原理是继承，因此类不能够是final类，否则就无法实现这种动态代理的原理。
    - 优势：
      - 效率高
      - 要求低



## 动态代理小例子

[b站动态代理例子](https://www.bilibili.com/video/BV1nz4y1d7uy?p=43&spm_id_from=pageDriver)

- 在不修改原有代码的情况下，实现功能增强。执行过程中创建代理对象，来增强功能。
- 步骤：
  - 创建目标类
  - 创建接口实现类，给这个类实现增加的功能
  - 使用jdk中的proxy，创建代理对象，实现创建对象的能力。
- 如果想要只为某些方法来添加动态代理，可以在动态代理中采用判断方法的技巧，因为动态代理中是知道你的调用方法的名字的，通过名字可以进行分类从而实现分别处理。





# 第三节课开始：



## 什么是AOP：

- 底层本质就是动态代理，只不过进行了一种规范化处理而已。

- 实现方式：

  - JDK：
    - 要求目标类必须实现了接口
  - CGLIB：
    - 原理是继承，通过目标类的继承来创建子类

- 动态代理的作用：

  - 在目标类源代码不改变的情况下去增加功能。
  - 减少代码的重复。
  - 专注业务逻辑代码。
  - 解耦合，让业务功能和非业务功能分离。

- 切面定义：

  - 面向切面编程，和上面一样有两种方式。
  - 动态代理实现步骤和方法都定义好了，让开发人员用统一的方式来使用动态代理。
  - 切面：
    - 给你的目标类添加的功能就是切面。特点：非业务方法，一般都是可以单独使用的。

  - 考虑的点：
    - 找出切面
    - 合理安排切面执行的事件（目标方法前后）
    - 合理安排切面执行的位置（哪个类的哪个方法）

- JoinPoint定义：（单个）

  - 连接点，连接业务方法和切面的位置，某类中的业务方法

- Pointcout：（多个）

  - 切入点，多个连接点方法的集合

- 目标对象：

  - 给哪个类的方法增加功能，这个类就是目标对象

- Advice：

  - 通知是用来表示切面的功能执行的事件

- 切面的三要素：

  - 切面的工作代码
  - 切面的执行位置（Pointcut）
  - 切面的执行时间

- ![image-20210505200519099](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210505200519099.png)





## AOP的实现：

- 技术实现：
  - spring在内部实现了aop的规范，可以做aop，主要在事务处理使用aop。项目开发中很少使用spring的aop实现，因为spring的aop比较笨重
  - aspectJ：开源的aop的框架，实际开发中用到最广泛的。spring框架中实际上已经继承了这个框架，通过spring就能使用aspectJ的功能
- 实现aop的两种方式：
  - xml的配置文件
  - 使用注解，我们在项目中做aop功能，一般都使用注解，aspect有5个注解。





## aspectJ的使用：

### 执行时间

- 切面的执行时间，执行时间在规范中叫做Advice通知（增强）。在aspectJ在框架中使用注解来表示的。
- 五个注解：
  - Before
  - AfterReturning
  - Around
  - AfterThrowing
  - After
- 也可以使用xml配置文件中的标签。



### 执行位置

- 使用的是切入点表达式
- ![image-20210505201246663](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210505201246663.png)
- 最常用就是用下面的红色的那两个就够用的。
- 符号使用：
  - \* 0至多个任意字符。
  - \.. 用在方法参数中，表示任意多个参数。用在包名后，表示当前包以及其子路径。
  - \+ 用在类名后，表示当前类和其子类。用在接口后，表示当前接口以及其实现类。
- 例子：

![image-20210505203122218](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210505203122218.png)

- 小技巧：
  - 找到含有service包的全部类
  - `execution(* *..service.*.*(..))`



## aspectJ实现aop基本步骤：

- 新建maven项目
- 加入依赖：
  - spring
  - aspectj
  - 单元测试
- 创建类：接口和它的实现类。要做的是给类中的方法增加功能。
- 创建切面类（普通类）：
  - 在类的上面加入@Aspect
  - 类中定义方法，就是切面要执行的功能代码，在方法上面加入aspectj中的通知注解，例如@Before。还需要指定切入点表达式。
- 创建Spring的配置文件：声明对象，把对象给容器统一管理：标签或注解的方式。
  - 声明目标对象
  - 声明切面类对象
  - 声明aspectj框架中自动代理生成器标签：用于代理对象的自动创建功能
- 创建测试类，从spring容器中获取目标对象（实际上就是代理对象），通过代理执行方法实现aop的功能增强。



### 新项目Maven要添加的依赖

```xml
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
</dependencies>
```

### 创建目标类

- 接口和接口实现类
- 其中接口实现类即为我们的目标类，接口是给反射使用的

### 创建切面类

![image-20210508193713894](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210508193713894.png)

#### @Before：

- 前置通知注解，强调了注解发生的时间。
- 它有一个属性叫做value，是切入点表达式，表示切面的功能和执行的位置。
- 特点：
  - 在目标方法之前执行
  - 不会改变目标方法的执行结果
  - 不会影响目标方法的执行

![image-20210508194406810](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210508194406810.png)

### 创建配置容器配置文件

- 在Spring中注册对应的目标类和切面类。
- 自动代理生成器，会将容器中所有的目标对象都找到，一次性都生成代理对象
- 配置文件内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--把对象交给spring容器，由spring容器统一创建管理对象-->


    <bean id="someService" class="com.tuzi.ba01.SomeServiceImpl"></bean>

    <!--自动代理生成器，使用aspectJ框架内部的功能，创建目标对象的代理对象。
        创建代理对象是在内存中实现的，修改目标对象的内存中的结构，创建为代理对象。
        所以目标对象实际上是被修改后的代理对象-->
    <bean id="myAspect" class="com.tuzi.ba01.MyAspect"></bean>
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```



### 测试是否正确：

![image-20210508200844609](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210508200844609.png)

- 底层本质上用的还是JDK的动态代理。
- 加载配置文件的时候，实际上在扫描到aop那一条的时候才开始找@Aspect，最终生成代理类。相当于和之前相比，多了一步去生成代理而已，其他没啥区别。



## 切面类如果出错

- 如果找切面类中写错了，目标对象的方法找不到
- 切面类就和没有一样，直接执行目标类本身的功能。



## AOP作用：

- 想补充功能但是自己没有源代码
- 给项目中的多个类增加一个相同的功能
- 给业务方法增加事务，日志输出





## 切面类中的参数：

### JoinPoint：

- ![image-20210508212546440](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210508212546440.png)
- 注意这个参数是大写的嗷！！！
- JoinPoint必须位于所有参数中的第一个位置上嗷！！！
- 这个参数是放在方法里面，方法里面的参数是限定的嗷！！！
- ![image-20210508213251647](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210508213251647.png)





## 切面类种类：

### @Before：

- 前置通知，在对应的类执行之前先执行函数中的内容
- 参数：
  - value	切入点表达式
  - JoinPoint



### @AfterReturning：

- 这个方法是有参数的，推荐使用Object，参数名自定义。
- 属性：
  - value   切入点表达式
  - JoinPoint
  - returning    自定义表达式，表示目标方法的返回值，自定义变量名必须和通知方法的形参名一致。
- 位置：方法定义上面
- 特点：
  - 目标方法之后执行
  - 可以获取目标方法的返回值，根据返回值处理不同的功能
  - 可以修改这个返回值

- 例子：
  - ![image-20210508214951572](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210508214951572.png)
- 定义的方法中有返回值的话，可以才可以获得返回值哦。
- 切面类甚至可以悄悄咪咪修改一波返回值，但是在后置通知中修改返回值对于程序的执行是没有影响的嗷QAQ！！！不对不对！！！引用类型是可以变的嗷！！！但是我的方法都执行完了，又没啥影响orz。
- 后置通知的执行：
  - Object res = doOther();
  - myAfterReturing(res);





### @Around：

- 定义格式：
  - public
  - 必须有一个返回值，推荐使用Object
  - 方法名称自定义
  - 方法有参数，固定的参数ProceedingJoinPoint
- 属性：
  - value
- 特点：
  - 功能最强的通知（可以在前后都增强功能）
  - 控制目标方法是否被调用执行
  - 可以修改执行结果，影响最后的调用结果
  - 等同于jdk中的动态代理，InvocationHandle接口
- ProceedingJoinPoint就等同于Method，执行目标方法的
- 返回值：就是目标方法的执行结果，可以被修改嗷
- 具体例子：

![image-20210508220335822](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210508220335822.png)

- 控制是否执行目标函数的功能的例子：

  - 结合JoinPoint一起使用嗷！！！
  - 发现ProceedingJoinPoint是继承于JoinPoint的！！！可以当作JoinPoint来使用。
  - 例子：

  ![image-20210508221043374](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210508221043374.png)

  - 通过参数卡死了方法，不符合条件就不能够执行嗷！！！

- 修改目标方法的执行结果，它返回的结果才是调用代理类所获得的最终结果，换句话说，对于结果进行了加工处理。
- 经常用于事务处理，目标方法之前开启事务，目标方法之后提交事务。





### @AfterThrowing

- 异常通知
- 格式：
  - public
  - 没有返回值
  - 方法名称自定义
  - 方法有一个Exception，如果还有的话是JoinPoint
- 属性：
  - value 切入点表达式
  - throwing 自定义变量，表示目标方法推出的异常对象，变量名必须和方法的参数一样
- 特点：
  - 在目标方法推出异常的时候执行
  - 可以做异常的监控程序，监控方法是不是有异常。如果有异常可以发送邮件，短信等进行通知
- 例子：
  - ![image-20210509092244894](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210509092244894.png)
- 没有异常就不执行，发生了异常可以进行异常通知。
- 本质上是try加上catch来捕获异常的那种用法嗷！！！



### @After

- 最终通知
- 格式：
  - public
  - 无返回值
  - 方法名称自定义
  - 方法没有参数，如果还有的话是JoinPoint
- 特点：
  - 总是会执行
  - 在方法之后执行
- 一般是用于做资源清除工作的
- 就算遇见了异常也会正常执行嗷！！！
- 例子：
  - ![image-20210509093258094](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210509093258094.png)
- 实际上就类似于try...catch...finally中的finally后的代码



### @PointCut

- 辅助的功能注解
- 切入点表达式共有的，引用又长又臭，顶不住。如果有多个可复用的切入点表达式，用@PointCut
- 属性：
  - value , 切入点表达式
- 特点：
  - 如果@PointCut定义在一个方法上面，这个方法名称就成为了切入点表达式的别名。
  - 其他的通知中，value属性就可以使用这个方法的别名

- 例子：
  - ![image-20210509093803580](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210509093803580.png)
  - ![image-20210509093838876](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210509093838876.png)
- 记得上面这个要加上括号哈，mypt()本质上就是一个辅助功能的无代码方法，帮助我们定义切入点用的。这个方法一般是私有的哈，上面这个改一下，它不需要被外界调用哇！！！





## 动态代理种类：

- 如果目标类实现了**接口**，这个时候默认使用的就是JDK的动态代理。
- 没有接口的话默认使用的是cglib来实现动态代理。
- 有接口其实也可以使用cglib来实现动态代理的：

![image-20210509110642999](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210509110642999.png)







## Spring集成MyBatis：

- 使用到的技术是IOC，可以把MyBatis框架中的对象交给spring统一创建，开发人员从spring中获取框架。开发人员就不用同时面对两个或者多个对象了，就面对一个spring就可以了。



### MyBatis的使用：

- 定义dao接口
- 定义mapper文件
- 定义mybatis的主配置文件 mybatis.xml
- 定义dao的代理对象，Student dao = SqlSession.getMapper(studentDao.class)
- 调用接口的方法即可。



要使用dao对象，需要使用getMapper()方法，怎么使用getMapper方法呢？

- 获取SqlSession对象，需要使用SqlSessionFactory的openSession()方法。
- 创建SqlSessionFactory对象，通过读取mybatis主配置文件就能够创建SqlSessionFactory对象。



需要SqlSessionFactory对象，使用Factory就能够获取SqlSession，有了SqlSession就能有dao，目的就是获取dao。Factory的创建需要去读取主配置文件。



主配置文件信息：

大型项目的时候，之前学的主配置文件中的连接池是不够用的，性能太弱了，因此使用其他的连接池，也交给spring来处理。会使用一个独立的连接池类来替换mybatis自己带的，把连接池类也交给spring创建。

=========================================================================================================================================================================================

- 需要让spring创建以下对象
  - 连接池类对象，使用阿里的druid连接池
  - SqlSessionFactory对象
  - 创建出dao对象
- 现在的学习阶段使用xml的bean标签，而且这几个是固定的嗷！！！



### 具体集成步骤：

- 新建Maven项目
- 加入Maven依赖：
  - Spring依赖
  - MyBatis依赖
  - mysql驱动
  - spring的事务的依赖
  - mybatis和spring集成的依赖：mybatis官方提供的，用来在spring项目中创建mybatis的SqlSessionFactory，dao对象的。
- 创建实体类
- 创建dao接口和mapper文件
- 创建mybatis主配置文件
- 创建Service接口和实现类，属性是dao
- 创建spring的配置文件：声明mybatis的对象交给spring创建：
  - 数据源
  - SqlSessionFactory
  - Dao对象
  - 声明自定义的service
- 创建测试类，获取Service对象，通过service调用dao完成数据库的访问。





#### Maven依赖：

```xml
  <dependencies>
    <!--单元测试-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <!--spring用到的ioc核心-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <!--做事务用到的-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <!--mybatis的依赖-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.1</version>
    </dependency>

    <!--mybatis和spring集成的依赖-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.1</version>
    </dependency>

    <!--mysql驱动-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.23</version>
    </dependency>

    <!--国内阿里公司的数据库连接池-->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.12</version>
    </dependency>

  </dependencies>
```

除此之外，build标签中也需要加入对应的配置和依赖：

```xml
<build>
    <!--把src/main/java目录中的xml文件包含到输出结果中，输出到classes目录中-->
    <resources>
      <resource>
        <directory>src/main/java</directory>
        <includes><!--包括目录下的.properties , .xml都会被扫描到-->
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>
    </resources>

    <plugins>
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



#### MyBatis配置：

- Dao中的接口和xml文件照着写就好
- 主配置文件由于不用自带的POOLED了，可以将environment全部删掉



#### 逻辑理清：

这里的service层实际上就是起到了调用Dao层的对象处理业务的作用。



#### Druid的使用：

- [Druid官网](https://github.com/alibaba/druid)

- 配置可以看官网的配置说明。





#### Druid的对象创建：

```xml
<bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <!--本质上是用Set方法对于上面那个类进行注入嗷！！！-->
        <property name="url" value="jdbc:mysql://localhost:3306/mysqllearning"></property>
        <property name="username" value="root"></property>
        <property name="password" value="xxx"></property>
        <property name="maxActive" value="20"></property>
</bean>
```

- 这个代码相对固定



#### SqlSessionFactory的创建：

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="myDataSource"></property>
        <!--mybatis主配置文件的位置，spring中指定其他文件的位置要用classpath嗷！！！-->
        <!--mybatis主配置文件的位置：
            configLocation属性是Resource类型，读取配置文件
            它的赋值用value嗷
        -->
        <property name="configLocation" value="classpath:mybatis.xml"></property>
</bean>
```

- 这个代码相对固定





#### Dao对象的创建：

```xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
        <!--
            指定包名，包名是dao接口所在的包名
            MapperScannerConfigurer会扫描这个包中的所有接口，把每个接口都执行一次
            getMapper()方法，得到每个dao接口的代理对象
            创建好的dao对象放入spring的容器中的，dao对象默认的名称是接口名
            下面的value是包名嗷！！！
         -->
        <!--多个包的话可以用逗号进行分隔-->
        <property name="basePackage" value="com.tuzi.dao"></property>
</bean>
```

- 自动可以创建所有的dao对象，名字为接口名的首字母小写，这样就可以拿到对应的接口对象辽。







#### Service对象的创建：

- Service本质上是对于Dao层的一个封装，所以Service接口里面包含的方法和Dao里面的是有关系的，Dao提供的是最基本的服务，Service则提供复杂的业务逻辑，调用Dao中的功能来实现某些操作，例如给Service一个对象，它先判断你给的对象对不对，对的话再调用底层的Dao来将数据给数据库。Service的接口和实现类最好分开，实现不同的业务功能。
- 除此之外，Service为了使用Dao的代理类，实现类中包含了Dao对象以此来使用Dao层提供的服务功能。

```xml
	<!--创建Service层对象的实现-->
    <bean id="studentService" class="com.tuzi.service.impl.EmployeeServiceImpl">
        <property name="employeeDao" ref="employeeDao"></property>
    </bean>
```







#### Spring整合之后：

- MyBatis被Spring整合之后，事务变成了自动提交的嗷！！！





#### 属性配置文件保存信息：

- 和之前一样，将配置文件单独写，然后在spring主配置文件中导入信息即可。
- 如何在applicationContext导入配置文件：
  - `<context:property-placeholder location="classpath:jdbc.properties">`
- 这个配置能够使得spring知道这个配置文件的存在嗷！！！

![image-20210510211956274](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210510211956274.png)



- 又解耦合啦，多快乐嗷！！！
- 注意下：

![image-20210510212149277](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210510212149277.png)

老师这里说location里面要加上个classpath，但是我不加貌似也没啥问题嗷orz。





## 复习AOP

- 六种通知要再去复习哈：
  - Before
  - AfterReturning
  - Around
  - AfterThrowing
  - After
  - PointCut（辅助）

- 代理的使用方式：
  - 目标类有接口 - jdk代理
  - 目标类没有接口 - cglib代理
  - 有接口也可以强制使用cglib代理
- 切入点表达式
- 使用情况：
  - 增加功能
  - 多个类增加相同功能
  - 事务，日志输出





### 整合Mybatis:

- 使用IoC核心技术，将MyBatis也交给Spring来控制，spring是容器，容器存放你项目中要使用的各种对象。
- 交给Spring的Mybatis对象：
  - 数据源DataSource（Druid）。（它本质上实现了JDBC中的创建连接对象）
  - SqlSessionFactory，使用的SqlSessionFactoryBean在内部创建的SqlSessionFactory。（它利用上面的连接对象，创建了会话的对象工厂，也就是创建了对象）
  - Dao代理对象，使用MapperScanConfigure，在类的内部，调用getMapper，创建接口的Dao对象。（它利用了上面的会话对象，创建了具体的sql语句和returnType）





# 整合Mybatis基本语法结束





## 事务：

### 什么是事务：

- 一组sql集合，要么都成功，要么都失败



### 什么时候用到事务：

- 当操作涉及到多张表，或者是多个sql语句的insert , update , delete 。 需要保证这些语句都能成功才能完成我的功能，否则都失败，保证操作符合要求。

- 事务放在哪儿呢？
  - 业务方法中才可能需要多个dao方法，调度多种方法实现事务，因此需要处理事务。
  - 一般在Service类中使用
- 通常使用JDBC , MyBatis如何实现事务：
  - JDBC: Connection conn ; conn.commit() ; conn.rollback() ; 
  - MyBatis：SqlSession.commit();   SqlSession.rollback();
- 上面处理方式不足：
  - 不同的数据库访问技术，处理事务的对象方法不同，需要了解不同数据库访问计数使用事务的原理。
  - 掌握多种数据库中事务处理逻辑，什么时候提交事务，什么时候回滚事务
  - 处理事务的多种方法
  - 总结：技术太多，太乱太杂
- 怎么解决不足？
  - spring提供了一种处理事务的统一模型，能使用统一步骤，方式完成不同数据库的事务处理。

Spring操作数据库事务示意图：

![image-20210510221850283](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210510221850283.png)



- 处理事务需要怎么做，做什么
  - 将事务使用的信息提供给spring就可以了
  - 内部提交，回滚事务的事务管理器，代替你实现事务
  - 是一个接口和它的众多实现类。
- 接口：PlatformTransactionManager，定义了事务的重要方法 commit 和 rollback。实现类：spring把每一种数据库访问技术对应的事务处理类都创建好了。
- mybatis访问数据库的-----spring创建好的DataSourceTransactionManager

### 怎么使用：

#### 基本用法：

- 告诉spring你使用的是那种数据库访问计数，怎么告诉spring？
- 声明数据库访问计数对于事务管理器实现类，在spring中用bean声明就可以了
- 例如mybaits ---> <bean id='xxx' class="...DataSourceTransactionManager" />



- 你的业务方法需要什么样的事务呢？需要说明事务的类型

- 说明书屋类型的方法包括：

  1. 事务隔离级别

  - ![image-20210511165910263](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210511165910263.png)

  - 以ISOLATION_为开头的，官方文档中可以通过api找得到。
  
  2. 事务的超时时间：
  
  - 一个方法的最长执行时间。如果方法执行时间超过了时间，事务就回滚。时间是秒，默认是-1。难于控制，一般用默认的就可以了。
  
  3. **事务的传播行为**
  
  - 控制业务行为是否有事务
  
  - 七个传播行为：
  
    - ![image-20210511184630473](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210511184630473.png)
    - 前三个掌握就行了。
  
  - 第一个是REQUIRED，是默认值：
  
    - 指定的方法必须在事务内执行，若当前存在事务，就加入到当前事务中。没有就新建一个事务。
    - 例如在调度的过程中，你的主程序属于一个事务当中，你调度的方法同时也需要事务，这个时候这个方法就会加入到当前事务中执行。
    - 例如在调度的过程中，你的主程序不属于一个事务当中，你调度的方法同时也需要事务，这个时候这个方法就会创建一个新事务并在事务中执行。
  
    ![image-20210511185034328](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210511185034328.png)
  
  - 第二个是supports：
  
    - 有事务，无事务都可以哈
    - 例如查询操作就符合这种条件
  
  - 第三个是requires_new：
  
    - 每次自己都要新建一个事务，不用别人的
    - 每次执行到都会挂起别人的事务

#### 提交，回滚事务的时机：

- 提交：
  - 任务执行成功，没有异常抛出，执行完毕后spring自动提交事务。
  
  - 抛出运行时异常或ERROR，spring执行回滚，调用事务管理器的rollback。RuntimeException和它的子类都是运行时异常，例如NullPointer... , NumberFormat...等等
  
  - 抛出非运行时异常，主要是受查异常，提交事务。
  
    受查异常时你在代码中必须处理的遗产给，比如IOException , SQLException等。

### 总结：

- 事务管理器和它的实现类，使用<bean>
- 指定哪些类，哪些方法需要加入事务的功能
- 指定事务的隔离级别，传播行为，超时等



- 你需要告诉spring你项目中类的信息，方法的名称和方法的事务传播行为就可以了。spring帮你做嗷！！！



## 事务实例：

- 这里动力节点的例子讲的非常好，自己回去再看几遍嗷！！！[电商销售平台实例](https://www.bilibili.com/video/BV1nz4y1d7uy?p=85)

- 处理事务的方案：

  - 注解方案：

    - 适用于中小项目的解决方案：
      - 使用注解@Transactional注解增加事务
      - 这个写在public方法上面
      - spring框架自己用aop实现给业务方法增加事务的功能。
      - 我们可以给注解的属性赋值，表示具体的隔离级别，传播行为，异常信息等。
  - 大型项目：
      - AspectJ的AOP配置管理事务。
      - 大量配置事务，使用框架在spring配置文件中声明类，方法需要的事务，这种方法使得业务方法和事务配置完全分离。
      - 实现步骤：都是在xml配置文件中完成的。

### @Transactional

- 使用@Transactional的步骤：
  
  - 声明事务管理器对象。
    - 开启事务注解驱动，告诉spring，我要使用注解的方式管理事务。spring使用aop机制，创建@Transactional所在的类代理对象，给方法加入事务的功能。spring自动帮助你开启和关闭事务，本质上就是用的@Around方式来帮助你开启和关闭事务。

    ![image-20210511203114993](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210511203114993.png)
  
    - 方法上面还需要加上注解。
  
  - 事务实操：
  
    - ![image-20210512205808185](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210512205808185.png)
    - 第二步有一个易错点哈：
    - ![image-20210512205922596](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210512205922596.png)
    - 第二步实操：
    - ![image-20210512210009595](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210512210009595.png)
  
- 上面的完成之后，就可以在实现类中写注解来使用事务啦！！！

- 实现类中的注释实例：

  - ![image-20210512211345019](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210512211345019.png)

- 如果只添加@Transactional的话，结果和上面这个是一样的，因为上面这一坨定义就是默认值。如果需要个性化配置的话，单独去配置属性就好了嗷！！！

- roollbackFor的处理逻辑：

  - 判断异常是否在rollbackFor中，如果在的话一定回滚。
  - 如果抛出的异常不在rollbackFor的列表中，spring会判断异常是不是RunTimeException的异常，如果在的话也回滚

- 默认情况下大多就直接写了个注解就够了嗷，不需要额外配置了嗷！！！



### AspectJ使用配置文件的方式

- 使用方法：
  - 加入依赖
  
  ```xml
  <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-aspects</artifactId>
              <version>5.2.5.RELEASE</version>
  </dependency>
  ```
  
  
  
  - 声明事务管理器对象
  - 声明方法需要的事务类型（配置方法的事务属性）：
    - 隔离级别
    - 传播行为
    - 超时
  
- 下面是操作实例：
  - ![image-20210512213217252](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210512213217252.png)
  - 声明事务管理器对象：
  - ![image-20210512213324448](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210512213324448.png)
  - 有坑！！！
  - ![image-20210512213410220](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210512213410220.png)
  - 声明事务属性（隔离级别，传播行为，超时时间）：
    - 超时时间一般用默认的不用动嗷！！！
    - 配置图片：
    - ![image-20210512214741799](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210512214741799.png)
    - 少了一个查询补上嗷！！！
    - ![image-20210512214909360](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210512214909360.png)
  - 配置顺序就是从上往下哈，优先级就是看详细不，越详细优先级越高嗷！！！！
  - 问题，这个名字不是全限定名称啊，很多重名的咋办捏？？？ -> 还要别的来配置嗷！！！
  - 配置一个aop类，配置切入点表达式，配置哪些包中的类要应用事务（相当于上面定义了事务的规则，下面定义这一套的规则用于哪些类，哪些包中）
  - ![image-20210512215756579](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210512215756579.png)
  - 上面就用上了哟~





# Web环境中怎么使用容器

## 实现步骤：

- 环境：
  - 创建maven web项目
  - 加入依赖：
    - servlet依赖
    - jsp依赖
  
  ```xml
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
      </dependency>
  
      <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2.1-b03</version>
      </dependency>
    </dependencies>
  ```
  
  
  
  - 再拷贝添加spring等依赖。
  - 创建jsp发起请求。
  - 创建Servlet，接收请求参数，调用Service，调用dao完成功能。
  - 创建一个jsp显示结果页面。
  
- Servlet提升版本：
  - 默认生成的web.xml中版本太低了，这儿要把版本生成为4.0，有个小技巧，你要康康下面这个视频嗷！！！
  - [提升版本的方法](https://www.bilibili.com/video/BV1nz4y1d7uy?p=99&spm_id_from=pageDriver)
  
- 无非就是把对于spring的操作放入到servlet的处理逻辑中而已，没啥区别

- 去看我写的spring-mybatis-webapp，这样会比较方便，我写的没啥问题了。

- 一些问题：

![image-20210514120234347](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210514120234347.png)

- 容器创建只需要一次就行，不然太浪费资源了呀！！！然后你就要去看看，这个时候可以就会用到全局对象，把容器对象放入全局对象中，高效利用资源。



## 监听器：

- 负责将容器对象放入全局作用域当中，避免反复创建容器浪费资源。

- 全局作用域对象为ServletContext

- 监听器可以自己创建，也可以使用框架中提供的ContextLoaderListener

- 如何使用ContextLoaderListener：

  - 添加maven依赖

  ```xml
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.2.5.RELEASE</version>
      </dependency>
  ```

  - web.xml文件中注册监听器ContextLoaderListener

  ```xml
  	<listener>
          <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
      </listener>
  ```

  - 监听器被创建对象之后，会读取/WEB-INF/applicationContext.xml，why？

  因为再监听器中要创建ApplicationContext对象，需要加载配置文件，上面那个就是监听器默认读取的spring配置文件的路径。默认的名称就是那个嗷！！！

  ![image-20210514194304502](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210514194304502.png)

  你改了就行嘛，改成类路径下文件的名字（这个时候配置文件就是resources里面的spring配置文件的上面了），注意，这一条配置是加在listener标签上面的嗷。

  实际上记住下面这一坨配置就可以聊，监听器就用上了嗷！

  - 从全局作用域对象中拿到ServletContext和全局作用域对象等。

  上面的web.xml配置文件中的写法：

  ```xml
  	<!--监听器注册-->
      <context-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:applicationContext.xml</param-value>
      </context-param>
      <listener>
          <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
      </listener>
  ```

  代码中获取到这个容器的做法：

  ```java
  		//监听器加入之后的用法
          WebApplicationContext ctx = null;
          String key = WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE;
          Object attr = getServletContext().getAttribute(key);
          if(attr!=null){
              ctx = (WebApplicationContext)attr;
          }
  ```

  

- 监听器作用：

  - 将创建的容器对象放入到全局作用域中



### 工具方法获取全局变量：

```java
ServletContext sc = getServletContext();
ctx = WebApplicationContextUtils.getRequiredWebApplicationContext(sc);
```

- summary：
  - 监听器配置好。
  - 代码中可以从全局变量中调出容器即可。
- 配置监听器：
  - 创建容器对象，创建了容器对象之后，就能够把applicationContext中的所有对象都创建好。
  - 用户发起请求就可以直接使用对象了。





































































































































































































































































































































































































































































































































































































































































## 一些小技巧：

- `alt+enter`可以强制类型转换
- `alt+insert`可以补充方法
- `ctrl+alt+o`是为了清楚配置的设置
- 指定路径的时候，默认resources根目录，例如：`/ba01/context.xml`
- Spring中默认的配置文件名称为applicationContext.xml
- `ctrl+F12`可以查看类结构
- `ctrl+d`可以复制一行
- `ctrl+y`可以删除一行
- `ctrl+h`可以查看类的继承关系
- [快捷键大全](https://www.cnblogs.com/leeego-123/p/10429728.html#:~:text=idea%E8%BF%9B%E9%98%B6%E5%BF%AB%E6%8D%B7%E9%94%AE%EF%BC%9A%201%20%E6%9F%A5%E7%9C%8B%E6%8E%A5%E5%8F%A3%E7%9A%84%E5%AE%9E%E7%8E%B0%E7%B1%BB%EF%BC%9ACtrl%2BAlt%2BB%EF%BC%9A%202%EF%BC%9A%E6%9F%A5%E7%9C%8B%E4%B8%80%E4%B8%AA%E7%B1%BB%E4%B8%AD%E6%9C%89%E4%BB%80%E4%B9%88%E6%96%B9%E6%B3%95%EF%BC%9AAlt%2B7%20%E6%88%96%20%E7%82%B9%E5%B7%A6%E4%BE%A7%E8%BE%B9%E6%A0%8FStructure,2%20%E8%BF%94%E5%9B%9E%E4%B8%8A%2F%E4%B8%8B%E4%B8%AA%E5%85%89%E6%A0%87%E5%9C%B0%E6%96%B9%3AAlt%2BCtrl%2B%E7%AE%AD%E5%A4%B4%20%28%E5%90%91%E5%B7%A6%29%20%E5%92%8C%20Alt%2B%20Ctrl%2B%E7%AE%AD%E5%A4%B4%20%28%E5%90%91%E5%8F%B3%29---%EF%BC%88%E5%85%A8%E9%94%AE%E7%9B%98%EF%BC%89)









### 数据库和Java对象映射

#### 读取的时候：

- 数据库的类型据我发现绝大部分都能够映射为String类型，例如serial啊，数字类型啊等。
- 但是数据库底层如果是char类型的数据 ，就不一定能映射为Java对象中的Integer或者是其他数字类型了。





#### 插入的时候：

- 你设置的类型这个时候就最好和数据库底层符合，不然看起来就会在插值的时候有点怪异orz
- (小声：虽然好像确实可以这么操作，但是这么操作不太好嗷！！！)[关于类型转换](https://blog.csdn.net/weixin_43894879/article/details/108063492)



















