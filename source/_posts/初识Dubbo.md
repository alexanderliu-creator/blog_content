---
title: 初识Dubbo
date: 2021-05-26 13:08:09
tags: 后端
categories: 动力结点后端
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721212039.png
---





# 从这里开始我们正式迈入互联网分布式相关的应用和开发，下面将先学习Dubbo，然后Spring Boot等，操作操作！！！

<!--more-->



## 分布式结构

- 若干独立系统的集合，但是用户使用起来像是在使用一套系统
- 原因：
  - 规模的逐步扩大和业务的复杂，单台计算机扛不住。
- CRM集中式开发则是性能不太好嗷！！！
- 架构演变：
  - 单一架构：
    - 开发部署简单
    - 拓展不容易，性能提升难
  - 垂直应用架构：
    - 将大应用拆分为小应用
    - 拓展容易
    - 易改，交互，相互调用问题
  - 分布式架构：
    - 业务拆分后，实现各个模块的远程调用和复用。
    - RPC: 远程过程调用。



## Dubbo概述

- 解决了分布式系统中相互调用的问题
- 注册中心：统一管理调度
- 网络进行传输：
  - 序列化
  - 网络通信
- 序列化才能网路通信，类似于xml , json , 二进制流都可以序列化。二进制流序列化效率最高，Dubbo采用的就是效率最高的二进制。
- 网络通信：
  - Socket通信机制。
  - 提升通信效率，不用反复连接，直接传数据。



### 前世今生：

- SpringCloud横空出世，Dubbo就更新了。。。
- SpringCloud还是要学的，这两个国内都用的多嗷！！！



# Apache Dubbo

- 面向接口的远程方法调用
- [Dubbo官网嗷！！！](https://dubbo.apache.org/zh/)

- Dubbo对应的图像如下：



![image-20210526184907666](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526184914.png)







## 直连方式解决架构：



### 服务提供者：

- Dubbo和Spring是完全直连的，因此要添加Spring的依赖。
- Dubbo依赖添加：
  - 主要是Spring依赖
  - 还有dubbo依赖

```xml
	<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>dubbo</artifactId>
      <version>2.6.2</version>
    </dependency>
```

- 创建了一个实体类user
- 创建了一个service包，包里面有一个queryUser的方法，这个方法的本质就是我们的图中的provider，方法的规范：

![image-20210526184948775](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526184948.png)

- 同时也要创建方法的实现类：

![image-20210526185229525](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526185229.png)

- 下面要想方法把我们定义的功能“暴露”出去：
  - 使用配置文件（resources中的spring类型的xml文件）来规定如何暴露嗷！！！
- 引入标签的时候别引入错了：

![image-20210526190036899](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526190237.png)

- 引入完成之后，我们的文件中的内容为：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!--服务的提供者必须声明并且保证唯一，它的名称是dubbo内部使用的唯一标识-->
    <dubbo:application name="001-link-userservice-provider" />

    <!--访问服务协议的名称以及对应端口号，dubbo官方推荐使用的dubbo协议，默认端口号为20880
        name为指定的名称
        port为指定的端口号（默认为20880）
    -->
    <dubbo:protocol name="dubbo" port="20880" />

    <!--
    暴露服务接口：
        dubbo:service
        interface：暴露服务接口的全限定类名
        ref：接口引用的实现类在spring容器中的标识
    -->
    <dubbo:service interface="com.tuzi.service.UserService" ref="userService" registry="N/A"/>

    <!--
        由于这里需要用的接口的实现类
        因此我们这里还需要使用正常的标签来加载spring
    -->
    <bean id="userService" class="com.tuzi.service.impl.UserServiceImpl" />
</beans>
```

- dubbo内的数据设置成功后，我们在web.xml配置文件中，加入相关的内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:dubbo-userservice-provider.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
</web-app>
```

- 本质上对应了图中的某一步：

![image-20210526203537822](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526203537.png)

通过Spring这个Container来生成Provider也就是服务的提供者嗷！！！

- 服务的提供者的创建流程：

![image-20210526203657797](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526203657.png)





### 服务消费者：

- 本质上也是一个module，一个单独的东西和项目。
- 由于consumer需要知道provider提供的接口和接口中的方法，因此我们这儿要导入provider
- 导入provider的方法是将provider打成jar包，然后在consumer中引入这个jar包。
- 打包provider为jar包的方法为：

![image-20210526205711259](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526205711.png)

- 上面写错了，这里打包成war就有点顶了，注释掉默认就是打包成jar包了。右边点击Maven中的install就成功将这个项目打成了jar包嗷！！！
- 这儿就是导入了相应的包：

![image-20210526210311402](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526210311.png)

- 这里引入了对应的打好的包嗷！！！

![image-20210526210505121](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526210505.png)

-  上面这里可以看到local repository，其实可以在里面看到我们的com.tuzi里面的内容嗷！！！就知道在哪里了嘛！
- pom.xml完整配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.tuzi</groupId>
  <artifactId>link-consumer</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <!--spring依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <!--dubbo依赖-->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>dubbo</artifactId>
      <version>2.6.2</version>
    </dependency>

    <!--如果要调用接口，那么我们必须知道它有哪些接口哇,依赖于provider的Jar包-->
    <dependency>
      <groupId>com.tuzi</groupId>
      <artifactId>link</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
  </dependencies>

  <build>

  </build>
</project>
```

- 下面编写consumer的核心配置文件（也就是spring文件）：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!--声明服务消费者的名称：保证唯一性-->
    <dubbo:application name="link-consumer" />

    <!--
        这里是在consumer中引用provider对应的内容嗷
        引用远程服务接口：
            id: 远程服务接口的名称
            interface: 调用远程接口的全限定类名
            url: 访问服务接口的地址
            registry: 不使用服务中心，值为:N/A
    -->
    <dubbo:reference id="userService" interface="com.tuzi.service.UserService" url="dubbo://localhost:20880" registry="N/A"/>
</beans>
```

- 接着配置springmvc的配置文件，里面的内容为：

```java
package com.tuzi.web;

import com.tuzi.model.User;
import com.tuzi.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class UserController {

    @Autowired
    private UserService userService;

    @RequestMapping(value = "/user")
    public String userDetail(Model model, Integer id){
        User user = userService.queryUserById(id);
        model.addAttribute("user",user);
        return "userDetail";
    }
}
```

- 定义mvc中的中央调度器dispatcherServlet

- web.xml中的内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:application.xml,classpath:dubbo-consumer.xml</param-value>
        </init-param>
    </servlet>

    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

- 添加userDetail这个jsp , 内容如下：

```xml
<%--
  Created by IntelliJ IDEA.
  User: Alexander Liu
  Date: 2021/5/27
  Time: 9:09
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>用户详情</title>
</head>
<body>
<h1>用户详情</h1>
<div>
    用户标识:${user.id}
</div>
<div>
    用户名称:${user.username}
</div>
<div>
    用户年龄:${user.age}
</div>
</body>
</html>
```













# 课程到这里问题很多，老师讲的也不是很好，暂缓学习。





























