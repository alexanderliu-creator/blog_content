---
title: SpringMVC框架学习
date: 2021-05-15 14:39:11
tags: 后端
categories: 动力结点后端课程
---





# 这里是刚刚学完Spring的兔子，直接上SpringMVC，冲冲冲！！！

<!--more-->

---

## 基本概念：

- 是基于spring的一个框架，实际上就是spring的一个模块，专门是做web开发
- 相当于一个servlet的包装升级版，底层本质上还是servlet，在底层基础上加入了一些功能，让我们做web开发更加方便！！！
- SpringMVC就是一个Spring , Spring是一个容器，ioc能够管理对象，使用<bean> ,  @Component等注解也能使用。
- springMVC能够创建对象，放入到容器中（SpringMVC容器），里面存放的是控制器对象。我们要做的就是使用@Controller创建控制器对象，这个对象能够处理用户请求，显示处理结果，当作一个Servlet来使用。



### 控制器如何起作用：

- 前面学过了@Controller实际上创建的也只是一个普通类对象，而不是Servlet，SpringMVC赋予了控制器对象一些额外的功能。

- 由于web开发底层是Servlet，SpringMVC中又一个对象是Servlet , DispatcherServlet，之后这个Servlet对象把请求转发给我们的Controller对象，最后是Controller对象处理请求。

- DispatcherServlet被称为中央调度器，流程为：

  index.jsp -> DispatcherServlet -> 转发分配给Controller对象（@Controller注解创建的对象）

- Controller可能会有多个的嗷！！！





## MVC第一个项目

- 和之前一样用maven模板的web项目去创建项目
- 一样的，生成的web.xml文件的依赖版本太低了，需要用4.0版本的
- 需要加入spring-mvc的依赖：
  - spring-webmvc依赖，它会简介把spring依赖都加入到项目中
- jsp , servlet依赖
- 重点：
  - web.xml中注册springmvc的核心对象：DiapatcherServlet(中央调度器)，是一个Servlet , 父类是继承HttpServelt
  - DIspatcherServlet叫做前端控制器
  - 负责接收用户请求，调用控制器对象处理，将response返回显示给用户。
- 创建控制器类：
  - 上面加入@Controller注解
  - 创建对象并且放入springmvc穷奇中
  - 在类中方法上面加入@RequestMapping注解
- 创建作为结果的jsp，显示结果
- 创建Springmvc配置文件（和spring的配置一样）
  - 声明组件扫描器，指定@Controller所在的包名
  - 声明视图解析器，帮助处理视图的。



### 实际操作：

- 依赖添加：

```xml
	<dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
```

