---
title: MyBatis框架学习
date: 2021-04-26 21:54:49
tags: 后端
categories: 动力结点后端课程
---





# 终于走到了SSM框架，先上MyBatis嗷，把这个看完了再去上Spring。。。



<!--more-->



## 三层架构：



### 三层架构概念：

- 界面层（视图层）- 接收用户的数据，显示请求的处理结果给用户。(JSP和Servlet和html都是)
- 业务逻辑层（Service）-接收界面层数据和计算逻辑，检查数据，业务逻辑，调用数据访问层数据
- 数据访问层（持久层） -与数据库打交道的，执行对于数据的查询，修改和删除等等



### 三层对应的包：

- controller（servlet）
- service（XXXService类）
- dao （XXXDao类）

![image-20210427164919108](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427164919108.png)



### 三层对应框架：

- 界面：springmvc
- 业务逻辑：spring
- 数据访问：mybatis





## JDBC缺陷：

- 代码重复，开发效率低
- 对象查询麻烦，释放资源，自己封装成List
- 业务代码和数据操作会混杂在一起



- `DButil`我们写过，就是为了解决这些弊端







## MyBatis框架概述

- 定义：框架
- 下载：代码在github上[MyBatis下载](https://github.com/mybatis/mybatis-3)
- 重要概念：
  - sql mapper: sql映射，把一行的数据映射为一个Java对象，操作这个对象就相当于操作表中的数据。
  - Data Access Objects(DAOs)：数据访问，增删改查
- 功能：
  - 提供了创建Connection，Statement , ResultSet的能力。不同我们创建。
  - 执行sql语句的能力，不用你执行sql。
  - 循环sql，把sql结果转为java对象，List集合的能力。
  - 关闭资源的能力
- 开发人员做的是，提供sql语句，MyBatis执行，返回List集合或者Java对象就结束了。
- 总结：
  - mybatis是一个sql的映射框架，提供了数据库的操作能力，增强的JDBC，使用MyBatis让开发人员集中精神写sql就可以了，不用关心其他的东西。







## 如何配置和使用MyBatis



- 下载后解压的内容：

![image-20210427171828369](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427171828369.png)



文档和核心的jar包，这个其实吧，用途不大，主要是文档orz。mybatis的添加其实直接加依赖就好了，文档中有添加对应依赖的语句，或者在之前那个仓库找也可以。其实不用下这个的，[MyBatis官方中文文档](https://mybatis.org/mybatis-3/zh/index.html)。



### 查询数据库例子：

- 配置添加：

![image-20210427172750340](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427172750340.png)

- 需要的配置：mysql和mybatis的依赖

- XML文件的书写：

  - 与dao接口处于同一个文件夹下
  - 文件名称和接口保持一致

- 具体写法：

  - 找文档！！！文档写的很详细的。
  - ![image-20210427173815212](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427173815212.png)
  - ![image-20210427173922946](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427173922946.png)
  - mapper是当前文件根标签，必须的
  - namespace:
    - 命名空间，唯一的值，可以是自定义的字符串。
    - 要求你使用dao接口的全限定名称。
    - 这个要改成当前的目录下的包名+类名（可以用相对路径选择）
    - 全限定名称获取：![image-20210427182327785](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427182327785.png)
  - 可以使用特定标签标识特定操作：
    - select表示执行查询
    - update表示更新
    - insert表示插入
    - delete表示删除
  - 语句实例：![image-20210427182426346](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427182426346.png)

  - mybatis会找到这些数据并且将数据赋予对应的对象



#### 连接数据库配置：

![image-20210427183122020](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427183122020.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

- 这个文件，名字可以自己取，放在resources根目录之下，内容不用去背，这个是MyBatis住配置文件，sql映射文件的位置

- 内容解读：

  - 最上面是约束文件，约束格式的。

  - configuration是根标签，配置总标签。

  - environment是环境的配置文件，表示环境的名称的:

    - transactionManager: mybatis事务类型，type: JDBC表示使用JDBC中的Connection对象的commit和rollback做事务处理。
    - 下面的用的少，可以不怎么看了。

  - dataSource表示数据源，连接数据库的：

    - 下面的这些是数据库的连接配置文件
    - name中的driver , user , username , password是固定值，不可以改的

    ![image-20210427183715467](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427183715467.png)

- environments中有个default，必须和某个environment的id值一样嗷，告诉mybatis使用哪个数据库的连接信息，也就是使用哪个数据库。就是你底下可以为了方便配置多个数据库的登录信息，但是当你真的去使用数据库时，你需要通过default告诉数据库去具体使用哪个配置。

- 把你在代码中写的移动到了配置文件中而已

- 底下还有个mappers，一个mapper指定一个文件的位置。从类路径开始的位置指定路径信息。`target/classes(类路径)`

- 为了编译过程中把其他文件也加入进去：

  - ![image-20210427184407995](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427184407995.png)
  - 可以多安装一个插件就可以了。（其实我的IDEA默认有的，加一个build的配置就行）
  - 并且需要：
    - ![image-20210427184603851](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427184603851.png)

  - 需要指定你需要的文件的位置嗷：

  ![image-20210427184748424](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427184748424.png)

  位置是你编译的位置就可以了。



### 执行查询程序：

- 步骤：
  - 定义mybatis主配置文件的名称，从类路径根开始
  - 读取config表示的文件
  - 创建sqlSessioinFactoryBuilder对象
  - 创建sqlSessionFactory对象
  - **获取sqlSession对象，获取SqlSession**
  - **指定要执行的sql语句的表示，sql映射文件中的namespace+'.'+标签的id值**
  - 执行sql语句
  - 输出结果
  - 关闭sqlSession对象
- 例子：

![image-20210427191658238](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210427191658238.png)

默认目录在target的classes文件夹下开始的，编译后的文件才是开始执行的文件

- 过程好复杂orz



## 三种解决xml文件没有在target中的方式：

[b站相应教程](https://www.bilibili.com/video/BV185411s7Ry?p=11)

- 总结：
  - clean之后再compile
  - build中的rebuild
  - file中restart IDLE
  - 实在不行手动把文件拷到对应的位置就可以了。



## 插入操作：

![image-20210428192023934](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210428192023934.png)

- MyBatis默认是不自动提交事务的，要手动提交
- `sqlSession.commit();`
- 过程：
  - 原有过程中添加修改：
    - 增加抽象的Dao中的方法定义
    - xml指定新的sql语句
    - 执行的java文件中更改执行的id



## 日志开启

![image-20210428171014278](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210428171014278.png)

- 可以看到语句和语句执行的情况。



# 主要类的介绍：

## Resources类：

- mybatis中的一个类，负责读取主配置文件
- `InputStream in = Resources.getResourceAsStream("mybatis.xml");`

## SqlSessionFactoryBuilder：

- 创建SqlSessionFactory对象
- `SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();`



## SqlSessionFactory：

- 重量级对象，创建时间长，使用资源多，项目中有一个就够用。
- `SqlSessionFactory factory = builder.build(in);`

- 接口，实现类`DefaultSqlSessionFactory`
- 作用：
  - 获取SqlSession对象
  - `SqlSession sqlSession = factory.openSession()`
- 方法说明：
  - openSession()：无参数的，获取是非自动提交事务的SqlSession对象
  - openSession(boolean)：openSession(true)就是自动提交的



## SqlSession:

- 也是个接口，定义了操作数据库的方法，例如：selectOne() , selectList() ....
- 实现类：DefaultSqlSession
- 使用要求：
  - 不是线程安全的，需要在方法内部使用，在sql语句执行之前，使用openSession()获取SqlSession。执行完sql语句之后关闭它，`SqlSession.close()`，这样能保证它的使用是线程安全的。





# 其他内容

## 工具类编写：



- static是为了使得定义静态的共有资源，方便资源共享与方法的使用而不需要实例化对象。[一些讲解](https://blog.csdn.net/MyySophia/article/details/88593596)





- 工具类代码

```java
package com.tuzi.util;

import java.io.InputStream;
import java.io.IOException;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class MyBatisUtils {

    private static SqlSessionFactory factory = null;
    static {
        String config = "mybatis.xml";
        try {
            InputStream in  = Resources.getResourceAsStream(config);
            //创建SqlSessionFactory对象，使用SqlSessionFactoryBuild
            factory = new SqlSessionFactoryBuilder().build(in);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // 获取SqlSession方法
    public static SqlSession getSqlSession(){
        SqlSession sqlSession = null;
        if (factory!=null){
            sqlSession = factory.openSession(); //非自动提交事务
        }

        return sqlSession;
    }
}

```



## 传统Dao：

### 代码模板的使用：

- File -> Setteings -> Editor -> File and Code Templates -> Files -> + ->写一个名字，extension换成对应的文件格式后缀
- 把内容定下来，并且起一个名字就ok
- 比如mybatis.xml文件啊之类的配置文件就可以配成模板
- 创建文件的时候，会自动显示出来可以选模板的嗷！！！
- ![image-20210428200835601](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210428200835601.png)
- 完成实现类来实现Dao，通过实现类来完成这些功能，而不是在xml中指定返回类型。





# 后一章重点内容

## 动态代理：

![](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210428201711453.png)

- Mybatis动态代理：
  - 更具Dao的方法调用，获取执行sql语句的信息
  - mybatis根据你的dao接口，创建出实现类，并创建这个对象，完成SqlSession调用方法，访问数据库。
  - Mybatis帮你干，所以不用你去写实现类了。

### 动态代理使用：

![image-20210428202225135](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210428202225135.png)

- Mybatis就帮你创建了实现类（相当于你上面干的事儿）
- 然后就可以直接使用“实现类”的方法来解决这个问题

![image-20210428202320504](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210428202320504.png)

- 和上面相比，相当于把`sqlId`的selectList(sqlId)这种方式转换成通过class信息，mybatis直接帮你创建好对应的方法实现类来获得可以执行功能的数据库对象。
- 核心代码：`StudentDao dao = sqlSession.getMapper(StudentDao.class);`
- 然后就可以使用`dao`中的方法来执行sql语句并返回结果。







## 传入参数：

### parameterType：

- 从java代码中把数据传入mapper文件的sql语句中.
- parameterType: 写在mapper文件中的一个属性，表示dao接口中方法的参数的数据类型。

- 使用例子：

![image-20210429090504885](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210429090504885.png)

- 去掉好像也没有问题嗷！！！
- 原因：
  - parameterType: dao接种中方法参数的数据类型。
  - 值是java中数据类型的全限定名称或者是mybatis定义的别名。例如上面写成int也行（这个别名是mybatis文档中告诉你它自己定义的嗷！！！）

- 别名：

![image-20210429090811057](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210429090811057.png)



具体在你下载的mybatis文档的第15页左右。

- parameterType不是强制的，一般我们不写，myBatis可以通过反射来知道传入参数的类型的，问题不大。



### 一个简单参数：

- mybatis把java的基本数据类型和String都叫做基本类型

- 在mapper文件中获得简单类型 一个值，使用`#{任意字符}`。

- 本质：

  - 写了#{}之后，mybatis执行sql是使用的jdbc中的PreparedStatement对象

  - 由mybatis执行了下面的内容：

    ![image-20210429091627190](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210429091627190.png)

- 因此你一开始定义的对象里面一定要有无参构造方法和set和get的值，都要有，不然mybatis通过反射拿不到属性名和对应的方法的嗷。



### 多个参数：

#### @Param命名参数（推荐使用）：

- ![image-20210429092049496](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210429092049496.png)

- 在参数前面使用，`@Param("自定义参数名称")`
- 然后在mapper文件中去使用这个命名。



#### 使用对象传参（推荐使用）：

- 新构造一个Java类，含有get和set的那种，在dao中的抽象方法中，加入一个传入对象的抽象方法。
- 用对象的属性的值来代表内容。

![image-20210429094840854](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210429094840854.png)



#### 使用位置传参（不推荐）：

- 按照0的位置开始
- `#{arg0} #{arg1}`这种形式能够获得多个参数的值
- 位置有可能出错嗷，不是特别清楚嗷！！！



#### 使用Map传参（不推荐）：

- 使用Map存放多个值
- 用key去取参数
- 一个实例：

![image-20210429101058674](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210429101058674.png)

- 可读性差，不用Map。阿里巴巴开发文档说明了不要使用Map开发。



### #和$的区别：

- #{...}代替sql语句的?更加安全
- #占位符：
  - 放在列名后面，在值前面。
  - 主要用于插值
- $字符串替换：
  - 主要用于替换表名，列名和不同列排序等操作
  - 使用$包含的“字符串”替换所在位置
- ![image-20210429102140137](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210429102140137.png)
- 相当于对应的就是Statement和PreparedStatement
- 使用$的一个小栗子：

![image-20210429210006514](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210429210006514.png)

![image-20210429210059074](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210429210059074.png)

- $可以替换表名或者列名，你能确定数据是安全的。





#### 使用占位符替换列名

- 本质上就是字符串的拼接（注意括号的情况）

- \# 使用 ? 在sql语句中做站位的，使用PreparedStatement执行sql，效率最高。
- #能够避免sql注入，更加安全。
- \$ 不适用占位符，是字符串的连接。使用Statement执行sql，效率更低，缺乏安全性。
- \$ 可以替换表名或者列名`select * from emp where ename='liututu' order by ${strategy}`，这个strategy就是通过sql注入实现了排序的方式的多样性。





## 内容复习：

- 动态代理：
  - mybatis帮你创建dao实现类，调用实现类中的方法来执行sql语句。
  - 使用过程：
    - `sqlSession.getMapper(接口.class)`
    - 使用dao接口的方法，调用方法就执行了mapper文件中的sql语句。
  - 要求：
    - dao和mapper在同一个目录。
    - dao接口和mapper文件名称一致。
    - mapper文件中的namespace的值是dao接口的全限定名称，
    - mapper文件中的select , insert , update , delete等的id是接口中的方法名称。
    - dao接口中不要使用重载方法，写唯一的，不然反射可能出问题！！！
- 传参方式：
  - 一个简单类型的参数：#{任意字符}
  - 多个简单类型的参数，@Param("自定义名称")
  - 使用一个Java对象，这个对象必须要有一个无参构造以及每个参数对应的get方法，不然反射玩不起来、对象作为mapper文件找到参数
  - 使用参数的位置：语法为`#{arg0},#{arg1}`
  - 使用map作为参数，#{map的key}
- #和$的区别：
  - 都是占位符
  - #是用来表示列值的，放在等号右侧
  - $表示字符串的连接，把sql语句连接成一个字符串
  - #使用的jdbc指定PreparedStatement对象执行SQL语句，效率高没有sql注入的风险
  - $使用的是Statement，效率低一点，有注入的风险。



## 封装输出结果resultType：

- mybatis执行了sql语句得到了java对象

- resultType结果类型，sql执行完毕后，数据转为Java对象。

- 查询语句必须有resultType

- 处理方式：

  - mybatis执行sql语句，调用无参构造创建对象。

  - mybatis把ResultSet指定列值赋给同名的属性。

  - 对等的jdbc:

    ![image-20210430103629569](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430103629569.png)

- 不管返回值是单个对象还是List，写的都应该是你创建的那个对象嗷！！！

- resultType中的数据类型是任意的，只要能够将sql结果转为这个类就ok。

- 很灵活，这个对象可以很灵活的改变嗷，根据需要不一样就可以定义不一定的对象。





### 定义别名：

- 例子：`select count(*) from student;`
- 要么用别名，要么用接口全限定名称

![image-20210430105252452](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430105252452.png)

- 类型和它的值：
  - 类型的全限定名称
  - 类型的别名
  - 建议使用全限定名称
- 思考：![image-20210430110157706](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430110157706.png)

- 定义自定义类型的别名：
  - mybatis主配置文件中定义，使用\<typeAlias>定义别名。
  - 可以resultType中使用自定义别名。

#### 方式一：

- 具体形式：

![image-20210430110444351](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430110444351.png)

- 多的都可以在这里配置嗷！！！

- 一次给一个对象定义别名嗷。



#### 方式二：

- package中，name是包名，这个包中的所有类，类名就是别名，类名不区分大小写。
- 具体形式：

![image-20210430111403195](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430111403195.png)

- 感觉很好用哇！！！



#### summary：

- 第二种用到多，但是有可能发生冲突的情况。
- 第一种灵活，但自定义名称也有可能冲突。
- 综上所述，推荐使用全限定名称。





### Map：

- 定义一个方法返回Map，推荐`Map<Object,Object>`，返回的值实际上是查询的列名为key，查出来的值为value。
- 返回值为Map的时候，只能最多返回一行记录，多余一行是错误的。使用的少嗷。返回对象是最方便的。



## resultMap：

- 结果映射，指定列名和java对象的属性对应关系。

- 使用场景：

  - 自定义列值赋予哪个属性
  - 当你的列名和属性名不一样的时候，一定使用resultMap，默认是相同的属性名和相同的列名是对应的。

- 使用方法：

  - 定义resultMap。
  - 在select标签中，使用resultMap来应用定义的。

  ![image-20210430202139430](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430202139430.png)

  - 可以自己定义，定义的resultMap是可以复用的。
  - 没有指定上的属性自动补上NULL嗷！！！
  - 指定列和属性的赋值关系。

- resultMap的id必须要是唯一的嗷！！！

- resultMap和resultType不要一起使用，只能使用一个嗷！！！

## 列别名：

- resultType的原则是：
  - 同名的列赋值给同名的属性
  - 因此可以使用列别名来解决这个问题，列别名（as...），要是属性名

上面两个都可以，但是推荐用第一种嗷！！！





## 模糊查询：

### 第一种：

- Java代码中指定like的内容
- 使用占位符号: \#{name}
- 推荐使用这个，直接传入这个参数就好啦









### 第二种：

- 拼接字符串来获得

![image-20210430205152995](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430205152995.png)



- 以上两种推荐第一种。





# 后一章重点内容：

## 动态sql:

- 通过条件获得不同的sql语句。
- 主要是where部分发生变化。
- 使用的是mybatis提供的不同的标签来实现。
- 动态sql使用java对象作为参数。







### if标签：

![image-20210430210220686](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430210220686.png)

- 相当于对于某些条件进行判断，以构造合法有意义的sql语句。

- 当上面的有问题，下面的没问题的时候，出问题了，语句多了一个and，这个地方有一个小技巧：

  ![image-20210430210655009](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430210655009.png)

  注意，可能下面的语句也要改哦。





### where标签：

- 包含多个if的，当多个if有一个成立的话，where标签会自动增加一个where关键字，并去掉if中多余的and和or等
- 哇塞，神奇嗷呜！！！



### foreach标签：

- 循环Java中数组，list集合的，主要用在sql的in语句中。
- 适用于`in (a,b,c)`这种方式



用法：

![image-20210430215334659](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430215334659.png)

- 语法规则：
  - collection：接口中的方法参数的类型，数组是array，list使用list。
  - item：自定义的，表示数组和集合成员的变量。
  - open：循环开始时候的字符
  - close：循环结束时候的字符
  - separator：成员之间的分隔符
- 具体例子：

![image-20210430220019973](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430220019973.png)

- 也可以这么写：

![image-20210430220232248](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430220232248.png)





### 动态SQL之代码片段

- 复用常用的sql语句封装成代码片段
- 步骤：
  - 先定义\<sql id="自定义名称唯一"> sql语句 ， 表名 ， 字段等\</sql>（是在mapper中的xml定义的嗷）
  - 再使用，\<include refid="id的值">
- 上面直接对应的替换就ok了。 



## Mybatis主配置内容：

- settings：
  - 配置项的内容有好多，文档中有，可以取康康嗷，默认的其实一般就够用了。
- transactionManager：
  - 提交事务回滚事务的方式，type是事务处理的类型。
  - type的类型：
    - JDBC：表示mybatis底层是调用JDBC中的Connection对象的，commit和rollback
    - MANAGED：把mybatis的食物处理委托给其他容器。（一个服务器软件或者给Spring去赶）

- datasource：
  - 数据源，java体系中，实现了javax.sql.DataSource的都是数据源。
  - 所以为了获取这个连接，必须给出驱动啊，用户名和密码啊等信息嗷。
  - 这里的type指定数据源的类型：
    - POOLED：使用连接池，会创建PooledDataSource类
    - UNPOOLED：不使用连接池，每次执行sql，先创建连接，执行sql，再关闭连接。Mybatis会创建一个UnPooledDataSource管理Connection对象的使用。一般情况下不用的嗷。
    - JNDI：Java命名和目录服务（windows注册表）



# 后一章重要内容：

## 属性配置文件：

- 把数据库信息放在一个单独的文件中，和mybatis主配置文件分开。目的是便于修改，保存和管理多个数据库的信息。

- 步骤：

  - resources目录中定义一个属性配置文件，例如：jdbc.properties。在属性配置文件中，定义数据，格式是：key-value。一般使用 \. 做多级目录的。这样代码结构比较清晰。

    ![image-20210501101334981](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210501101334981.png)

    ![image-20210501101636758](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210501101636758.png)

    整体结构很清晰，通过 \. 的方式可以很直白的表示这是jdbc的相应数据。

  - 然后在主配置文件中，引入相应的配置文件。

    ![image-20210501104503707](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210501104503707.png)

  - 这样就完成了相应的配置，可以成功执行sql！！！





## Mybatis主配置内容（续）:

- mapper标签一次指定一个文件，如果需要多个dao文件的话，需要多个mapper来指定多个mapper文件。
- 使用报名来指定mapper文件，name是xml文件（mapper文件）所在的包名。这个包中所有的xml文件，都能被一次加载进来，一些要求：
  - mapper文件名称需要和接口名称一样，区分大小写，都要一样。
  - mapper文件名和dao接口需要在同一目录中。



## PageHelper：

- 做数据分页的，在mysql中是limit语句
- Maven添加相应的依赖：

```xml
<!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
<dependency>
  <groupId>com.github.pagehelper</groupId>
  <artifactId>pagehelper</artifactId>
  <version>5.2.0</version>
</dependency>
```

- 添加完相应的依赖后，重新load才可以，然后要在mybatis的主配置文件的environments之前加入plugin。

  ![image-20210501110612835](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210501110612835.png)

- 相当于过滤器的作用。

- 使用：

![image-20210501110745159](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210501110745159.png)



- 使用：

  - ![image-20210501112120397](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210501112120397.png)

  - 不是mybatis框架的，是一个小组件，便于分页处理。















# 一些重要的补充知识点

- select语句中查询的属性和你对象属性的映射关系：

![image-20210430111015421](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210430111015421.png)