- 更新web.xml版本：[实际操作](https://www.bilibili.com/video/BV1sk4y167pD/?p=3&spm_id_from=pageDriver)
- 声明核心对象：DiapatcherServlet
  - 声明这个对象
  - 让服务器启动的时候，立马创建这个对象嗷！
- 创建这个DispatcherServlet的过程中，会去读取/WEB-INF/servletName-servlet.xml文件作为配置文件。默认规则不灵活，一般自定义这个配置文件所在的位置。文件所在的位置一般改为classpath: , 这个时候配置文件就应该放在resources文件夹下。

web.xml中核心对象的声明与配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--
        声明注册springmvc核心对象
        需要在Tomcat启动后就会创建这下面这个对象的实例
        因为Servlet在创建过程中，会同时创建springmvc容器对象
        读取springmvc的配置文件，将配置文件中的对象都创建好
        当用户发起请求时，就可以直接使用对象了。有点像spring中最后讲的监听器

        Servlet初始化执行init()方法，DispatcherServlet在init()中(
            //创建容器，读取配置文件
            WebApplicationContext ctx = new ClassPathXMLApplicationContext("springmvc.xml")
            //将对象放入ServletContext中
            getServletContext().setAttribute(key,ctx);
        )
    -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--
            tomcat启动后，创建servlet对象
            load-on-startup：表示Tomcat启动后创建对象的顺序
            数值越小，创建这个对象的时间越早，为大于等于0的整数。
            0一般也不用。
        -->

        <!--创建springmvc容器中的内容读取以下这个路径的文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!--
        使用框架的时候，url-pattern可以使用两种值：
           1. 使用拓展名的方式，*.xxxx , xxxx是自定义拓展名，常用的方式：
           *.do , *.action , *.mvc等等
           http://localhost:8080/myWeb/some.do

           2. 使用斜杠“/”
    -->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>


</web-app>
```

其中，关于springmvc的配置文件放在了resources目录下，和spring一样的，通过这个容器可以构造springmvc对象和处于容器中的所有对象。

- index.jsp默认的那个不是很全哈，建议删了重新自己再去建一个嗷！！！
  - 可以通过<a href="some.do" />这种方式来处理请求哈
- 创建一个控制类 -> 处理器对象

```java
package com.tuzi.controller;


import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;


//创建处理器对象，对象放在springmvc容器中
@Controller
public class MyController {
    //处理用户提交的请求，springmvc中是使用方法来处理的
    //方法是自定义的，可以有多种返回值，多种参数，方法名称自定义

    //@requestMapping，请求映射，将一个请求地址与一个方法绑定在一起。
    //value的值必须是唯一的，表示对应请求的uri地址的。使用时推荐以“/”为开头

    //使用位置：方法上面 ， 类的上面
    /*
    *   方法上面：处理器方法，控制器方法
    * 使用@RequestMapping修饰的方法是可以处理请求的
    * 类似于doPost和doGet
    *
    * 返回值是ModelAndView：
    *   Model：数据，请求处理完成后，显示给用户的数据
    *   View：视图，比如jsp等待
    * */
    @RequestMapping(value = "/some.do")
    public ModelAndView doSome(){
        //处理some.do的请求了
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","欢迎使用springmvc做web开发");
        mv.addObject("fun","执行的是doSome方法");

        //框架在请求的最后把数据放入request作用域中，调用request.setAttribute
        mv.setViewName("/show.jsp");
        //框架对于视图执行的就是forward操作，相当于request.getRequestDispatcher("/show.jsp").forward(...)

        return mv;
    }
}
```

- 声明组件扫描器：

  - 在springmvc.xml里面声明嗷

  - ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
        
        <context:component-scan base-package="com.tuzi.controller" />
    </beans>
    ```

  - 里面要把你的注解包括进去，才能扫描到注解然后创建好对象哇！！！

- 能处理请求的都是控制器对象（处理器），可以把上面的MyController称为后端控制器。

### 处理流程：

- b发送some.do -> tomcat(web.xml -- url-pattern知道*.do的请求给DispatcherServlet
- 这个时候这部分的请求就给了这个中央调度器DispatcherServlet（根据springmvc.xml配置文件） --> 把some.do转发给MyController.doSome()方法
- 框架执行doSome()把得到的ModelAndView进行处理之后，转发给show.jsp
- summary:
  - some.do --> DispatcherServlet --> MyController
- DispatcherServlet的作用：
  - 负责创建springmvc容器对象，读取xml配置文件，创建文件的Controller对象
  - 负责接收用户的请求，分派给自定义的Controller对象





### SpringMVC执行源代码分析：

- tomcat启动，创建容器过程（load-on-start指定的1来创建DispatcherServlet对象）

  - init()方法中，创建容器，读取配置文件
  - 把容器对象放入到ServletContext中：
    - getServletContext().setAttribute(key , ctx)

- 请求处理过程：

  - 执行Servlet的操作
  - doDispatcher是核心方法
  - 过程：

  1. protected void serive
  2. protected void doService
  3. this.doDispatch(request , response){ 调用MyController的.doSome() }

  - 本质上doDispatcher方法调用了doSome()方法，doDispatcher()方法是中央调度器调用了controller，调用了doSome()方法

## 用户非法访问show.jsp

- 把不想给用户看到的页面放入WEB-INF文件夹中，这些文件是不对用户开放的。
- 由于项目中保护的页面可能很多，就要向WEB-INF下面创建一个子目录。专门存放这些页面，这样比较安全。
- 但是这个时候就需要我们改路径来让程序找的到我们受保护的页面。让Controller能够返回正常的页面。`mv.setViewName("/WEB-INF/view/show.jsp");`
- 如果多个页面都需要写就很麻烦嘞，这个时候就需要在springmvc.xml中配置视图解析器，帮助开发人员设置视图文件的路径

```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀：视图文件的路径-->
        <property name="prefix" value="/WEB-INF/view/" />
        <!--后缀，表示视图文件的拓展名-->
        <property name="suffix" value=".jsp" />
</bean>
```

这个时候就可以使用逻辑名称（文件名）来指定视图，框架会自动用prefix + 逻辑名称 + suffix来实现字符串的连接来找到对应的。这样就实现了部分页面的屏蔽嗷！！！



### 关于mvc的小总结：

- 一个类中可以有多个方法对应多个请求
- 一个方法也可以被多个请求调用，很方便
- 以后写数据库可能只需要写四个方法就可以啦，就很方便



## @RequestMapping的用法

- value：所有请求地址的公共部分，叫做模块名称
- 位置：类的上面。
  - 相当于定义了公共的路径：/test/some.do , /test/other.do , 公共路径（模块儿）就为/test
- ![image-20210515203339787](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210515203339787.png)
- method：表示请求的方式，它的值是RequesetMethod的枚举值。例如表示get请求方式：RequestMethod.GET
- 例如指定some.do使用get请求方式，在方法对应的Mapping上面多家一个属性，`method = ...`
- 当不指定请求方式的时候，就没有任何的限制嗷！
- HTTP状态码为405，表示当前页面不支持你使用的访问方式（例如不支持POST方式访问）





## 怎么接收用户的请求参数

- 放在方法参数位置来获取对应的参数
- 你只要在方法的参数列表中写上对应的方法，SpringMVC就会自动将值赋予参数，例如HttpServletRequest
- 那就可以拿到 , `request.getParameter("paraName")`



- 接收请求参数有以下四种：

  - HttpServletRequest
  - HttpServletResponse
  - HttpSession
  - 用户提交的数据

- 接收用户提交的参数：

  - 逐个接收

  - 对象接收

### 逐个接收参数：

  - 要求：
    - 处理器方法的形参名和请求中参数名必须一致，同名的请求参数赋值给同名的形参
  - 框架内接收请求参数：
    - 使用request对象接收请求参数
    - 框架自动调用getParameter获得参数，注意哈，这个时候接收都是用String去接收的，就和我上次在数据库中发现的一样，String是万金油。
    - 当你调用doSome的时候，就会按照变量的名称（这就是为啥必须一致），按照变量类型进行赋值并完成转型。
    - 这个时候就可以直接在方法中使用变量啦！！！
  - 如果你给一个int的值赋值为空，首先服务器会先get到一个空的字符串，然后在parseInt()赋值给你的变量的时候，就会出问题了！！！这个时候状态码为400 ， 客户端错误，提交请求参数过程中发生了问题。后端代码中就不会报出异常的，spring只会自己默默的记下一个记录。
  - 说明前后端的类型对应还是很严格的，如果int接收不到结果，这个时候改成包装类型会好很多，Integer这个类型，如果转不成int的时候，Integer可以为null啊，这个就是包装类型的好处嗷！！！
  - 全部都用String，一点问题都没有，String没有任何问题！！！！但是可能需要你自己去判断数据的有效性，有点时候还不如直接出现问题，然后让Spring框架帮你解决，比如一个String赋值给int失败了，你的doSome方法根本不会执行啊，参数传不进去怎么执行？？？
  - get和post方法不一样可能会导致乱码，get没有问题，post方法解决乱码问题。



#### Post解决参数乱码：

- request.setCharacterEncoding("utf-8") ， 如果手写很麻烦，可以使用过滤器来实现重复的代码嗷！！！
- 框架中有提供过滤器嗷，直接用就好啦！！！
- CharacterEncodingFilter就是框架自带的一个过滤器，位置在web.xml文件中添加就好啦！！！

#### 过滤器配置设置：

- 在web.xml中导入系统默认提供的过滤器并进行初始化

```xml
	<filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!--设置项目中使用的字符编码-->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <!--强制请求对象，HttpServletRequest()使用encoding编码的值-->
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <!--强制请求对象，HttpServletResponse()使用encoding编码的值-->
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <!--
            这儿的/*表示强制所有的请求先经过过滤器处理
        -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

- 当然过滤器之前学过你也可以自己写一个过滤器嗷！！！

- SpringMVC中的MVC：

![image-20210516150949936](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210516150949936.png)

两种C的角色，前端控制器和后端控制器。



#### 参数名字对不上(@RequestParam)：

- 请求中的参数名和处理器方法的参数名不一样。
- @RequestParam，处理上面的问题，使用位置在处理器形参方法的前面。
- `@RequestParam(value="rname") String name`，意思是把rname这个属性的参数赋值给后面的name这个变量。
- RequestParam其实还有个属性叫做required，默认是true，表示请求中必须包含此参数。如果请求设置为false就没这个要求，就会比较宽松，赋值失败就是赋值为null





### 对象参数接收：

- 使用Java对象来接收对象
- 一样的，对象的属性要和表单传递的属性同名。
- Java自动调用无参构造赋值然后自动帮你调用set方法来对于属性
- 可以有多个对象，对象和单个参数混合使用都可以的嗷！！！
- 但是在这种情况下，RequestParam是不可以不可以不可以使用的嗷！！！



### List , Map , Array都支持

- 前端传参比较复杂，不经常使用而已，前端需要特殊的格式嗷！！！





## 处理器方法返回值：

### ModelAndView

- 需要数据又需要视图使用这种是最方便的，两种类型都可以返回。
- 只需要视图或者数据，这种方式就大意了，没必要哈，比如现在做前后端交互，就可以不使用这个。





### String

- 注意，仅仅用于页面的跳转，而数据没有改变或者传递嗷！！！

- 从a页面跳转到b页面，只有页面的跳转而没有数据的更改，这个时候就可以使用String
- 使用的话这个要用到逻辑视图名称，需要配置视图解析器。

![image-20210516212104596](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210516212104596.png)

- return的值就是另外一个页面的逻辑名称

![image-20210516212153420](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210516212153420.png)

- 其实也是可以加入数据的，只是不像使用model那么方便了，要手动添加类似于request这样的对象啊，设置全局作用域的变量和数据来进行赋值。
- 如果使用完整的视图路径，例如`/WEB-INF/view/show.jsp`，这个时候就不能配置解析器哇。
- 逻辑名称和完整名称这里是不可以混合使用的嗷！！！



### Void(Ajax)

- 不能表示数据，也不能表示视图
- 使用ajax的时候就可以用void啊，因为前端渲染，你不需要去返回视图，你把数据给前端就好了。
- 通过HttpServletResponse输出数据，相应ajax请求。
- ajax请求服务端返回的就是数据，与视图无关。
- 使用ajax的话，这里只讲后端嗷，需要用的依赖项：

![image-20210516213609180](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210516213609180.png)

- 前端使用ajax的一个例子（使用jquery）：

![image-20210516214136673](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210516214136673.png)

![image-20210516215249743](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210516215249743.png)

这里成功的话就可以从response中，根据属性名把对应的属性的值取出来。要注意哈，这里的dataType可以不用配置，这里的dataType默认就会来尝试使用json来解析哈。

- 后端使用ajax的一个例子（使用jackson）：

![image-20210516214500670](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210516214500670.png)

- 重点：
  - 调用service处理请求，将拿到的数据封装为对象，这里自己写，服务器帮你写其实都没问题嗷。
  - 转换对象为JSON格式。
  - 向response中输出数据，响应ajax的请求。
- 中间这部分和后面这部分大部分的重复，几乎不怎么需要去修改代码都可以操作起来让框架去写的。（1. java对象转换为json   2. 通过HttpServletResponse输出json数据）



### Object

- 表示的返回值是数据哈（对象类型的啥都行），ajax也可以使用这种哇，因为ajax也是表示数据呢。

- 对象有属性，属性就是数据，所以返回Object表示数据，和视图无关，可以使用对象表示的数据来响应ajax请求

- 现在做ajax，主要使用json数据格式：

  - 加入处理json工具库的依赖，springmvc默认使用jackson依赖。
  - springmvc配置文件之间加入\<mvc:annotation-driven\>，注解驱动，完成的功能就是将java对象转换为json对象。
  - 处理器方法上面加入@ResponseBody注解

- 上面三步就可以获得一个json格式的数据了，原理为：

  - annotation-driven , 注解驱动： 完成的功能是完成java对象到json,xml,text, 二进制等数据格式的转换。 底层实现了HttpMessageConverter接口，消息转换器，实现类完成了数据之间的转换
  - HttpMessageConvert代码中：
    - canWrite方法：检查处理器方法的返回值，能不能转为var2表示的数据类型
    - write方法：把处理器方法的返回值对象，调用jackson中的ObjectMapper转为json格式。
  - 加入了注解驱动之后，会自动创建7个实现类对象，实用的包括：
    - StringHttpMessageConverter
    - MappingJackson2HttpMessageConverter
  - @ResponseBody注解放在处理器上面，通过HttpServletResponse输出数据，响应ajax请求。

- 面试题：springmvc如何实现使得返回值是json，答案就是上面实现ajax的三步

- 实操：

  - ajax依赖添加：

    ```xml
    	<dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-core</artifactId>
          <version>2.9.0</version>
        </dependency>
    
        <dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-databind</artifactId>
          <version>2.9.0</version>
        </dependency>
    ```

  - springmvc中添加注解依赖，注意哈，可能有多个同名的，这个时候你要找名字中含有mvc的才是我们要用到的。

    ![image-20210517095004721](https://gitee.com/alexs-rabbit//picture2/raw/master/image-20210517095004721.png)

  - 前后状态变换：

    多了对应的jackson的库文件，所以要注意一下下哈，这里的jackson库文件处理json的就可以用了。

  - 后端实例：
  
  ```java
  //创建处理器对象，对象放在springmvc容器中
  @Controller
  @ResponseBody
  public class MyController {
  
      @RequestMapping(value="./returnStudent.do")
      public Student doJson(String name, Integer age){
          //调用service，获取请求处理的数据，Student表示结果数据
          Student stu = new Student();
          stu.setName("兔子姐姐");
          stu.setAge(20);
          return stu;
      }
  }
  ```
  
  - 这里上面只要标注上@ResponseBody
  - 框架就自动帮助你把数据转成了json格式并且放入response中把数据传回前端。



#### List集合返回多个对象

- Object的位置为List\<Student>

- 返回的是一个json数组，输出结果的顺序和你放入list中对象的顺序是一致的！
- 前端收到一个数组之后，应该通过类似于遍历这样的方式来对于json数组进行处理。类似于`$.each(response,function(){...})`，当然也可以用Map，但是Map不好排序，并且查值难，一般用list，list方便嗷！！！



### 返回值为String（深入讲解版）

- 处理器方法返回的string，string表示数据的，不是视图
- 区分返回值为string的是数据，还是视图，看前面有没有@ResponseBody注解，注意哈，返回的是对象的话会自动转为json，返回的是String的话返回的是文本。
- 后端给的String，前端用json解析，不出问题才怪orz。前端可以改为String来解析哇。
- 前端解析中文会乱发我吐了，咋整呢？

![image-20210517195317031](https://raw.githubusercontent.com/alexanderliu-creator/cloudimg/main/image-20210517195317031.png)



- RequestMapping后面加上produces属性

`@RequesetMapping(value="/returnStringData.do",produces="text/plain;charset=utf-8")`

注意哈，这里不走过滤器，这里仅仅就是返回了json，因此这里要用属性来对于数据格式进行处理。

- 默认使用的"text/plain;charset=ISO-8859"，所以这里要重新进行设置嗷，如果你想返回字符串给前端的话。





## 返回对象框架处理流程：

- @ResponseBody：
  - 作用：把处理器方法返回对象转成json后，通过HttpServletResponse输出给浏览器。
  - 位置：方法的定义上面，和其他注解没有顺序的关系。
- 处理流程：
  - 框架会把返回Student类型，调用框架中的ArrayList\<HttpMessageConverter>中每个类的canWrite()方法，检查哪个HttpMessageConverter接口中的实现类能处理Student类型的数据。最终就找到了MappingJackson2HttpMessageConverter。
  - 框架会调用实现类的write()，MappingJackson2HttpMessageConverter的write()方法，把李四同学的student对象转为json,调用Jackson的ObjectMapper实现转为json
  - 框架会调用ResponseBody把结果输出到浏览器，ajax请求完成。





















## url-pattern

### /的意义和用法

- 使用位置在于中央调度器，有不同的用法。
- 前面见过的*.do的方法，还有/的方法。
- 发起的请求的处理方式：

  - 请求页面的处理tomcat(jsp会转换为Tomcat)
  - js请求也是由Tomcat处理的
  - 静态照片的处理也是由Tomcat处理的
  - 以上，静态资源文件都是由Tomcat处理的
- some.do则是由SpringMVC框架处理的（DispatcherServlet）。
- Tomcat本身能够处理静态资源的访问，像Html，js , jsp啊之类的，Tomcat都能处理。本质上是Tomcat创建一个Servlet ,  名字叫做default，这个Servlet设置的startup也是1，在服务器刚启动的时候就创建了。
- 这个Default的Servlet处理的：

  - 静态资源

  - 未映射到其他Servlet的请求
- 这个东西在Tomcat中的配置文件中是能够看到的嗷！！！
- 在你的Servlet-mapping中如果没有找到，就交给default servlet来处理了嗷！！！

- 你发现default servlet对应的是 / ,说明 / 是能够干上面这两件事儿的。
- /用来表示静态资源和未映射的请求都给default来处理，当你的中央调度器中单独使用了 / 的时候，Tomcat默认的default就会被取代哦！！！会导致所有的资源都给中央调度器。默认情况下DispatcherServlet没有处理静态资源的能力。
- /会使得所有的资源都由中央调度器处理，中央调度器只负责只有处理动态资源的Controller，没有处理静态资源的嗷，那就很难受，FileNotFound。



### 处理静态资源访问的方案：

#### default-servlet-handler

- springmvc配置文件需要加入下述配置

- \<mvc: default-servlet-handler>
- 加入这个标签后，框架会创建对象DefaultServletHttpRequestHandler(类似于我们自己创建的 mycontroller)，DefaultServletHttpRequestHandler则会个对昂可以把接收的请求转发给tomcat的default的这个servlet。
- default-servlet-handler和@RequestMapping由冲突 ，需要加入annotation-driven来解决问题。
- 本质上（需要加入两个配置）：
  - \<mvc: default-servlet-handler>
  - \<mvc: annotation-driven>
- 原理：加入一个处理器对象，DefaultHttpRequestHandler，让这个对象处理静态资源的访问。



#### resources（掌握）

- \<mvc: resources />
- ResourceHttpRequestHandler
- 这个不用依靠服务器实现的default类，更加独立，所以更加好用。
- 加入后框架会创建ResourceHttpRequestHandler这个处理对象，让这个对象去处理静态资源的访问，不依赖Tomcat服务器
- 属性：
  - mapping：访问静态资源的uri地址
  - location：静态资源所在的目录位置
- e.g. `mapping="/images/**"`，表示的是images后面任意的文件（多级目录都可以），`location="/images/"`，表示WEB-INF下的images目录，意思就是你碰到images后面的资源的时候，去images目录下找。

![image-20210517194809464](https://raw.githubusercontent.com/alexanderliu-creator/cloudimg/main/image-20210517194809464.png)

- ![image-20210517165243055](C:/Users/Alexander/AppData/Roaming/Typora/typora-user-images/image-20210517165243055.png)
- resources标签又和RequestMapping冲突了，用了这些资源，annotation-driven是一定要加上的。



##### 一句话搞定:

- 把所有静态资源文件放在WEB-INF下一个叫做static的文件夹，在这个文件夹下面创建类似于html , js , image等文件夹再去放各种类型的静态文件。
- `<mvc:resources mapping="/static/** location="/static/" />`，这一个就可以了嗷，完成了多种静态资源的一句配置。



### 使用 / 的好处：

- 类似于网站的访问的时候，不需要.do啦，该什么地址就什么地址，直接改操作地址和监听地址为some就好啦。其实就感觉回到了一开始学servlet的时候orz
- 可能是为了我们能够将有后缀的资源名称和动态资源名称分开叭orz







## 加 / 和不加 /路径的区别：

- 地址分类：

  - 绝对地址：带有协议名称的是绝对地址
  - 相对地址：没有协议开头的，例如 user/some 和 /user/some都是的哦

- 参考地址：

  - 在index.jsp页面中发起user/some.do请求，你访问的是http://localhost:8080/myWeb/user/some.do。当你的地址没有/开头的时候，访问地址为当前页面地址+连接地址。地址可以拆分为：路径 + 资源。这里的资源为index.jsp，前面的都是跳转之前当前页面的地址嗷！！！

  (一个小例子)[https://raw.githubusercontent.com/alexanderliu-creator/cloudimg/main/image-20210517202614284.png]

  这个相当于把当前路径的资源名直接去掉，然后把href中的内容拼接到后面构成新的资源名字嗷！！！

  - 前端使用/user/some.do这个时候访问失败了！！！http://localhost:8080/user/some.do，少了一个myweb，这里的参考地址是你的服务器地址，所以出问题了嗷！这个时候访问地址就要改为/myWeb/user/some.do

- 使用EL表达式来完成路径的访问：

  - `<a href="${pageContext.request.contextPath}/user/some.do"></a>`，这样比较方便指定路径名嗷！！！
  - pageContext.request.contextPath，这个是自动生成的项目名称。

- 后端的资源要加上 /user/some.do，后端这个监听的(value="/user/some.do")，这个/表示的是项目的根。

  - 可能出现的问题就是反复访问index.jsp的时候，反复加上user/some.do , 又变为了user/user/some.do，就裂开了。所以user/some.do就会出现这种问题。

  - 解决方案：

    - ${pageContext.request.contextPath}
    - 加入一个base标签，是html语言中的标签，表示当前页面中访问地址的基地址。你的页面中所有没有以"/"开头的地址，都是以base标签中的地址为参考地址，使用base的地址 + user/some.do这样访问的。

  - 上面第二种的一个例子：

    - jsp页面中加上:

      `<base href="http://localhost:8080/myWeb/" />`

    - base相比一劳永逸，这个地址只用配置一次，不然${}可能写很多次

  - 第三种解决方案：

    - `String basePath=request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+request.getContextPath()+"/"`
    - 然后jsp中加入：\<base href="<%=basePath%>" />

    - 动态获取路径并且设置为base，就会很方便。

- 就有点类似于前端不加后端要加，但是前端还有讲究，讲究如上所述嗷！！！



# 下面好重要！！！



## SSM整合开发

- 三层对象：

  - SpringMVC：视图层，界面层，负责接收请求，显示处理结果

  - Spring：业务层，管理service , dao , 工具类对象的

  - MyBatis：持久层， 访问数据库的。

- 用户请求 --> SpringMVC接收 --> Spring中的Service对象 --> MyBatis处理数据，然后把流程反向再走一次就能够完成者一套的流程。（SSM也叫做SSI）

- 整合涉及到的是容器问题：

  - Spring容器 -》 管理Service , Dao , 工具类对象
  - SpringMVC容器 -》 管理Controller控制器对象的
  - 把合适的对象交给合适的容器来创建和管理。把Controller还有web开发的相关对象交给springmvc容器，这些web用的对象写在springmvc配置文件中。

- SpringMVC和Spring容器是有关系的，已经确定好了。mvc容器时spring容器的子容器，类似于java中的继承。子容器可以访问父容器的内容，因此Controller可以访问父容器中的对象，就可以实现controller使用service对象。

- 实现步骤：

  - 使用spring中的mysql库，表使用student
  - 加入内容：
    - Spring
    - SpringMVC
    - MyBatis
    - jackson
    - mysql驱动
    - druid连接池
    - jsp
    - servlet
  - 写web.xml：
    - 注册DispatcherServlet:
      - 创建SpringMVC容器对象
      - 创建Servlet，才能接受用户的请求
    - 注册spring的监听器（ContextLoaderListener）：
      - 创建spring的容器对象
      - 创建service , dao对象
    - 注册字符集过滤器：
      - 解决post乱发问题
    - 创建包：
      - Controller
      - Service
      - Dao
      - Entity
    - 写三大框架的配置文件：
      - mvc
      - spring
      - mybatis
      - 数据库的属性配置文件
    - 写代码 ， dao接口和mapper文件，service和实现类 ， controller , 实体类
    - 写jsp页面

### 依赖添加：

- pom.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.tuzi</groupId>
  <artifactId>ssm</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <!--servlet依赖-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
    </dependency>

    <!--jsp依赖-->
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.2.1-b03</version>
    </dependency>

    <!--springmvc依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <!--spring事务的依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <!--jdbc依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <!--jackson依赖-->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
    </dependency>

    <!--mybatis依赖-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.1</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.1</version>
    </dependency>

    <!--mysql依赖-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.23</version>
    </dependency>

    <!--alibaba的druid依赖-->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.12</version>
    </dependency>
  </dependencies>

  <build>
    <resources>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>
    </resources>

    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```



### web.xml内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

  <!--注册中央调度器-springmvc配置文件-->
  <servlet>
    <servlet-name>myweb</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:/conf/dispatcherServlet.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>myweb</servlet-name>
    <url-pattern>*.do</url-pattern>
  </servlet-mapping>

  <!--注册监听器-spring配置文件-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:conf/applicationContext.xml</param-value>
  </context-param>
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  
  <!--注册过滤器，解决post方式编码的问题-->
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>utf-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceRequestEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
    <init-param>
      <param-name>forceResponseEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>


</web-app>
```





### SpringMVC的配置文件：

- 用于声明Controller和其他web相关的对象

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--组件扫描器，声明controller和其他web的对象-->
    <context:component-scan base-package="com.tuzi.controller" />

    <!--配置前缀和后缀，这个是视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/" />
        <property name="suffix" value=".jsp" />
     </bean>

    <!--注解驱动-->
    <mvc:annotation-driven />
    <!--
        1. 相应ajax请求并且返回json
        2. 解决静态资源访问问题
    -->
</beans>
```





### Spring的配置文件：

- 用于声明service , dao , 工具类对象等

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--引入配置文件-->
    <context:property-placeholder location="classpath:/conf/jdbc.properties" />

    <!--spring配置文件-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
          init-method="init" destroy-method="close">
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.user}"></property>
        <property name="password" value="${jdbc.passwd}"></property>
    </bean>

    <!--sqlSessionFactory创建sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="configLocation" value="classpath:conf/mybatis.xml" />
    </bean>

    <!--声明mybatis扫描器，创建dao对象-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <property name="basePackage" value="com.tuzi.dao"></property>
    </bean>

    <!--声明service的注解@Service所在的包名的位置-->
    <context:component-scan base-package="com.tuzi.service" />

    <!--事务配置：注解配置，aspectj的配置-->
</beans>
```







### 包的创建：

- controller
- entity
- dao
- service





### mybatis主配置文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"></setting>
    </settings>

    <!--添加属性别名，这儿一般都是实体类型的包名-->
    <typeAliases>
        <package name="com.tuzi.entity"/>
    </typeAliases>
    
    <mappers>
        <!--
            name: 包名 ， 这个包中的所有的mapper.xml都能一次性加载
            使用package的要求：
                1. mapper文件名称和dao接口名称完全一样，包括大小写
                2. mapper文件和dao接口必须在统一目录
        -->
        <!--指定mapper的resource，这个地方可能需要修改嗷-->
        <package name="com.tuzi.dao"/>
    </mappers>
</configuration>
```

### 一些事项：

- 写配置文件的时候最好写一点，一点一点来测，不然很难debug。
- 浏览器中往往直接访问某个地址，来查看和验证接口是否构造正确。
- 路径错误非常常见，有的时候需要去解决发现哈，如果base不好用的话，就尝试去使用`${pageContext.request.contextPath}/user/some.do`这种形式来发起请求嗷！！！



- 后面的项目的代码和实例：[b站代码](https://www.bilibili.com/video/BV1sk4y167pD/?p=51&spm_id_from=pageDriver)
- 有可能会jquery找不到，试着去pom重新导入。



- 具体内容请查看你写的ssm代码嗷！！！

## 重定向和转发操作：

- forward表示转发，`request.getRequestDispatcher("xx.jsp").forwawrd(request,response)`
- redirect表示重定向，`response.sendRedirect("xxx.jsp")`
- 框架中简化了：
  - 使用forward和redirect直接表示转发和重定向。
  - 特点：不和视图解析器一块儿工作。
- 语法：
  - `setViewName("forward:视图文件完整路径")`
  - `setViewName("redirect:视图文件完整路径")`
  - 这里使用完整路劲哈，就当视图解析器不存在就好！！！
- 用于情况：有些页面不在视图解析器所解析的目录之下，forward指定全路径这个时候就比较好用啦！forward不受视图解析器的限制嗷！可以转发到视图解析器以外的页面。
- redirect有可能发生问题哈，浏览器向服务器发起了两次请求。框架提供了一个额外功能，框架会把Model中的简单类型数据转为String使用，然后让浏览器再次访问服务器的时候，携带这些数据。说白了就是你第一次设置的全局数据，浏览器会在第二次访问的时候用get方式来携带这些数据，以实现两次访问服务器的时候的数据共享。两次访问服务器会生成两个request，无法共享数据的，所以只能使用上述的方式。
- redirect可以利用这个特性，通过第二次访问的时候携带的参数，使用EL表达式中的`${param.myname}`来获取请求参数值嗷QAQ！！！
- redirect是不能够直接访问WEB-INF下受保护的资源的 ，本质上是让浏览器重新发送请求去访问WEB-INF下的资源，但是浏览器无权限。如果是forward的话，请求再服务端转发，服务端是有资格去访问WEB-INF下的资源的嗷！！！



# 异常处理

## 统一异常处理

- 为了解决try...catch...写的很多，修改不容易的特点
- 本质上使用aop的方式将异常处理与业务代码分离。
- 把controller中所有异常处理都集中到一个地方，解耦合。
- 使用注解：
  - @ExceptionHandler
  - @ControllerAdvice



### 实际操作

- 加入依赖
- 创建一个普通类作为全局异常处理类：
  - 类上面加上@ControllerAdvice
  - 类中定义方法，方法上面加入@ExceptionHandler
- Controller中抛出异常
- 创建处理异常的视图页面。
- 创建springmvc的配置文件：
  - 组件扫描器，扫描@Controller注解。
  - 组件扫描器，扫面@ControllerAdvice所在的包名。
  - 声明注解驱动。



#### @ControllerAdvice：

- 必须让框架知道这个注解所在的包名，需要在springmvc中配置组件扫描器包括这个包名
- 定义的处理类和Controller中定义的类是很类似的，返回值也可以是ModelAndView，参数可以是Exception，来接收异常并且处理异常。



#### @ExceptionHandler：

- 有一个参数是value，这个value是异常的类型，即发生此异常的时候采用当前方法来处理
- e.g. `value = NameException.class`
- 异常处理逻辑：
  - 记录异常到数据库和日志文件
  - 记录日志发生的事件，哪个方法发生的，异常错误内容
  - 发送通知，把异常的信息通知相关人员
  - 给用户友好的提示。
- 和Controller中的类一样，可以通过指定不同的value来处理不同的值。
- 以及提示一下，非常有可能出现其他异常，其他异常可以直接注解@ExceptionHandler，不用给value，就可以处理考虑情况意外的其他异常。这个处理异常的方法有且只能有一个。
- 导入对应的组件扫描器的时候还要记得同时要加上注解驱动，这个项目才跑的起来嗷！！！（注意有好多个注解驱动，要选mvc对应的那个）



### 兔兔总结

- Controller类中也可以抛出异常嗷，相当于业务逻辑非常清晰了，不用在业务代码中处理。最终一起在异常处理类中处理就好嗷！
- 注意哈：异常处理是对于所有异常采用一样的处理方式嗷，系统中全局管理异常嗷！！！全局类对于所有异常进行集中管理嗷，灵活度可能没那么高但是非常方便嗷！！！



## 拦截器

- 实现HandlerInterceptor都是拦截器
- 与过滤器类似，功能侧重点不同。过滤器是用于过滤请求参数，例如对于编码字符集进行处理。
- 拦截器则是全局的，主要是对于Controller进行拦截，做判断处理的。一个项目中可以有0个或者多个拦截器，一起拦截用户请求。
- 用于：
  - 用户登录处理
  - 权限检查
  - 记录日志
- 使用步骤：
  - 自定类实现HandlerInterceptor接口
  - 在springmvc配置文件中，声明拦截器，让框架知道拦截器的存在。
- 执行事件：
  - Controller方法执行之前
  - Controller方法执行之后
  - 请求处理完成后也会执行



### 单个拦截器例子：

- 创建拦截器类：
  - 实现HandlerInterceptor接口
  - 实现接口中的三个方法，可以不用全部重写哦，使用哪个写哪个就好啦！！！

#### preHandler

- 返回值是一个boolean：
- 特点：
  - 控制器方法之前先执行的。用户的请求首先到达此方法。
  - 可以验证用户是否登录，验证用户是否有权限访问某个连接地址。
  - 成功：
    - 可以放行请求
  - 失败：
    - 拦截请求，请求不能被处理



#### postHandler

- 被拦截的处理器对象Controller
- ModelAndView为处理方法的返回值
- 特点：
  - 处理器方法之后执行
  - 能够获取到处理器方法的返回值，可以修改ModelView中的数据和视图，影响到最后的执行结果
  - 主要对于原来的执行结果做二次修正。



#### afterHandler

- 返回值为void

- 请求方法处理完成后执行的，例如对于视图执行了forward，就认为请求处理完成。
- 一般做资源回收工作的，程序请求过程中创建了一些对象，在这里回收



```xml
<mvc:interceptors>
	<mvc:interceptor>
        <!--
		指定拦截的请求uri地址
		path：就是uri地址，可以使用通配符**
 		**：表示任意的字符，文件或多级目录和目录中的文件。
		/**：这个就代表所有请求，/代表请求的根
-->
        <mvc:mapping path="/user/**" />
        <bean class="com.tuzi.handler.MyInterceptor" />
    </mvc:interceptor>
</mvc:interceptors>
```

### 实际使用总结：

- 如果preHandler将请求拦截下来了，但是你要知道，这个方法的返回值只有true或者false。所以这个时候就可能要借用request.getRequestDispatcher().forward()这种方式来返回一个提示界面来完成交互。

- preHandle很重要，这是整个项目的入口，它控制请求是否能够被处理，返回假的话方法到此就截止了嗷！

- 就像是给它背后的controller增加了一个功能叭！！！拦截器可以看作多个Controller中共用的功能，用到的还是aop的思想，功能的集中处理嗷！！！

- 方法中有实际的判断逻辑，来判断能处理还是不能够处理，类似于Session和Cookie啊这种之类的。

- 甚至可以在类中定义公有变量，然后在preHandle中赋予事件，在postHandle中赋予时间，然后计算程序运行的时间嗷！！！

- postHandler的执行时间是在Handler执行完成之后，response还没返回之前，给了返回response之前最后的一次机会。

- 由于多个方法处于同一个类中，他们是可以共享一些数据和变量的，由此他们可以进行一些操作。

- 执行顺序：

  - preHandler() -> Handler -> postHandler() -> afterCompletion() 这样的流程嗷QAQ！ 

  - 流程图和概念：
    - ![image-20210521102438626](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210521102438.png)
    - ![image-20210521102519813](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210521102519.png)



### 多个拦截器例子：

#### 两个都为true

- 记得，有多个拦截器的时候，要在配置文件中声明哦。
- 框架中保存多个拦截用到的是ArrayList，先声明的先执行，后声明的后执行。
- 看起来感觉执行顺序有点乱？？？？
- ![image-20210521103045025](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210521103045.png)
- ![image-20210521103321593](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210521103321.png)



#### 第一个true，第二个false：

1preHandler() -> 2preHandler() -> 1afterCompletion



#### 两个都为false：

- 第二个为真或假对于整体没影响
- 请求到达1就结束了，就和之前一样，只会执行1preHandler()。
- 只要有一个preHandler返回false，请求到此结束，Handler都不会执行的嗷！！！
- 实践过程中，拦截器为模块，不同拦截器做的是不同的功能方向，因此会出现这种情况。



### 拦截器和过滤器的区别

1. 过滤器是Servlet中的对象，拦截器是框架中的对象。前一个范围更大，依赖更少。

2. 过滤器实现Filter接口，拦截器实现HandlerInterceptor

3. 过滤器用来设置request , response的参数，属性。侧重对于数据进行过滤。拦截器则是用来验证请求的，能够截断请求。
4. 过滤器在拦截器之前先执行的。
5. 过滤器是tomcat创建的对象，拦截器是springmvc创建的对象。
6. 过滤器就一个时间点，拦截器可以有三个时间点。
7. 过滤器可以处理jsp , js , html等等，拦截器是侧重对于Controller的对象。如果你的请求不能够被DispatcherServlet接收，这个请求不会被拦截哈。
8. 拦截器是拦截普通类方法执行，过滤器过滤请求Servlet请求响应。



### 登录系统小例子：

[动力节点教程](https://www.bilibili.com/video/BV1sk4y167pD?p=71&spm_id_from=pageDriver)



# 知识补充（MVC内部执行流程）：

- SpringMVC执行流程：

![image-20210521110508543](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210521110508.png)



## 映射器

- 以前使用对象的时候（Spring）中获取对象，使用getBean这种方式获取对象。但是在MVC框架中，框架都帮助你处理好了。

- 例如处理器映射器，MVC内部就存在了HandleMapping，自动帮助你getBean拿到处理器对象，存入到一个叫做HandlerExecutionChain的对象中。

- HandlerExecutionChain：这个类中保存着：

  - 处理器对象
  - 项目中所有的拦截器

- 中央调度器把2中的链交给了处理器适配器（实现了HandlerAdapter），本质上它执行处理器方法（调用Controller的方法并且得到结果ModelAndView，返回给中央调度器）

- 没有注解之前，需要实现不同的接口才能做控制器使用，现在注解@RequestMapping中应该实际使用的是RequestMappingHandlerMapping这个映射器。

- Dispatcher中的调用过程：

  `HandlerExecutionChain mappedHandler = getHandler(processedRequest);`



## 适配器

- 适配器本质上是用来执行处理器代码的。

- 映射器是用来找对象的，找你写好的RequestMapping。

- Dispatcher中的调用过程：

  `HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());`



## 视图解析器

- Dispatcher将获取的mv返回给视图解析器对象。
- 视图解析器：
  - mvc中的对象，需要实现ViewResolver接口（可以有多个）
  - 作用：组成视图的完整路径，使用前缀，后缀，并且创建View对象。
  - View是一个接口，表示视图的，在框架中使用jsp , html而不是string表示的。说白了就是将html这些用于展示视图的对象都实现了View这个接口而已。
- InternalResourceVIew：视图类，表示jsp文件的。是视图解析器创建的，它创建了InternalResourceResourceView类对象。这个对象里面，有一个属性url=/WEB-INF/view/show.jsp。
- 中央调度器将创建好的View对象获取到，调用View自己的方法，将Model数据放入request作用域，执行对象视图的forward，请求结束。



## 框架内优点：

- 各个模块儿通过DIspatcher打交道，你会发现，耦合度非常低，更新某些模块，其他模块是不用变的嗷！！！











# 一些小技巧

- `ctrl+alt+o`可以合理导入包嗷！
- `ctrl+h`可以查看某个接口的实现类

