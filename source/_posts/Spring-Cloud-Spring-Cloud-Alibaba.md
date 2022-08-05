---
title: Spring Cloud + Spring Cloud Alibaba
date: 2021-07-24 16:23:19
tags: 后端
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724162539.png
---



# 这是继Boot之后，对于SpringCloud+Alibaba分布式框架的学习



# Cloud概述

- 什么是微服务：
  - Martin Fowler
- 分布式架构强就强在一个整体的性能特别棒嗷！！！

![image-20210724164643053](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724164643.png)

SpringCloud = 分布式微服务架构的一站式解决方案，是多种微服务架构落地技术的集合体，俗称为微服务全家桶。

- 优质项目推荐：

![image-20210724165218435](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724165218.png)

- Cloud和Boot的选型：

  - Boot2.X版和Cloud H版
  - Boot和Cloud的版本选择是有约束，有依赖的。在官网中的Cloud的Overview中，是有对应的说明的嗷！！！

## 版本选择

  ![image-20210724170443790](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724170443.png)

  - [更加详细的版本说明](https://start.spring.io/actuator/info)

  ![image-20210724171310522](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724171310.png)

  - 具体最好的推荐版本，进入你要使用的Cloud的参考文档中，都有对应的说明的哈！！！

  ![image-20210724172343094](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724172343.png)

- 这里我们使用和网课一样的框架嗷！！！方便后面的学习和深入：

![image-20210724185921087](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724185921.png)

  

## 技术迭代：

![image-20210724185450373](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724185450.png)

- 可以看出来，Nacos是新的技术中的重中之重。许多老的技术已经被新的技术淘汰掉了。







# 支付模块微服务实操

## 父工程Project配置：

- 环境部分：

  - 约定 > 配置 > 编码
  - IDEA中的创建流程：
    - Project：
      - Module1 - 例如：订单模块
      - Module2 - 例如：支付模块
      - ...
  - 创建过程：

  ![image-20210724190959986](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724191000.png)



- Project的创建：

![image-20210724191213484](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724191213.png)



- 字符编码：

![image-20210724194208127](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724194208.png)

- 支持注解设置：

![image-20210724194621731](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724194621.png)

- Java编译版本设置：

![image-20210724194852269](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724194852.png)

- FIle Type过滤：
  - 比如不显示iml文件等，这个是可以配置的嗷！！！

![image-20210724195142387](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724195142.png)



## 父工程POM：

- 指明为POM

![image-20210724195343393](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724195343.png)

- 删掉src，只做版本管理的话不用src文件夹嗷！！！
- POM中做好版本依赖管理：

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.atguigu.springcloud</groupId>
    <artifactId>cloud</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <name>Maven</name>
    <!-- FIXME change it to the project's website -->
    <url>http://maven.apache.org/</url>
    <inceptionYear>2001</inceptionYear>


    <distributionManagement>
        <site>
            <id>website</id>
            <url>scp://webhost.company.com/www/website</url>
        </site>
    </distributionManagement>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>12</maven.compiler.source>
        <maven.compiler.target>12</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <lombok.version>1.18.10</lombok.version>
        <log4j.version>1.2.17</log4j.version>
        <mysql.version>8.0.18</mysql.version>
        <druid.version>1.1.16</druid.version>
        <mybatis.spring.boot.version>2.1.1</mybatis.spring.boot.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-project-info-reports-plugin</artifactId>
                <version>3.0.0</version>
            </dependency>
            <!--spring boot 2.2.2-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.2.2.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud Hoxton.SR1-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.1.0.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--mysql-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
                <scope>runtime</scope>
            </dependency>
            <!-- druid-->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.spring.boot.version}</version>
            </dependency>
            <!--junit-->
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
            </dependency>
            <!--log4j-->
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>


    <build>
        <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
            <plugins>
                <plugin>
                    <artifactId>maven-clean-plugin</artifactId>
                    <version>3.1.0</version>
                </plugin>
                <plugin>
                    <artifactId>maven-site-plugin</artifactId>
                    <version>3.7.1</version>
                </plugin>
                <plugin>
                    <artifactId>maven-project-info-reports-plugin</artifactId>
                    <version>3.0.0</version>
                </plugin>
            </plugins>
        </pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-site-plugin</artifactId>
                <configuration>
                    <locales>en,fr</locales>
                </configuration>
            </plugin>
        </plugins>
    </build>

    <reporting>
        <plugins>
            <plugin>
                <artifactId>maven-project-info-reports-plugin</artifactId>
            </plugin>
        </plugins>
    </reporting>
</project>

```

上面是pom中的代码

- dependencyManagement和dependencies的区别：
  - 第一个能让所有子项目中引入一个依赖而不需要显示给出版本号，Maven会自动沿着父子层次往上走，知道找到一个拥有此元素的项目，它就会使用这个dependencyManagement元素中指定的版本号嗷！！！
  - 第一个只是声明依赖，并不实现引入嗷，因此子项目需要显示的声明需要的依赖，version和scope都会继承父工程的版本号嗷！！！
- 禁用test可以节约时间嗷！！！

![image-20210724204148101](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724204148.png)

- clean + install可以将我们自己的项目打包安装到我们的Maven仓库中嗷！！！发布到仓库中之后才方便子工程继承嗷！！！



## 支付模块构建：

### 最简单的提供服务功能(Provider)：

- 始终：约定 > 配置 > 编码。最简单的功能的约定如下所示：

![image-20210724204456499](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724204456.png)

- 如何构建微服务模块：
  1. 建module
  2. 改pom
  3. 写yml
  4. 主启动
  5. 业务类
- 建module：

![image-20210724204819838](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724204819.png)

- 改pom：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud</artifactId>
        <groupId>com.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8001</artifactId>

    <dependencies>
<!--        坐标监控的时候十分重要，这两者一般都是一起出现的嗷！！！-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

<!--        下面是数据库-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>

<!--        mysql-connector-java-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

<!--        jdbc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

- 写yml：
  - 最少都要配置端口号和服务名称嗷！！！

```yaml
server:
  port: 8081

spring:
  application:
    name: cloud-payment-service
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/cloud-payment?serverTimezone=GMT
    username: root
    password: ******

mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.tuzi.cloud.entities
```

- 主启动：

![image-20210725094338249](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725094345.png)

- 业务类：

![image-20210725094759209](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725094759.png)

1. 建表SQL：

```sql
CREATE TABLE `payment`(  
    `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT 'ID',
    `serial` VARCHAR(200) DEFAULT "",
    PRIMARY KEY(`id`)
)
```




2. entities实体类：

![image-20210725101756502](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725101756.png)

```java
package com.tuzi.cloud.entities;


import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Payment implements Serializable
{
    private long id;
    private String serial;
}
```

这里注意一下，BIGINT和这里的Long是对应的。要实现**序列化**，这样后面才能做分布式部署嗷！！！

```java
package com.tuzi.cloud.entities;


import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T>
{
    //404 not_found
    private Integer code;
    private String message;
    private T data;

    public CommonResult(Integer code,String message){
        this(code,message,null);
    }
}
```

CommonResult是用来传递返回的结果(JSON)给前端的时候用的嗷！！！所有还用到了泛型，用于接受不同类型的参数，并且同一返回嗷！！！。



3. dao层：

```java
package com.tuzi.cloud.dao;

import com.tuzi.cloud.entities.Payment;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;

@Mapper
public interface PaymentDao
{
    public int create(Payment payment);

    public Payment getPaymentById(@Param("id") long id);
}
```

持久层一般都是接口，而且使用@Mapper注解就足够用了嗷！！！

- 定义完接口之后，我们还要写对应的实现xml嗷！！！

![image-20210725102607967](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725102608.png)

![image-20210725103726002](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725103726.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.tuzi.cloud.dao.PaymentDao">

    <insert id="create" parameterType="Payment" useGeneratedKeys="true" keyProperty="id">
        insert into payment(serial) values(#{serial})
    </insert>
    
    <resultMap id="BaseResultMap" type="com.tuzi.cloud.entities.Payment">
        <id column="id" property="id" jdbcType="BIGINT" />
        <id column="serial" property="serial" jdbcType="VARCHAR" />
    </resultMap>
    <select id="getPaymentById" resultMap="BaseResultMap" parameterType="long">
        select * from payment where id=#{id}
    </select>
</mapper>
```

- 首先，插件非常方便，只要绑定了NameSpace之后，插件就会自动给没有映射的方法爆红，然后提示帮你在xml中生成对应的方法。
- 推荐使用resultMap作为映射，当表的数据很多，而且命名不规范的时候，ResultMap能够很好的规定我们的数据元素和数据库中的元素之间的映射关系嗷！！！



4. service接口：

![image-20210725105342521](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725105342.png)

- Service理论上是实现业务逻辑，并且调用dao层的，我们这里直接将service接口中的方法设置为和Dao层中一致，可以少些代码嗷！！！

```java
package com.tuzi.cloud.service;


import com.tuzi.cloud.entities.Payment;
import org.apache.ibatis.annotations.Param;

public interface PaymentService {
    public int create(Payment payment);

    public Payment getPaymentById(@Param("id") long id);
}
```

- 有了接口也要熟练去写实现类嗷！！！

```java
package com.tuzi.cloud.service.impl;

import com.tuzi.cloud.dao.PaymentDao;
import com.tuzi.cloud.entities.Payment;
import com.tuzi.cloud.service.PaymentService;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

@Service
public class PaymentServiceImpl implements PaymentService
{
    @Resource
    private PaymentDao paymentDao;

    @Override
    public int create(Payment payment) {
        return paymentDao.create(payment);
    }

    @Override
    public Payment getPaymentById(long id) {
        return paymentDao.getPaymentById(id);
    }
}
```

这里注意一下，Impl肌肉记忆去写@Service，这里的@Resource注解的作用和@Autowired一样，起到了自动注入的作用嗷！！！这个类的目的就是根据我们规定的方法，调用我们的Dao层，完成功能的具体实现。最终需要将这个实现类加入到容器中，这样才能供我们后续使用嗷！！！



5. controller

- Controller -> Service -> Dao

```java
package com.tuzi.cloud.controller;

import com.tuzi.cloud.entities.CommonResult;
import com.tuzi.cloud.entities.Payment;
import com.tuzi.cloud.service.PaymentService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController
@Slf4j
public class PaymentController {

    @Resource
    private PaymentService paymentService;

    //写操作用Post
    @PostMapping(value = "/payment/create")
    public CommonResult create(Payment payment){
        int result = paymentService.create(payment);
        log.info("*****插入结果为："+result);

        if(result > 0){
            return new CommonResult(200,"插入数据库成功",result);
        }else {
            return new CommonResult(444,"插入数据库失败",null);
        }
    }

    //读操作用Get
    @GetMapping(value = "/payment/get/{id}")
    public CommonResult getPaymentById(@PathVariable("id") long id){
        Payment payment = paymentService.getPaymentById(id);
        log.info("*****查询结果为："+payment);

        if(payment != null){
            return new CommonResult(200,"查询数据库成功",payment);
        }else {
            return new CommonResult(444,"没有对应记录，查询ID: "+id,null);
        }
    }
}
```

- 写操作一般通过表单来操作的，所有用Post，查询操作一般Get就可以，CommonResult相当于返回JSON的一个同一的数据模板，我们封装之后就可以把它返回了。
- 注意，Controller能够调用的，只有Service，它是不能够调用Dao的嗷！！！





- 下面要对于我们编写的内容进行测试：
  - 遇到的坑：有些地方，例如project structure里面，项目默认的编译版本为Java12, 会报错嗷！！！
  - 对于Post请求做测试，在我们没有写前端的Post表单的情况下，我们可以直接使用POSTMAN对于Post请求做测试嗷，这样很方便嗷！！！
  - 自测通过才行嗷！！！
  - ![image-20210725112625240](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725112625.png)
- 小总结：就是上面那五点就可以实现后端的功能嗷！！！







### 最简单的提供服务功能(Consumer)：

- 前面微服务的流程：新建module , 修改pom文件 ， application.yml ， 启动类的配置和之前都差不多，不再赘述，值得一提的是这里的application.yml中的配置为：

```yaml
server:
  port: 80
```

默认为80，因为80为默认的网络接口，所以我们这里必须设置为80端口嗷！！！

- 下面这里介绍的就是Consumer的业务类：

![image-20210725161657175](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725161657.png)

Consumer只需要去调用Provider提供的服务，为啥要Dao 和 Service呢？？？

- 实体类还是要的嗷，这里我们直接拷贝Provider中的实体类到Consumer中嗷！！！
- 编写我们的Controller：
  - Controller的存在就是为了调用Provider提供的服务嗷！！！
  - 使用RestTemplate可以完成接口的调用，使用方法如下：
  - ![image-20210725161756124](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725161756.png)
- 由于需要使用restTemplate，我们需要编写对应的配置类将其注入到我们的容器中来嗷！！！（要用的东西都可以通过@Configuration配置类中的@Bean来添加到容器中来嗷！！！）

![image-20210725162128770](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725162128.png)



- 两个都可以跑起来，测试结果如下：

![image-20210725164203858](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725164203.png)

![image-20210725164220164](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725164220.png)



- 注意点，编译的时候的版本很阴间，总是用的是12，为了解决这个问题，我们在父工程中引入了Maven编译和运行的插件来限定语言编译的级别和版本为8：

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <configuration>
        <source>1.8</source>
        <target>1.8</target>
    </configuration>
</plugin>
```



### 测试中消费者插入功能失败：

![image-20210725171613632](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725171613.png)

![image-20210725171559335](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725171621.png)

- 上传成功数据库中却没有？？？
- 因为少了@RequestBody注解嗷！！！客户端封装好的payment对象处于请求体中，再次调用服务端的功能，这个时候服务端就不是和以前一样自己从请求头中封装数据了，而是需要从请求体中获取数据，因此需要在参数前加上@RequestBody！！！好大的天坑呜呜呜。
- service（老版本的run dashboard出不来咋办？）![image-20210725172442474](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725172442.png)



## 工程重构：

![image-20210725182619979](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725182620.png)

- 以上两个子工程的entities文件夹是一样哒！！！有没有什么办法将他们合并呢？ ---- 工程重构（减少重复代码）
- 新建一个工程：cloud-api-commons

pom中多引入一个东西：

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>5.1.0</version>
</dependency>
```

- 将相同的entities放在cloud-api-commons的相同位置上。

- 做好了cloud-api-commons之后，clean并且install安装到仓库中去，供其他两个工程调用。

- 改造80和8001：

  - 删掉entities
  - 引入我们自定义的jar包：

  ```xml
  <dependency>
      <groupId>com.atguigu.springcloud</groupId>
      <artifactId>cloud-api-commons</artifactId>
      <version>1.0-SNAPSHOT</version>
  </dependency>
  ```

- 引入了上面的依赖之后，IDEA不再爆红，证明依赖引入成功。







# 热部署Devtools：

![image-20210725142522278](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725142522.png)



- devtools依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```

- 加入父类的plugin

![image-20210725143413525](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725143413.png)

- ADBC打勾：

![image-20210725144738950](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725144740.png)



- resgistry的设置：

![image-20210725144846898](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725144846.png)

registry中对应位置打勾设置：

![image-20210725145159899](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725145312.png)

![image-20210725145255410](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725145255.png)

- 设置完成之后，重启IDEA即可嗷！！！
  - 效果好像不是特别好，老方法`Ctrl + F9`也可以使用嘛！！！
- 只能在开发的环境中使用，生产环境中不能够使用嗷！！！







# Eureka技术：

- 虽然停更了，但是思想还是十分重要的嗷！！！

- 概念：
  - 服务治理，管理服务域服务之间依赖关系
  - 实现服务调用，负载均衡，容错等
- 服务架构：

![image-20210725210150177](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725210150.png)

Server提供服务注册服务，Client会通过注册中心进行访问。

![image-20210725210810903](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725210810.png)



## 将Eureka整合进支付模块中：

- 不同版本：

![image-20210725212348036](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725212348.png)

- 还是那几个步骤：
  - 新建module
  - 改pom
  - application.yml
  - 主启动
  - 业务类
- pom：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud</artifactId>
        <groupId>com.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-eureka-server7001</artifactId>

    <dependencies>
        <!--        坐标监控的时候十分重要，这两者一般都是一起出现的嗷！！！-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

<!--        eureka相关配置-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
    </dependencies>

</project>
```

- application.yml：

```yaml
server:
  port: 7001
eureka:
  instance:
    hostname: localhost #eureka服务端的实例名称
  client:
    register-with-eureka: false
    #false表示不向服务中心注册自己
    fetch-registry: false
    #false表示自己就是注册中心，负责维护服务实例，并不需要去检索服务
    service-url:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
      #设置域Eureka Server交互的地址查询和注册服务都需要依赖这个地址
```

- 主启动：

```java
package com.tuzi.cloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class EurekaMain7001 {
  public static void main(String[] args) {
    //这是注册中心的主启动类
      SpringApplication.run(EurekaMain7001.class,args);
  }
}
```

启动测试：

![image-20210725214319330](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725214319.png)

- 将支付功能入驻进Eureka：

  - 引入客户端的依赖：

  ```xml
  <!--        eureka相关配置-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
  </dependency>
  ```

  - 改配置：

  ```yaml
  spring:
    application:
      name: cloud-payment-service
  
  eureka:
    client:
      # 表示自己注册为Eureka
      register-with-eureka: true
      # 是否从EurekaServer中抓取出已有的注册信息，默认为true，单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
      fetch-registry: true
      service-url:
        defaultZone: http://localhost:7001/eureka
  ```

  - 主启动：

  加上`@EnableEurekaClient`注解

  - 再次查看页面：

  ![image-20210725220616118](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210725220616.png)

- 添加order模块是一样的嗷！！！只是配置上有一点不一样：

```yaml
spring:
  application:
    name: cloud-order-service
    
eureka:
  client:
    # 表示自己注册为Eureka
    register-with-eureka: true
    # 是否从EurekaServer中抓取出已有的注册信息，默认为true，单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:7001/eureka
```

![image-20210726110810875](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726110811.png)

如果registry-with-eureka改为false，即为取消注册到Eureka。





## Eureka集群部署：

- 集群原理：

![image-20210726111814974](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726111815.png)

![image-20210726112206889](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726112206.png)



- 一个哥们要注册集群中其他所有的哥们

### 集群实操：

- 新建一个cloud-eureka-server7002的module

- pom依赖是完全一样的

- application.yml中实现相互注册，互相守望：

  - 由于本机只有一个localhost，为了模拟集群的效果，我们要修改对应的配置文件：

  ![image-20210726114945368](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726114945.png)

  修改之后方便模拟两个不同的主机嗷！！！

  - 相互注册，互相守望：

  ![image-20210726115338597](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726115338.png)

  ![image-20210726115400473](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726115400.png)

  - 效果：

  ![image-20210726132634280](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726132634.png)

  

  

  

## 发布我们的服务到集群中：

- 代码其实不用修改，关键是要调整我们的Yaml文件
- 配置集群版：

```yaml
defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
```

- 我们需要把服务部署到集群中去嗷！！！
- 测试流程：

![image-20210726151832578](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726151832.png)



## 支付服务提供者8001集群环境构建：

- 建立module（名字为8002）
- 修改pom（和8001POM中依赖一致即可）
- 写YML
- 主启动和业务类都是一样的嗷！！！
- 修改Controller：
  - 注意哈，这里对外提供的服务的名字都是一样的哈，因为yml是差不多的嘛（除了端口）
  
  - 这个时候就引入了负载均衡的概念：
  
    - 首先先要加入对应的server.port的打印：
  
    ![image-20210726151736623](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726151743.png)
  
    - 改order的原程序，不能够只访问8001端口哇，不然就没意义了，要访问集群哇！！！
  
    ```java
    //    public static final String PAYMENT_URL = "http://localhost:8001";
    public static final String PAYMENT_URL = "http://CLOUD-PAYMENT-SERVICE";
    ```
  
    改完还是错的，因为在这里修改了之后，默认还是调用的主机，然后这个主机和我们的eureka根本联系补上，就比较痛苦。
  
    - 为了让二者建立联系，我们需要配置负载均衡注解。(在Order80文件中)：
  
    ```java
    @Configuration
    public class ApplicationContextConfig {
        @Bean
        @LoadBalanced
        public RestTemplate getRestTemplate(){
            return new RestTemplate();
        }
    }
    ```
  
  - 配置完成之后，我们发现测试中，8001和8002可以交替提供负载，即负载分配成功嗷！！！
  
  - summary：
  
    - 相当于将具体的ip地址抽象为了提供的服务名，而具体由哪台机子来提供服务，则是由eureka动态负载分配来决定的嗷！！！
    - 只要将eureka集群搭建好，将对应的服务端和客户端都注册好，就能够自动实现以上的功能嗷！！！



## 微服务信息完善：

### 主机名称：服务名称的修改：

```yaml
instance:
  instance-id: payment8001
```

### 访问信息有IP信息显示：

```yaml
instance:
  instance-id: payment8001
  prefer-ip-address: true
```

- 方便我们线上的debug，找到对应的主机和其端口嗷！！！





## 服务发现（Discovery）：

- 本质就是一个注解标签：`@EnableDiscovery`

![image-20210726202905914](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726202906.png)

- 自动注入discoveryClient：

![image-20210726204241115](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726204248.png)

- 对于注入的discoveryClient的一些操作：

```java
@GetMapping(value = "/payment/discovery")
public Object discovery(){
    List<String> services = discoveryClient.getServices();
    for(String element: services){
        log.info("*****element: "+element);
    }

    List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
    for (ServiceInstance instance:instances){
        log.info(instance.getServiceId()+"\t"+instance.getHost()+"\t"+instance.getPort()+"\t"+instance.getUri());
    }

    return this.discoveryClient;
}
```

- 主启动类多一个注解：

```java
@SpringBootApplication
@EnableEurekaClient
@EnableDiscoveryClient
public class PaymentMain8001
{
    ...
}
```

- 对于功能进行测试：

![image-20210726204616698](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726204616.png)



- 运行测试结果：

![image-20210726205047066](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726205047.png)

之所以先启动Eureka，是因为我们的Payment有一个注册的操作，因此有先后的启动顺序嗷！！！



## 自我保护机制：

![image-20210726205240957](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726205241.png)

- 一句话：某时刻某一个微服务不可用了，Eureka不会立即清理，依旧会对于该微服务的信息进行保护。CAP理论里面的AP思想。（高可用的设计思想）

![image-20210726205645312](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726205645.png)

![image-20210726210112686](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726210112.png)

宁可保留错误的服务注册信息，也不盲目注销任何可能健康的实例。



### 关闭自我保护：

- 首先，这个实验比较麻烦，需要将集群改为单个点。将8001和7001配对使用。（切换回单例模式）

7001：

![image-20210726214310817](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726214310.png)

8001：

![image-20210726214455814](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726214456.png)

- ![image-20210726212728103](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726212728.png)
- 关闭自我保护模式需要同时修改eureka的server和client嗷！！！

对于eureka服务端的添加：

- ![image-20210726212932136](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726212932.png)



对于提供服务的客户端的添加：

- ![image-20210726213615865](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726213623.png)

- 测试流程和结果：

![image-20210726214117931](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726214118.png)





# Zookeeper技术：

![image-20210726220132822](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726220132.png)

无非就是将eureka换成了zookeeper而已哦！！！



## 注册中心Zookeeper：

![image-20210726220251839](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726220251.png)

- zookeeper部署在3344端口上嗷！！！





## 服务提供者：

![image-20210726220819182](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210726220819.png)

- zookeeper在linux上需要安装（配置linux服务器）：
  - 阿sir，Linux上安装还有配置防火墙啊，下载对应的Zookeeper啊（配置ZooKeeper），有点乱啊orz。
  - docker就是yyds，[在自己的云服务上使用docker来安装并启用zookeeper](https://www.cnblogs.com/caoweixiong/p/12325410.html)。



- 配置过程：

1. 新建module

2. pom：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud</artifactId>
        <groupId>com.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8004</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atguigu.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

<!--        boot整合zookeeper客户端-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>

</project>
```

3. 主配置：

```yaml
server:
  port: 8004

spring:
  application:
    name: cloud-provider-payment
  cloud:
    zookeeper:
      connect-string: xxx.xxx.xxx.xxx:2181
```

下面这个就是提供zookeeper服务的端口号

4. 主启动：

```java
package com.tuzi.cloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain8004
{
  public static void main(String[] args) {
    //
      SpringApplication.run(PaymentMain8004.class,args);
  }
}
```

- 不用eureka的话，就只用上面这两个注解就行，后面和Boot应用打交道一般都是用下面这个@EnableDiscoveryClient

5. 业务类：

- Controller：

```java
package com.tuzi.cloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain8004
{
  public static void main(String[] args) {
    //这个就是主类
      SpringApplication.run(PaymentMain8004.class,args);
  }
}
```

如果注册zookeeper不成功会报错的嗷，如果没有报错说明没啥大问题嗷，注意，教学视频中可能出现我们cloud中的jar包，不符合服务器的版本。由于docker拉取的是最新的zookeeper，我这里并没有报错嗷！！！

![image-20210727175612005](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210727175612.png)

- Jar包如果出了问题的话，就把zookeeper中默认包含的Jar给exclude排除掉，然后再引入我们需要的对应版本的jar包就可以了嗷！！！

- 这里回想当时学狂神的docker内容，如何在我们和docker正确建立联系之后，在docker中查询到这个节点的信息，还需要我们进一步的复习和学习嗷！！！



## 临时还是永久节点：

- 零时的嗷！！！
- 我们发现重新启动之后，又注册上了嗷！！！硬汉风格，我检测到你我就加你，我检测不到你的心跳我直接删了你嗷！！！



## 服务消费者：

1. 建立module
2. 修改pom：参加上面8004服务提供者的pom。

3. application.yml：参加上面8004服务提供者的application.yml。

```yaml
server:
  port: 80

spring:
  application:
    name: cloud-consumer-order
  cloud:
    zookeeper:
      connect-string: xxx.xxx.xxx.xxx:2181
```

4. 主启动类

5. 业务类：

加入对应的配置：

```java
@Configuration
public class ApplicationContextConfig {
    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

加入RestTemplate，方便调用。



主启动类：

```java
@RestController
@Slf4j
public class OrderZKController {
    public static final String INVOKE_URL = "http://cloud-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping(value = "/consumer/payment/zk")
    public String paymentInfo(){
        String result = restTemplate.getForObject(INVOKE_URL+"/payment/zk",String.class);
        return result;
    }
}
```

上面是对于Controller的配置。



# Consul技术：

- 简介：
  - go语言开发的
  - ![image-20210804195645580](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210804195652.png)
  - [Consul的中文文档](https://www.springcloud.cc/spring-cloud-consul.html)
  - 去Consul官方网文档中下载Consul。
- Consul下载文件夹中对应exe文件所在的目录：

```shell
# 使用consul --version来查看版本
Consul v1.10.1
Revision db839f18b
Protocol 2 spoken by default, understands 2 to 3 (agent will automatically use protocol >2 when speaking to compatible agents)
```



## 安装并运行Consul：

- `consul agent -dev`可以启动consul
- 启动了consul之后，在8500端口就可以监控到consul的控制台嗷！！！

![image-20210804212943249](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210804212950.png)





## 注册服务提供者：

1. 新建module
2. POM：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud</artifactId>
        <groupId>com.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-providerconsul-payment8006</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>

<!--        整合web组件-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

<!--        日常jar包配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

3. YAML：

```yaml
server:
  port: 8006

spring:
  application:
    name: consul-provider-payment

#consul注册中心地址
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

4. 主启动：

5. 业务类Controller

```java
package com.tuzi.cloud.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.UUID;

@RestController
public class PaymentController
{
    @Value("${server.port}")
    private String serverPort;

    @RequestMapping(value = "/payment/consul")
    public String paymentConsul()
    {
        return "SpringCloud with consul: "+serverPort+"\t"+ UUID.randomUUID().toString();
    }
}
```

6. 验证测试：

![image-20210804224328179](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210804224328.png)

![image-20210804224437015](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210804224437.png)

## 注册服务消费者：

1. 新建module
2. POM文件配置：

和上面是一样的嗷！！！

3. application.yml：

```yaml
server:
  port: 80

spring:
  application:
    name: consul-consumer-order

  #consul注册中心地址
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

4. 主启动：

和上面是一样的嗷！！！

5. 配置业务类：

- 添加config文件夹，加入那个操作小工具RestTemplate，用于简化返回JSON的操作嗷！！！：

```java
@Configuration
public class ApplicationContextConfig {
    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

- Controller：

```java
@RestController
@Slf4j
public class OrderConsulController
{
    public static final String INVOKE_URL = "http://consul-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping(value = "/consumer/payment/consul")
    public String paymentInfo(){
        String result = restTemplate.getForObject(INVOKE_URL+"/payment/consul",String.class);
        return result;
    }
}
```

- 测试：

![image-20210805170840324](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805170847.png)

![image-20210805170904325](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805170904.png)



## 总结：

![image-20210805171407613](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805171407.png)

![image-20210805171442249](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805171442.png)

- 思想都是差不多一样的嗷！！！
- eureka , zookeeper , consul的本质都是一样的，服务注册中心，用法也都是一样的嗷！！！无非是创建对应的模块，注册到服务中心嗷！！！
- CAP中，P一定要保证，因此不是AP就是CP嗷！！！
- AP是Eureka , CP是ZooKeeper和Consul。CAP关注的是数据而不是整体的策略嗷！！！例如淘宝这种高并发的，A就很重要，你可以由于数据不一致得罪一些消费者（例如我们经常看到的，还有产品但是秒杀不到），但是绝对不允许整个网站垮了用不了。马爸爸可能会刀了你嗷！！！



# Ribbon负载均衡

- Ribbon是基于**客户端**软负载均衡的工具。
- 已经进入了维护模式，但是有强大的声明力。
- 作用：
  - **LoadBalance**：
    - 集中式（服务端）
    - 进程内（本地，Ribbon就是属于这种，继承于消费方进程，通过它获取到服务提供方的地址嗷！！！）
  - 例如80通过轮询访问8001/8002
  - 一句话：**LB + RestTemplate调用**
- 开始前，先恢复我们的项目如下所示：

![image-20210805193418204](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805193425.png)

- Ribbon逻辑架构图：

![image-20210805193654507](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805193654.png)

工作步骤：

1. 选择EurekaServer，优先选择负载较少的server
2. 根据用户指定的策略，在server取到的服务注册列表中选择一个地址嗷！！！



- 问题？为啥我们之前没有引入ribbon都能够实现负载均衡？？？   --->   我们引入的netflix-eureka-client中自带了ribbon了嗷！！！



## RestTemplate的使用：

1. getForObject和getForEntity
2. postForObject和postForEntity
3. GET请求方法
4. POST请求方法



- getForObject和getForEntity区别：
  - getForObject返回的结果基本上可以理解为Json
  - getForEntity返回的结果为ResponseEntity对象，包含了响应中的一些重要的信息。
- postForEntity：
  - 使用方法更加复杂嗷！！！
  - ![image-20210805200601833](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805200602.png)
- 推荐使用ForObject直接安抚你会JSON而不是ResponseEntity嗷！！！



## 深入理解+面试常问：

- 负载均衡算法：

  - 轮询
  - lRule：根据特定算法从服务列表中选取一个要访问的服务。
  - ![image-20210805204525541](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805204525.png)
  - ![image-20210805204613614](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805204613.png)

  默认就是轮询嗷！！！

- 如何替换算法：

  - 修改cloud-consumer-order80
  - 配置细节
  - 新建package -> com.tuzi.myrule
  - 上面包下新建MySelfRule规则类
  - 主启动类添加@RibbonClient
  - 测试

- 替换细节：

  - 配置细节如下所示：![image-20210805205006582](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805205006.png)

  ComponentScan这个注解在SpringBootApplication这个注解的下面嗷！！！

  因此需要放在com.tuzi.cloud这个包的外面嗷！！！

  - 新建包以及MySelfRule内容如下：![image-20210805205412865](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805205412.png)

  - ```java
    @SpringBootApplication
    @EnableEurekaClient
    @RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = MySelfRule.class)
    public class OrderMain80
    {
      public static void main(String[] args) {
          //主启动类配置
          SpringApplication.run(OrderMain80.class,args);
      }
    }
    ```
    
    - 测试结果就很棒！！！
  
- 负载均衡算法深入理解：

  - 轮询原理：
    - ![image-20210805210333357](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805210333.png)
    - ![image-20210805210305221](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805210305.png)
    - 轮询重启的话，重新从1开始嗷！！！
    
  - 轮询算法源码解析：
    - Ctrl + Alt + B可以查看IRULE这个接口的实现类嗷！！！
    - 然后可以查看轮询算法的源码，重点在于choose方法这个部分嗷！！！
    
  - 手写负载算法：

    - 原理+JUC（CAS+自旋锁的复习）
    - ![image-20210806093635672](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210806093642.png)
    - 改造：

    Controller中多加入一个功能嗷！！！

    ```java
    @GetMapping(value = "/payment/lb")
    public String getPaymentLB(){
        return serverPort;
    }
    ```

    - 80订单微服务改造：

    ![image-20210806095701845](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210806095702.png)

    1. 去掉**@LoadBalanced**，这个就是加入的那个配置类，去掉LoadBalancer，不使用默认的。
    2. **LoadBalancer接口**：

    ```java
    public interface LoadBalancer
    {
        ServiceInstance instances(List<ServiceInstance> serviceInstances);
    
    }
    ```

    3. **MyLB**：

    ```java
    @Component
    public class MyLB implements LoadBalancer
    {
        private AtomicInteger atomicInteger = new AtomicInteger(0);
    
        public final int getAndIncrement(){
            int current;
            int next;
    
            do{
                current = this.atomicInteger.get();
                next = current >= 2147483647 ? 0 : current + 1;
            }while(!this.atomicInteger.compareAndSet(current,next));
            System.out.println("*****第几次访问，次数next: "+next);
            return next;
        }
    
        @Override
        public ServiceInstance instances(List<ServiceInstance> serviceInstances)
        {
            int index = getAndIncrement() % serviceInstances.size();
            return serviceInstances.get(index);
        }
    }
    ```

    4. **OrderController**：

    ```java
    @Slf4j
    @RestController
    public class OrderController
    {
    //    public static final String PAYMENT_URL = "http://localhost:8001";
        public static final String PAYMENT_URL = "http://CLOUD-PAYMENT-SERVICE";
    
        @Autowired
        private RestTemplate restTemplate;
    
        @Autowired
        private LoadBalancer loadBalancer;
        @Autowired
        private DiscoveryClient discoveryClient;
    
        @GetMapping("/consumer/payment/create")
        public CommonResult<Payment> create(Payment payment)
        {
            return restTemplate.postForObject(PAYMENT_URL+"/payment/create",payment,CommonResult.class);
        }
    
        @GetMapping("/consumer/payment/get/{id}")
        public CommonResult<Payment> getPayment(@PathVariable("id") long id)
        {
            return restTemplate.getForObject(PAYMENT_URL+"/payment/get/"+id,CommonResult.class);
        }
    
        @GetMapping("/consumer/payment/getForEntity/{id}")
        public CommonResult<Payment> getPayment2(@PathVariable("id") long id)
        {
            ResponseEntity<CommonResult> entity = restTemplate.getForEntity(PAYMENT_URL+"/payment/get/"+id,CommonResult.class);
            if (entity.getStatusCode().is2xxSuccessful()){
                return entity.getBody();
            }else {
                return new CommonResult<>(444,"操作失败");
            }
        }
    
        @GetMapping(value = "/consumer/payment/lb")
        public String getPaymentLB()
        {
            List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
            if(instances == null || instances.size() <= 0){
                return null;
            }
    
            ServiceInstance serviceInstance = loadBalancer.instances(instances);
            URI uri = serviceInstance.getUri();
    
            return restTemplate.getForObject(uri+"/payment/lb",String.class);
        }
    }
    ```

    5. 测试：

    ![image-20210806113351561](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210806113351.png)



# OpenFeign服务调用：

- 让编写**客户端**更加容易嗷！！！只需要创建一个接口上添加注解锁。Ribbon+RestTemplate -> 封装为Feign。简化了使用Ribbon的时候，客户端。
- Feign对于服务端的接口和客户端的接口进行一对一绑定。创建一个微服务接口，并在接口上添加注解即可。（微服务调用接口+@FeignClient）



## 使用步骤：

1. 新建Module

2. POM

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud</artifactId>
        <groupId>com.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-consumer-feign-order80</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>

        <dependency>
            <groupId>com.atguigu.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <!--        整合web组件-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <!--        日常jar包配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

</project>
```

3. application.yml

```yaml
server:
  port: 80

eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
```

4. 主启动

```java
@EnableFeignClients
@SpringBootApplication
public class OrderFeign80 {
  public static void main(String[] args) {
  		SpringApplication.run(OrderFeign80.class,args);
  }
}
```

5. 业务类

![image-20210806125041185](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210806125041.png)

![image-20210806131806776](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210806131806.png)

- 需要一个service：

```java
@Component
@FeignClient(value = "CLOUD-PAYMENT-SERVICE")
public interface PaymentFeignService
{
    @GetMapping(value = "/payment/get/{id}") //这个方法是服务端拥有的，我们通过@FeignClient将其对应起来。
    public CommonResult getPaymentById(@PathVariable("id") long id);
}
```

- 控制台Controller：

```java
@RestController
@Slf4j
public class OrderFeignController
{
    @Autowired
    private PaymentFeignService paymentFeignService;

    @GetMapping(value = "/consumer/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id)
    {
        return paymentFeignService.getPaymentById(id);
    }
}
```

![image-20210806132254785](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210806132254.png)

相当于自己定义的接口，加上Feign之后，指定了对应服务的方法嗷！！！

6. 测试

总结：

![image-20210806132959659](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210806133000.png)

- 原来用RestTemplate进行调用，和我们的编程习惯不太一样。我们习惯于使用xxxController调用xxxService，feign相当于在客户端定义了服务端提供的服务的方法，供客户端调用嗷！！！



## 超时控制：

- 服务端加上对应的sleep方法
- 客户端去调用服务端的方法，注意，feign默认的超时设置是1秒钟嗷！！！如果客户端一秒钟之后没有返回结果就会报错嗷！！！
- 设置Feign客户端的超时时间：
  
  - 本质是由底层的ribbon来进行设置的嗷！！！
  
  - ```yaml
    ribbon:
      ReadTimeout: 5000
      ConnectTimeout: 5000
    ```
  
  - 上面这个一个是连接的超时设置，还有一个是建立连接后读取资源的超时设置嗷！！！！



## 日志打印功能：

![image-20210806175818714](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210806175825.png)

- 需要一个配置的日志Bean：

```java
package com.tuzi.cloud.config;

@Configuration
public class FeignConfig
{
    @Bean
    Logger.Level feignLoggerLevel(){
        return Logger.Level.FULL;
    }
}
```

- Yaml文件中开启日志的Feign客户端：

```yaml
logging:
  level:
    com.tuzi.cloud.service.PaymentFeignService: debug
```



# Hystrix服务降级：

- 服务熔断的理念：避免整个服务的雪崩嗷！！！
- 作用：
  - 服务降级
  - 服务熔断
  - 接近实时的监控
  - ...
- 停更了已经



## 重要概念：

- 服务降级：
  - fallback
  - 假设对方系统不可以用了，需要兜底的方式来进行处理。例如：卡死了，给一个友好提示，例如：出问题了，请稍后再试
  - 出现情况：
    - 程序运行异常
    - 超时
    - 服务熔断除法服务降级
    - 线程池/信号量打满
  - 服务的降级 -> 服务的熔断 -> 恢复调用链路



- 服务熔断：
  - break
  - 就像家里的保险丝，会跳闸的
  - 类似于服务达到最大访问量了，这个使用它会自动调用服务降级的方法并返回友好提示。



- 服务限流：
  - flowlimit
  - 秒杀等高并发的操作，严禁一窝蜂的拥挤，大家排队一个N个，这样来嗷！！！服务器的保安。



## 案例编码：

- 用于处理分布式系统的延迟和容错的开源库，许多依赖不可避免会调用失败，提高分布式系统的弹性。
- 断路器本身是一种开关装置，某个服务单元故障之后，通过断路器的故障监控，返回一个符合预期的，可处理的备选响应，而不是长时间的等待或者抛出调用方无法处理的异常。



### 构建平台：

- 这里我们不使用eureka集群了，将7001的application.yml中的相应配置指向自己。使用7001 , 8001 , 8002 , 80



### 步骤（8001）：

1. 新建module:

cloud-provider-hystrix-payment8001

2. pom.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud</artifactId>
        <groupId>com.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-hystrix-payment8001</artifactId>

    <dependencies>
        <!--  hystrix  -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>

        <!--        坐标监控的时候十分重要，这两者一般都是一起出现的嗷！！！-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <!--        下面是数据库-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>

        <!--        mysql-connector-java-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <!--        jdbc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>com.atguigu.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <!--        eureka相关配置-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
    </dependencies>

</project>
```

3. application.yml：

```yaml
server:
  port: 8001

spring:
  application:
    name: cloud-provider-hystrix-payment

eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
```

- 注意一下哈，如果这里使用集群的话，这里的defaultZone对应的内容应该是集群中的每一个服务器嗷！！！



4. 主启动：

```java
@SpringBootApplication
@EnableEurekaClient
public class PaymentHystrixMain8001 {
  public static void main(String[] args) {
    //
      SpringApplication.run(PaymentHystrixMain8001.class,args);
  }
}
```



5. 业务类：

- service方法（为了简洁，这里没有采用定义接口，写实现类的方式嗷！！！）：

```java
@Service
public class PaymentService
{
    //正常访问，肯定OK
    public String paymentInfo_OK(Integer id)
    {
        return "线程池： "+Thread.currentThread().getName()+"paymentInfo_OK , id： "+id+"\t"+"O(n_n)O哈哈~";
    }

    //睡3秒，
    public String paymentInfo_TimeOut(Integer id)
    {
        int timeNumber = 3;
        try {
            TimeUnit.SECONDS.sleep(timeNumber);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "线程池： "+Thread.currentThread().getName()+"paymentInfo_OK , id： "+id+"\t"+"O(n_n)O哈哈~"+" 耗时(秒钟)： "+timeNumber;
    }
}
```

- controller：

```java
@RestController
@Slf4j
public class PaymentController {

    @Resource
    private PaymentService paymentService;

    @Value("${server.port}")
    private String serverPort;

    @GetMapping("/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id){
        String result = paymentService.paymentInfo_OK(id);
        log.info("*****result: "+result);
        return result;
    }

    @GetMapping("/payment/hystrix/timeout/{id}")
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
        String result = paymentService.paymentInfo_TimeOut(id);
        log.info("*****result: "+result);
        return result;
    }
}
```

- 以上述为平台，从正确 -> 错误 -> 降级熔断 -> 恢复



## JMeter压力测试：

![image-20210806202843820](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210806202844.png)

![image-20210806202901697](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210806202901.png)



- 高并发会导致ok被timeout拖慢了orz，就很大问题嗷！！！

![image-20210806203222602](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210806203222.png)



- 这就演示了高并发情况下，系统的处理能力变弱，甚至可能直接卡死的情况嗷！！！



### 80加入（80）：

1. 新建module
2. pom.xml

3. application.yml

```yaml
server:
  port: 80
eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
```

4. 主启动：

```java
@SpringBootApplication
@EnableFeignClients
public class OrderHystrixMain80 {
  public static void main(String[] args) {
    //主启动类
      SpringApplication.run(OrderHystrixMain80.class,args);
  }
}
```

5. 业务类：

- 这里也使用了Feign，需要编写对应的Service：

```java
@Component
@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT")
public interface PaymentHytrixService
{
    @GetMapping("/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id);

    @GetMapping("/payment/hystrix/timeout/{id}")
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id);
}
```

- 我们编写对应的Controller：

```java
@RestController
@Slf4j
public class OrderHystrixController
{
    @Autowired
    private PaymentHytrixService paymentHytrixService;

    @GetMapping("/consumer/payment/hystrix/ok/{id}")
    public String paymentInfo_OK(@PathVariable("id") Integer id)
    {
        String result = paymentHytrixService.paymentInfo_OK(id);
        return result;
    }

    @GetMapping("/consumer/payment/hystrix/timeout/{id}")
    public String paymentInfo_TimeOut(@PathVariable("id") Integer id){
        String result = paymentHytrixService.paymentInfo_TimeOut(id);
        return result;
    }
}
```

- 压测，太难了呜呜，更加慢了。这个时候，可以从客户端和服务端两方面来解决问题嗷！！！

![image-20210807083807531](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807083808.png)



- 解决的要求：

  - 超时导致服务器变慢（转圈） -> 超时不再等待
  - 出错（宕机或程序运行出错） -> 出错要有兜底
  - 解决：

  ![image-20210807083945884](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807083946.png)



## 服务降级（fallback）：

- 降级的配置：
  - @HystrixCommand
  - 8001自身：设置超时时间的峰值，峰值内可以正常运行。超时了的话需要有兜底的方法处理，作服务降级fallback。



### 8001的服务降级：

对于Service的修改：

```java
//下面是服务降级嗷！！！
@HystrixCommand(fallbackMethod = "paymentInfo_TimeOutHandler" , commandProperties = {
        @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "3000")
}) //确定了服务正常的时间是3秒钟，3秒如果过了（超时错误），就采用paymentInfo_TimeOutHandler兜底
public String paymentInfo_TimeOut(Integer id)
{
    int timeNumber = 5;
    try {
        TimeUnit.SECONDS.sleep(timeNumber);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    return "线程池： "+Thread.currentThread().getName()+"paymentInfo_TimeOut , id： "+id+"\t"+"O(n_n)O哈哈~" + " 耗时(秒钟)： "+timeNumber;
}

public String paymentInfo_TimeOutHandler(Integer id)
{
    return "线程池： "+Thread.currentThread().getName()+"paymentInfo_TimeOutHandler , id： "+id+"\t"+"O(QAQ)O服务失败了呜呜~" ;
}
```

主启动类对于这个注解的激活：

主启动类添加@EnableCircuitBreaker注解嗷！！！

- 测试：

![image-20210807085709345](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807085709.png)

- 结论：
  - 那如果例如10/0这种异常，能不能够处理呢？结论是能够处理的嗷！！！
  - 服务只要不可用了，兜底方案都是paymentInfo_TimeOutHandler。后面的Properties无非是加入了一些条件限制而已嗷！！！
  - 实际上就是我们对于两种错误：限时控制和出错处理，都能够找到兜底的解决方案。实际上是8001对于自身服务器的一种保护机制。



### 80的服务降级：

- 客户端对于自己的保护，既可以放在客户端，也可以放在服务端。一般常用于客户端。

![image-20210807090720815](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807090720.png)



- 步骤：

  - ```yaml
    feign:
      hystrix:
        enabled: true
    ```

  - 主启动加上`@EnableHystrix`
  - 和8001一样的，在Service中，写上兜底的方法嗷！！！
  - 修改Controller：

  ```java
  @GetMapping("/consumer/payment/hystrix/timeout/{id}")
  @HystrixCommand(fallbackMethod = "paymentInfo_TimeOutHandler" , commandProperties = {
          @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "1500")
  })
  public String paymentInfo_TimeOut(Integer id)
  {
      String result = paymentHytrixService.paymentInfo_TimeOut(id);
      return result;
  }
  
  public String paymentInfo_TimeOutHandler(Integer id)
  {
      return "我是消费者80，支付系统繁忙嗷，请稍后再试呢~" ;
  }
  ```



### 上述处理过程问题：

1. 业务逻辑方法和兜底方法混在一起，每个业务方法对应一个兜底的方法，代码膨胀。
2. 同一定义和自定义的代码分开（有没有全局的处理方法呢？）



### 代码膨胀问题：

- `@DefaultProperties(defaultFallback="")`
- ![image-20210807102038131](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807102038.png)

- 把通用的方法标注在最外面，通用的和独享的分开。
- 注意哈，@HystrixCommand还是要有的嗷！！！如果没有那就是不兜底！！！有@HystrixCommand的情况下，如果不指定就是用默认，如果指定就是用指定的嗷！！！



### 代码混乱问题：

- 抓主要矛盾，由于方法处于Controller（客户端）中，导致处理方法和业务方法混杂在一起。我们发现，Controller中实际调用的Service方法，都来自于Service接口，因此我们要看看Service接口有没有什么可以操作的嗷！！！
- 最狠的情况：调用服务的时候，服务端关闭或者宕机了。服务的降级处理这时候和服务端没有任何关系，都是在客户端完成的嗷！！！
- 解决方法：定义降级处理的实现类就可以实现解耦嗷！！！相当于把我们的业务方法和兜底方法给区分开为两个类，这样就很清晰哇！！！



- 解决方法：

  - 定义降级处理的实现Service抽象类，专门用于实现降级处理：

  ```java
  @Component
  public class PaymentFallbackService implements PaymentHytrixService {
      @Override
      public String paymentInfo_OK(Integer id) {
          return "-----PaymentFallbackService fall back paymentInfo_OK";
      }
  
      @Override
      public String paymentInfo_TimeOut(Integer id) {
          return "-----PaymentFallbackService fall back paymentInfo_TimeOut";
      }
  }
  ```

  - yaml：

  ```yaml
  feign:
    hystrix:
      enabled: true
  ```

  - 接口上配置上这个类嗷：

  ```java
  @Component
  @FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT" , fallback = PaymentFallbackService.class)
  public interface PaymentHytrixService
  ```

  - 这个时候如果服务端裂开了，也就是CLOUD-PAYMENT-HYSTRIX-PAYMENT裂开了，ok这个时候没有注解，也没有做任何处理，这个时候默认会调用我们的兜底类中的方法进行处理嗷！！



## 服务熔断（break）：

- 保险丝，出现问题及时报错。服务调用正常之后，恢复调用链路。
- 还是`@HystrixCommand`，连续多次调用失败之后，就有会熔断。**调用响应正常之后，可以自动恢复链路嗷。**



### 实操：

- 8001的PaymentService的修改嗷：

```java
//=====服务熔断
@HystrixCommand(fallbackMethod = "paymentCircuitBreaker_fallback",commandProperties = {
        @HystrixProperty(name = "circuitBreaker.enabled",value = "true"),//是否开启断路器
        @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"),//请求次数
        @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "10000"),//时间窗口期
        @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "60"),//失败率达到多少后跳闸
})
public String paymentCircuitBreaker(@PathVariable("id") Integer id) {
    if (id < 0) {
        throw new RuntimeException("******id 不能为负数嗷");
    }
    String serialNumber = IdUtil.simpleUUID();

    return Thread.currentThread().getName() + "\t" + "调用成功！！！流水号： " + serialNumber;
}

public String paymentCircuitBreaker_fallback(@PathVariable("id") Integer id){
    return "id不能够为负数，请稍后再试。/(ToT)/~~   id： " + id;
}
```

- Controller添加熔断相关的测试内容：

```java
//====服务熔断
@GetMapping("/payment/circuit/{id}")
public String paymentCircuitBreaker(@PathVariable("id") Integer id){
    String result = paymentService.paymentCircuitBreaker(id);
    log.info("****result: "+result);
    return result;
}
```

- 测试后发现：
  - 如果狂点负数，那后面就算有正数，也用不了。要一段时间之后，才能慢慢恢复。
  - 这就是断路器跳闸了，不可以用了，慢慢才可以使用嗷！！！Open -> Half Open -> Close



### 总结：

![image-20210807120020749](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807120021.png)



- 断路器三个重要参数：
  - 快照时间窗：断路器确定是否打开需要统计一些请求和错误数据，统计的时间范围是快照时间窗，默认是最近的10s。
  - 请求总数阈值：快照时间内，必须满足请求总数阈值才有资格熔断。默认为20，意味着在10s内，如果该hystrix命令调用次数不足20次。即使所有的请求都超时或其他原因失败，断路器都不会打开。
  - 错误百分比阈值：当请求总数在快照窗口内超过了阈值，比如发生了30此调用，如果在这30此调用中，有15次发生了超时异常，也就是50%的错误百分比，在默认设定50%阈值的情况下，这时候就会将断路器打开嗷！！！
- 断路器开始或结束的条件：

![image-20210807155624797](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807155625.png)



- 一些小问题：
  - 再有请求调用的时候，将不会调用主逻辑，而是直接调用降级fallback，通过断路器，实现了自动发现错误并将降级逻辑切换为主逻辑，减少响应延迟的效果。
  - 原来的主逻辑如何恢复呢？熔断之后，会有一个休眠时间窗，这个时间段内，降级逻辑是临时的成为了主逻辑。当休眠时间窗到期，断路器进入半开状态，释放一次请求到原来的主逻辑上。如果这次请求正常返回，那么断路器会继续闭合，主逻辑恢复。如果请求依然有问题，断路器会继续进入打开状态，休眠窗口重新计时。
  - 还有很多花活，看Properties里面啊之类的，或者百度，都可以看到服务熔断相关的其他一些属性嗷！！！



## 服务限流（FlowLimit）：

- 后续讲alibaba的Sentinel的时候说明。



## 工作流程：

- hystrix官网中有讲嗷！！！
- 还挺复杂的，可以上github官网去康康嗷，原理倒不是很难，就是有一点点复杂嗷！！！



## Hystrix Dashboard：

1. 新建module
2. POM
3. application.yml：

```java
server:
  port: 9001
```

4. 主启动

```java
@SpringBootApplication
@EnableHystrixDashboard
public class HystrixDashboardMain9001 {
  public static void main(String[] args) {
    //
      SpringApplication.run(HystrixDashboardMain9001.class,args);
  }
}
```

5. 直接让hystrix跑起来，访问localhost:9001/hystrix就能够访问到Dashboard页面：

![image-20210807162455359](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807162455.png)

监控的前提是我们必须配置了actuator才行嗷（Dashboard和你监控的对象都需要嗷！）！！！

6. 具体监控配置实例（以监控8001为例子嗷！！！）：

- actuator一定要有嗷！！！

- 大坑：需要对应的配置：

  - 主启动类中，对于服务监控的配置，是Cloud升级之后遇到的坑。

  ```java
  @SpringBootApplication
  @EnableEurekaClient
  @EnableCircuitBreaker
  public class PaymentHystrixMain8001 {
    public static void main(String[] args) {
    SpringApplication.run(PaymentHystrixMain8001.class, args);
    }
  
    @Bean
    public ServletRegistrationBean getServlet() {
      HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
      ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
      registrationBean.setLoadOnStartup(1);
      registrationBean.addUrlMappings("/hystrix.stream");
      registrationBean.setName("HystrixMetricsStreamServlet");
      return registrationBean;
    }
  }
  ```

- 主启动类上要加上注解：`@EnableCircuitBreaker`



7. 监控：

![image-20210807163345145](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807163345.png)

- 9001监控8001的地址为：`http://localhost:8001/hystrix.stream`

![image-20210807163542137](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807163542.png)



- 监控图表的查看：

![image-20210807164355392](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807164355.png)



![image-20210807164601475](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807164601.png)



# Gateway网关：

- 一般的企业架构：

![image-20210807170138920](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807170139.png)

- gateway最牛逼牛逼在异步非阻塞嗷！！！
- 三大核心概念：
  - 路由
  - 断言
  - 过滤



## 实践：

1. 新建module
2. POM：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud</artifactId>
        <groupId>com.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-gateway-gateway9527</artifactId>

    <dependencies>
        <!--网关-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>

        <dependency>
            <groupId>com.atguigu.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-web</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-actuator</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

- 这里是大坑啊兄弟！！！这里由于gateway是基于webflux而不是mvc架构的，要移除webmvc相关的依赖。这里注意，这里的api-commons里面是带有mvc的，还有额外将其移除掉才能够正常启动嗷！！！

3. application.yml：

```yaml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway

eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
```

4. 主启动：

```java
@SpringBootApplication
@EnableEurekaClient
public class GateWayMain9527 {
  public static void main(String[] args) {
    //主启动类
      SpringApplication.run(GateWayMain9527.class,args);
  }
}
```

5. 网关配置：

- 例如和我们计算机网络学的一样，我们不想暴露8001端口，我们希望在8001外面套上一层9527。
- application中做额外的配置嗷！！！

```yaml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      routes:
        - id: payment_routh   #payment_routes
          uri: http://localhost:8001
          predicates:
            - Path=/payment/get/**

        - id: payment_routh2   #payment_routes
            uri: http://localhost:8001
            predicates:
              - Path=/payment/lb/**

eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
```

- 测试：

![image-20210807194157012](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807194157.png)



![image-20210807201233727](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807201233.png)



![image-20210807201425312](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807201425.png)

9527现在也能够访问微服务端口了嗷！！！



- 网关配置的两种方法：

  - yml配置
  - 通过@Bean添加组件配置

  ![image-20210807201616154](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210807201616.png)





配置类编写的实例：

例如通过9527去访问百度新闻等：

```java
@Configuration
public class GateWayConfig {
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder){
        RouteLocatorBuilder.Builder routes = builder.routes();
        routes.route("path_route_tuzi",
                r -> r.path("/guonei").uri("http://news.baidu.com/guonei")).build();

        return routes.build();
    }
}
```

- 新建config包，并且在里面添加配置类。



## 网关实现负载均衡：

![image-20210808085404686](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808085404.png)

- 从网关处实现负载均衡。
- 环境：7001+8001+8002
- 操作：对于application.yml的配置：

```yaml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true    #开启从服务中心动态创建路由的功能，利用微服务名进行路由
      routes:
        - id: payment_routh   #payment_routes
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/get/**

        - id: payment_routh2   #payment_routes
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/lb/**

eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
```

- 上面这样就可以实现负载均衡了嗷！！！





## RoutePredicateFactory：

- 常用：

  - After Route Predicate：

    - 得到当前时间：

    ![image-20210808092351159](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808092352.png)

    - ![image-20210808092512971](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808092513.png)

    在某个时间结点之后，才能够正常访问，网管？？？

  - Before Route Predicate

  - Between Route Predicate

  ![image-20210808092747828](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808092748.png)

  上面两个和After一样的，在某个时间结点之前和在两个时间结点之间嗷！！！

  - Cookie Route Predicate：
    - 调试常用：
      - jmeter
      - postman
      - curl：
        - 命令行就可以使用
        - ![image-20210808093421928](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808093422.png)
        - ![](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808094410.png)
        - 可以通过--cookie这种方式来用curl带上cookie访问，十分方便嗷！！！
        - ![image-20210808095207417](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808095207.png)
        - 可以通过-H这种方式来用curl带上Header访问，十分方便嗷！！！
    - `- Cookie=key,value`
    - 可以限制符合条件的Cookie访问嗷！！！
  - Header Route Predicate：
    - `- Header=X-Request-Id, \d+`
    - ![image-20210808095802974](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808095803.png)

  - Host Route Predicate：

  ![image-20210808095923720](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808095923.png)

  

  ![image-20210808100056575](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808100056.png)

  

  ![image-20210808100140548](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808100140.png)

  - Method Route Predicate：

  `- Method=GET`

  - Path Route Predicate

  `- Path=/payment/lb/**`

  - Query Route Predicate

  带有参数条件的，这个就是有对于参数的匹配嗷！！！

  ![image-20210808100320887](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808100321.png)



## Filter：

- 生命周期：pre , post
- 种类：GatewayFilter , GlobalFilter



### 自定义Filter：

```java
package com.tuzi.cloud.filter;

import lombok.extern.slf4j.Slf4j;
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.core.Ordered;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

import java.util.Date;

@Component
@Slf4j
public class MyLogGatewayFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        log.info("*********come in MyLogGatewayFilter"+new Date());
        String uname = exchange.getRequest().getQueryParams().getFirst("uname");
        if(uname == null){
            log.info("你就是奸细？？？？？");
            exchange.getResponse().setStatusCode(HttpStatus.NOT_ACCEPTABLE);
            return exchange.getResponse().setComplete();
        }
        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        return 0;
    }
}
```

- order是用来控制顺序的嗷！！！这里就是判断有木有uname，如果没有为null的话就会报错。如果有的话，就往下走。（非常像是request和response嗷！！！）
- 7001 + 8001 + 8002 + 9527测试：

![image-20210808104831639](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808104838.png)

![image-20210808104845632](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808104845.png)

- 自定义全局才是最常用的嗷！！！





# Config分布式配置中心：

- 随着项目增多，配置(application)越来越多，需要一套集中式的，动态的配置中心。
- 提供集中化的外部配置支持，配置服务器为多个不同微服务应用的所有中心化的**外部配置**。（类似于git和github）。提取出公有的配置信息放在共享的外部配置嗷！！！（例如Mysql的配置）
- 需要服务端和客户端两个部分来使用嗷！！！
- 能干吗？

![image-20210808105626921](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808105627.png)

- 与github和git整合配置嗷！
- 原理图：

![image-20210809162116676](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809162116.png)



## Config服务端配置：

1. github上新建一个springcloud-config仓库，并且拉到本机D盘的springcloud-config文件夹
2. 项目中新建Module3344
3. pom.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud</artifactId>
        <groupId>com.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-config-center-3344</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

4. application.yml

```yaml
server:
  port: 3344

spring:
  application:
    name: cloud-config-center
  cloud:
    config:
      server:
        git:
          username: ******
          password: ******
          uri: 用官网给的https的那个嗷！！！
          skip-ssl-validation: true
          ####搜索目录
          search-paths:
            - springcloud-config
        ####读取分支
      label: main



#spring:
#  application:
#    name: cloud-config-center
#  cloud:
#    config:
#      server:
#        git:
#          uri: 用官网给的https的那个嗷！！！
#          skip-ssl-validation: true
#          ####搜索目录
#          search-paths:
#            - springcloud-config
#          username: ******
#          password: ******
#        ####读取分支
#      label: master


#服务注册到eureka地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka

```

5. 主启动：

```java
@SpringBootApplication
@EnableConfigServer
public class MainAppConfigCenter3344 {
  public static void main(String[] args) {
    //
      SpringApplication.run(MainAppConfigCenter3344.class,args);
  }
}
```

6. 本机host增加映射：

`127.0.0.1    config-3344.com`

7. 测试通过config微服务是否可以从github上获取配置内容：

![image-20210808154200972](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808154208.png)

![image-20210808154842200](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808154842.png)

- 获取某个分支的内容：

![image-20210808154736784](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808154737.png)

![image-20210808154912451](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808154912.png)

- 上面这一种默认读出来就是master/main分支的内容嗷！！！说白了，默认的就是master/main分支，可以通过label去配置嗷！！！

- 如果访问的资源文件不存的话，返回的就是空啊或其他一些奇奇怪怪的东西嗷！！！
- 顺着读取和逆着读取结果不一样嗷，如下所示：

![image-20210808155213113](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808155213.png)

![image-20210808155248407](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808155248.png)

推荐还是下面这一种嗷！！！





## Config客户端配置：

- 命名规则最好就是符合，config-prod.yml这种形式来哈！！！

- 创建过程：

  1. 新建module

  2. pom：

     - 注解和3344基本一样嗷！！！

     - ```xml
       <?xml version="1.0" encoding="UTF-8"?>
       <project xmlns="http://maven.apache.org/POM/4.0.0"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
           <parent>
           <artifactId>cloud</artifactId>
               <groupId>com.atguigu.springcloud</groupId>
             <version>1.0-SNAPSHOT</version>
           </parent>
         <modelVersion>4.0.0</modelVersion>
       
           <artifactId>cloud-config-client-3355</artifactId>
       
           <dependencies>
               <dependency>
                   <groupId>org.springframework.cloud</groupId>
                   <artifactId>spring-cloud-starter-config</artifactId>
               </dependency>
       
               <dependency>
                   <groupId>org.springframework.cloud</groupId>
                   <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
               </dependency>
               <dependency>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-starter-web</artifactId>
               </dependency>
               <dependency>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-starter-actuator</artifactId>
               </dependency>
               <dependency>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-devtools</artifactId>
                   <scope>runtime</scope>
                   <optional>true</optional>
               </dependency>
               <dependency>
                   <groupId>org.projectlombok</groupId>
                   <artifactId>lombok</artifactId>
                   <optional>true</optional>
               </dependency>
               <dependency>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-starter-test</artifactId>
                   <scope>test</scope>
               </dependency>
       
               <dependency>
                   <groupId>org.springframework.cloud</groupId>
                   <artifactId>spring-cloud-starter-bootstrap</artifactId>
                   <version>3.0.0</version>
               </dependency>
           </dependencies>
       
       </project>
       ```
       
     - 上面这个依赖不是server啦，其他都是一样的嗷！！！
  
  3. yaml文件：
  
  ![image-20210808161052589](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808161053.png)
  
  这里遇到了一个非常大的坑，bookstrap.yml被IDEA识别不到。该怎么办：
  
  ![image-20210808174249486](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808174256.png)
  
  上图这个位置可以添加，但是注意哦！这个绿叶叶一开始没有，**一定要有主启动类，并且标注了@SpringBootApplication之后，这个绿叶叶才会被激活，才可以添加bootstrap.yml。**
  
  ```yaml
  server:
    port: 3355
  
  spring:
    application:
      name: config-client
    cloud:
      config:
        label: master   #分支名称
        name: config    #配置文件名称
        profile: dev    #读取后缀名称
        #上面三个结合，就是master分支下的config-dev.yml的配置文件
        uri: http://localhost:3344 #配置中心的地址
  
  #服务注册到eureka地址
  eureka:
    client:
      service-url:
        defaultZone: http://localhost:7001/eureka
  ```
  
  4. 主启动类：
  
  理论上来说应该先配置主启动类的，因为bookstrap.yml识别不到嗷。
  
  5. 业务类：
  
  ```java
  @RestController
  public class ConfigClientController {
      @Value("${info}")
      private String configInfo;
  
      @GetMapping("/configInfo")
      public String getConfigInfo(){
          return configInfo;
      }
  }
  ```
  
  6. 测试：
  
  7001 + 3344 + 3355：
  
  ![image-20210809160937672](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809160945.png)
  
  7. 踩过的大坑：首先，要引入多一个依赖，bootstrap的依赖。其次，yml文件的名字是bootstrap不是bookstrap。这个非常坑！！！![image-20210809161054660](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809161054.png)注意，左下角是云，不是电源嗷！！！


- 云端更新了之后，3344立即变了，3355有缓存，没有及时发生改变！！！3355无法动态刷新嗷！！！



### 客户端手动版动态刷新：

- 3355引入actuator依赖
- yaml添加配置暴露监控端点：

```yaml
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

- Controller上添加@RefreshScope

- 测试：

  - github --> 3344 --> 3355
  - 配置没有生效？？？裂开来，3355还是不能动态更新！！！

- How？运维工程师发送Post请求刷新3355

  - `curl -X POST "http://localhost:3355/actuator/refresh"`
  - 结果：`["config.client.version","config.info"]`
  - 再次刷新3355（3355并未重新启动）：

  ![image-20210809165829065](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809165829.png)

- 配置工程师：

  - 改配置。
  - 发信息通知3355，避免了重启。



### 客户端自动版动态刷新：

- 广播，一次通知，处处生效。实现定点打击，差异化的处理，该通知的通知，该处理的处理。（for循环）
- 现在还做不到，那咋办？消息总线Bus，请听下回分解。



# 消息总线+RabbitMQ：

- 搭配config一起使用，一般这种情况下消息中间件是躲不掉的~例如RabbitMQ等。
- Bus + Config 实现自动更新。
- Bus支持两种消息代理：RabbitMQ和Kafka。
- 就相当于订阅公众号一样的嗷！！！
- 原理图：

![image-20210809165753432](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809165753.png)



## RabbitMQ环境配置：

1. 安装Erlang
2. 安装RabbitMQ
3. 进入RabbitMQ安装目录下的sbin目录
4. 输入一下命令启动管理功能：
   - `rabbitmq-plugins enable rabbitmq_management`
   - 可视化插件
5. 查看是否安装成功
6. 输入账号密码登录：guest   guest



- 具体安装细节：
  - [erlang的版本和RabbitMQ版本对照](https://www.rabbitmq.com/which-erlang.html)
  - [RabbitMQ安装教程](https://blog.csdn.net/lu1005287365/article/details/52315786)



- 安装完成测试：

  1. 使用工具中的start：

  ![image-20210809190347362](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809190347.png)

  2. ![image-20210809190433786](C:\Users\Alexander\AppData\Roaming\Typora\typora-user-images\image-20210809190433786.png)
  3. 默认账号和密码为guest和guest，登录尝试：

  ![image-20210809190533279](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809190533.png)



## 广播实践：

- 为了演示效果，以3355为模板再去制作一个3366
- 除了端口，所有都类似的嗷！！！



### 设计思想（技术选型）：

- 广播的两种设计思想：

  - 利用bus触发一个客户端/bus/refresh，刷新所有客户端的配置

  ![image-20210809191620185](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809191620.png)

  - 利用消息总线触发一个服务端ConfigServer的/bus/refresh端点，从而刷新所有客户端的配置。

  ![image-20210809191704885](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809191705.png)

- 图二的架构更加适合嗷（推荐）！！！图一的局限性：

  - 微服务职责的单一性
  - 破坏了结点对等性
  - 有一定的局限性：微服务迁移的时候，网络地址常常会发生变化嗷！！！



### 实践：

- 3344来通知3355和3366

- 3344：

  - 引入RabbitMQ的依赖：

  ```xml
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-bus-amqp</artifactId>
  </dependency>
  ```

  引入了上面的依赖之后MQ和Bus都有了

  - yaml添加MQ相应的配置嗷！！！

  ```yaml
  # rabbitmq相关的配置
  spring:
    rabbitmq:
      host: localhost
      port: 5672
      username: guest
      password: guest
      
  #rabbitmq相关配置，暴露bus刷新配置的端点
  management:
    endpoints: #暴露bus刷新配置的端点
      web:
        exposure:
          include: 'bus-refresh'
  ```

- 3355 / 3366：

  - POM：

  ```xml
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-bus-amqp</artifactId>
  </dependency>
  ```

  - YML：

  ```yaml
  #rabbitmq相关配置，15672是管理界面的窗口，5672是MQ访问的端口
  spring:
    rabbitmq:
      host: localhost
      port: 5672
      username: guest
      password: guest
  ```

- 测试（7001 , 3344 , 3355 , 3366）：
  - 	更改github上的文件
  - 	广播通知3344：

  注意这里向3344发的哈！！！`curl -X POST "http://localhost:3344/actuator/bus-refresh"`

  - 	观察3355和3366是否也会跟着发生改变：

  ![image-20210809200644418](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809200644.png)

  芜湖出来了出来了嗷！！！！！！



- 基本原理：

![image-20210809201028920](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809201029.png)



## 动态刷新定点通知：

- 精确打击，定点清除。

- 例如只通知3355，不通知3366：

  ![image-20210809201429725](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809201430.png)

- 注意后面那个config-client:3355哈，这个是微服务名字加上端口号嗷！！！

- 下面这一就是整体的流程和原理嗷！！！

![image-20210809202219257](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809202219.png)





# Stream消息驱动：

- 不关注具体MQ的细节，用一种适配绑定的方式，自动给我们在各种MQ内切换。

- 目的：屏蔽底层消息中间件的差异，降低切换成本，统一消息的编程模型。

- Binder（屏蔽了底层的技术细节），我们只需要使用Binder，Binder帮助我们实际上去操作底层的消息中间件。

- 设计思想：

  - ![image-20210809203946168](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809203946.png)
  - ![image-20210809205708038](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809205708.png)
  - 采用的是发布-订阅模式嗷！！！

- 常用注解和api：

  - 套路示意图：

  ![image-20210809205913620](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809205914.png)

  - 重要组成部分：
    - Binder：很方便连接中间件，屏蔽差异
    - Channel：通道，Queue的一种抽象，实现存储和转发的媒介，通过Channel对队列进行配置。
  - ![image-20210809210411672](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809210412.png)



## 实践：

- 新建8801：生产者，8802和8803：消费者。

### 生产者：

1. 新建module（cloud-stream-rabbitmq-provider8801）
2. Pom：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud</artifactId>
        <groupId>com.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-stream-rabbitmq-provider8801</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

3. Yaml：

```yaml
server:
  port: 8801

spring:
  application:
    name: cloud-stream-provider
  cloud:
    stream:
      binders: # 在此处配置要绑定的rabbitmq的服务信息；
        defaultRabbit: # 表示定义的名称，用于于binding整合
          type: rabbit # 消息组件类型
          environment: # 设置rabbitmq的相关的环境配置
            spring:
              rabbitmq:
                host: localhost
                port: 5672
                username: guest
                password: guest
      bindings: # 服务的整合处理
        output: # 这个名字是一个通道的名称
          destination: studyExchange # 表示要使用的Exchange名称定义
          content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
          binder: defaultRabbit  # 设置要绑定的消息服务的具体设置

eureka:
  client: # 客户端进行Eureka注册的配置
    service-url:
      defaultZone: http://localhost:7001/eureka
  instance:
    lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
    lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
    instance-id: send-8801.com  # 在信息列表时显示主机名称
    prefer-ip-address: true     # 访问的路径变为IP地址
```

4. 主启动
5. 业务类：

- service接口：

```java
public interface IMessageProvider {
    public String send();
}
```

- 实现类：

引入的Source为![image-20210809213043359](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809213043.png)

这个Source就是上面套路示意图中的那个Source嗷！

```java
//可以不用@Service，业务逻辑不是传统的业务逻辑被调用了，是被用于和Binder打交道啦
@EnableBinding(Source.class) //定义消息的推送管道
public class IMessageProviderImpl implements IMessageProvider {

    @Resource
    private MessageChannel output; //消息推送管道

    @Override
    public String send()
    {
        String serial = UUID.randomUUID().toString();
        output.send(MessageBuilder.withPayload(serial).build());
        System.out.println("******** serial: " + serial);
        return null;
    }
}
```

调用这个Service是用于和Binder打交道，所以和我们以往的Service不一样嗷！！！

6. 测试：

- 启动7001
- 启动rabbitmq
- 启动8801
- 访问

![image-20210809215215097](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809215215.png)

![image-20210809215232209](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809215232.png)

![image-20210809215317783](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210809215317.png)







### 消费者：

1. 新建module：

cloud-stream-rabbitmq-consumer8802

2. POM.xml：

同8801

3. Yaml：

和上面几乎一样，一点小区别：

![image-20210810090652724](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810090653.png)

下面还有一个instance-id，也要把8801改成8802嗷！

4. 主启动

5. 业务类

```java
@Component
@EnableBinding(Sink.class)
public class ReceiveMessageListenerController {
    @Value("${server.port}")
    private String serverPort;

    @StreamListener(Sink.INPUT)
    public void input(Message<String> message)
    {
        System.out.println("消费者一号获得的信息为：message.getPayload() = " + message.getPayload() + "\t port: "+serverPort);
    }
}
```

6. 测试：

- 7001+8801+8802
- 测试8801向8802发送消息



![image-20210810093713693](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810093714.png)



![image-20210810093741447](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810093741.png)

- 感觉有点像管道的感觉。生产者放入管道，消费者就会等待生产者的消息，接受到消息之后，会及时进行处理。



### 分组消费与持久化：

![image-20210810094530024](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810094530.png)

- 依照8802复制出一份8803嗷！！！

![image-20210810094805089](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810094805.png)

- 问题：
  - 有重复消费问题
  - 消息持久化问题
- 用分组的方式来解决：
  - 重复消费会导致数据错误，多个消费者是**竞争关系**，应该保证其中的一个应用消费一次。



#### 自定义Group分组配置：

- 自定义配置为同一个组，保证消息只会被其中一个应用消费一次。不同的组是可以消费的，同一个组内会发生竞争关系，只有其中一个可以消费。



- 如何分组？

  - 分为：tuziA , tuziB
  - 配置方法：

  ![image-20210810174529772](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810174530.png)

  - 配置完成效果：

  ![image-20210810174622362](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810174622.png)

  不同的组之间是可以重复消费的，默认就是分为不同的组，所以会产生重复消费的问题。

- 解决重复消费：

  - 8802和8803设置为同一个组嗷！！！
  - 默认是轮询消费，就是生产者放入的信息，会由同一个组中的不同消费者轮询消费嗷！！！

  ![image-20210810175002099](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810175002.png)

  测试可知确实是轮询消费的嗷！！！

#### 持久化演示和配置：

- 8803的分组不动，8802的分组注释掉，做一个对比。
- 8802和8803都关机，在关机期间，8801发送四条消息。
- 8802拿不到它关机期间的消息，但是8803却会主动从MQ中拿出它关机期间的消息。说白了就是消息丢失了。
- 所以其实，加上了group分组，就实现了消息的持久化嗷！！！



# Sleuth链路监控：

- 用于监控链路
- 可以结合Zipkin使用，Zipkin是用于展示的嗷！！！



- Zipkin下载：
  - [Zipkin下载](http://guide.wangjubao.com/15_monitor_invoke/content.html#zipkin_download)
  - 新版本的zipkin，下载完成之后，使用java -jar命令就很方便的运行起来了嗷！！！
  - ![image-20210810192314480](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810192314.png)

- Zipkin主页面打开：`http://localhost:9411/zipkin/`
- ![image-20210810192827660](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810192827.png)



## 监控展示：

- 7001 ， 8001 ， 80
- 8001要引入POM：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

- 8001的yaml要变更嗷！！！

```yaml
spring:
  zipkin:
    base-url: http://localhost:9411
  sleuth:
    sampler:
      probability: 1
    #采样值介于0到1之间，1表示全部采集
```

监控的数据和图标要打到9411上

sleuth的采样率为0到1，一般0.5也够用了，1最全嗷！！！

- 多加上一个请求嗷！！！

```java
@GetMapping("/payment/zipkin")
public String paymentZipkin(){
    return "Hi , i'm paymentZipkin server all back, welcome to 兔子的小窝";
}
```

- 80也要改嗷！！！

  - 加上依赖
  - Yaml和上面一样修改
  - 业务类：

  ```java
  @GetMapping("/consumer/payment/zipkin")
  public String paymentZipkin(){
      String result = restTemplate.getForObject("http://localhost:8001"+"/payment/zipkin",String.class);
      return result;
  }
  ```

- 依次启动7001 ， 8001 ， 80。80调用几次8001测试一下嗷！！！
- 查看9411端口的主界面：

![image-20210810201430895](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810201431.png)

![image-20210810201603118](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810201603.png)

![image-20210810201625710](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210810201625.png)









# SpringCloud Alibaba：

- 参考资料：
  - [Alibaba中文文档](https://github.com/alibaba/spring-cloud-alibaba/blob/master/README-zh.md)
  - Spring官网上其实也有相应的内容嗷！！！



# Nacos服务注册和配置中心：

- **Na**ming + **co**figuration + **s**ervice
- Nacos就是注册中心+配置中心的整合：Nacos + Config + Bus
- 作用：
  - 替代Eureka做服务注册中心
  - 替代Config做服务配置中心
- [Nacos官网](https://nacos.io/zh-cn/)
- 下载的话，从官网进去，主页面下面有个对应的版本说明，点进去找对应的版本下载即可嗷！！！

## 下载安装：

- windows对应的是zip文件，不是tar.gz嗷，下载下来就可以啦！！！
- 版本选择1.1.4，全新版本的Nacos好像有一些不一样嗷！！！还需要康康呢！！！

![image-20210811093712676](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811093720.png)

- ![image-20210811093830577](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811093831.png)
- 默认用户名密码都是nacos，能登录进去就可以用嗷！！！



## 实践操作：

- alibaba依赖在父pom里面已经添加了，不需要添加额外的依赖了嗷！！！

### 服务注册中心：

#### 生产者：

- 服务提供者：

  - 这里的老五步，复杂的我就不写了嗷！！！

  - ```xml
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>
    ```

  - Yaml：

  - ```yaml
    server:
      port: 9001
    
    spring:
      application:
        name: nacos-payment-provider
      cloud:
        nacos:
          discovery:
            server-addr: localhost:8848 #配置nacos注册的地址
    
    management:
      endpoints:
        web:
          exposure:
            include: '*'
    ```

  - 主启动：

  - ```java
    @SpringBootApplication
    @EnableDiscoveryClient
    public class PaymentMain9001
    {
      public static void main(String[] args) {
        //主启动类
          SpringApplication.run(PaymentMain9001.class,args);
      }
    }
    ```

  - 业务类：

  - ```java
    @RestController
    public class PaymentController
    {
        @Value("${server.port}")
        private String serverPort;
    
        @GetMapping(value = "/payment/nacos/{id}")
        public String getPayment(@PathVariable("id") Integer id){
            return "nacos registry, serverPort: " + serverPort + "\t id: "+id;
        }
    }
    ```

  - 查看：
  - ![image-20210811100135222](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811100135.png)

  - 投机取巧，迅速启动一个配置相同的服务：

  ![image-20210811100347018](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811100347.png)

  

  - 再新建一个9002和上面几乎是一样的嗷，就是端口不一样而已。



#### 消费者：

- yaml：

```yaml
server:
  port: 83


spring:
  application:
    name: nacos-order-consumer
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848


service-url:
  nacos-user-service: http://nacos-payment-provider
```

注意下面这里使用服务名字哈，nacos本身就整合了ribbon，可以实现负载均衡，这个就是一个测试嗷！！！

- 有ribbon就自带RestTemplate，还需要注入到容器中才可以使用嗷！！！

```java
@Configuration
public class ApplicationContextConfig 
{
    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

- 业务类：

```java
@RestController
@Slf4j
public class OrderNacosController {
    @Resource
    private RestTemplate restTemplate;

    @Value("${service-url.nacos-user-service}")
    private String serviceURL;

    @GetMapping(value = "/consumer/payment/nacos/{id}")
    public String paymentInfo(@PathVariable("id") Long id){
        return restTemplate.getForObject(serviceURL+"/payment/nacos/"+id,String.class);
    }
}
```

- 测试的时候，默认实现轮询，因为Nacos默认整合了Ribbon



#### 对比：

- Nacos同时支持AP或CP

![image-20210811105133578](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811105133.png)

- Nacos切换AP和CP模式：

![image-20210811105329516](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811105330.png)






### 服务配置中心：

#### 基础配置：

- 新建3377配置项目，需要的额外配置：

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
<!--nacos-discovery 注册中心-服务发现与注册-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

- yaml：

bootstrap.yml：

```yaml
#全局配置
server:
  port: 3377

spring:
  application:
    name: nacos-config-client
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #注册到nacos服务注册中心
      config:
        server-addr: localhost:8848 #Nacos作为配置中心
        file-extension: yaml #指定yaml格式的配置
        
```

application.yml：

```yaml
spring:
  profiles:
    active: dev #表示开发环境
```

上面两个配置分开，就可以通过切换本地的active去切换本个module的配置，十分好用嗷！！！

Controller：

```java
@RestController
@RefreshScope //Nacos也支持动态刷新功能
public class ConfigClientController
{
    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/config/info")
    public String getConfigInfo(){
        return configInfo;
    }
}
```

- 重要的配置！！！

![image-20210811112015207](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811112015.png)

最后公式：

`${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}`

上面这些值都是从配置文件中摘出来的嗷！！！

nacos-config-client-dev.yaml，这个文件名（公用的配置文件）的名字就去欸的那个了嗷！！！

![image-20210811112402300](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811112403.png)

上面这个yaml中，**config后面一定要有一个空格！！！**

- 小总结：![image-20210811112638422](C:\Users\Alexander\AppData\Roaming\Typora\typora-user-images\image-20210811112638422.png)
- ![image-20210811112655365](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811112656.png)



- 报错了，文件名字后面没有对上！！！记住哈，这里文件后缀要求是yaml嗷！！！这是个很大的bug嗷！！！
- 此外，动态刷新。没有上面的config那么痛苦，直接改Nacos里面的文件，动态刷新就会自动生效嗷！！！



#### 分组配置：

- Config + Bus
- 问题：
  - 通常一个系统会准备多个环境，如何保证指定环境启动的时候能够读取到相应环境的配置文件呢？
  - 大型分布式项目，会有很多子项目。每个微服务项目都会有相应的开发环境，测试环境，预发环境，正式环境。如何进行配置管理？
- 图形化管理界面：

![image-20210811123933242](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811123933.png)

Namespace + Group + Data ID三者的关系设计

类似于Java里面的包名和类名

![image-20210811124450061](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811124450.png)

开发，测试，生产就可以创建三个NameSpace



##### DataID

![image-20210811160117549](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811160117.png)

- DataID：
  - 指定spring.profile.active和配置文件的DataID来使不同环境下读取不同的配置。
  - 默认空间+默认分组+新建dev和test两个DataID
- ![image-20210811155440009](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811155447.png)



![image-20210811155552778](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811155552.png)



- 灵活切换，默认空间和默认分组+DataID的不同，就能够实现不同环境之间的切换。
- `active: `的修改，修改完成之后重启就能够实现环境的夏欢嗷！！！



##### GroupID

![image-20210811160216987](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811160217.png)

- 新建两个分组，一个test分组和一个dev分组嗷！

![image-20210811160611235](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811160611.png)

![image-20210811160851892](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811160852.png)

分组了怎么用呢？

- ![image-20210811160914283](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811160914.png)

多加一条关于分组的配置即可嗷！！！

TEST和DEV分别切换，可以查看到不同环境中的同名文件嗷！！！

感觉粒度大，可以实现整个开发环境配置的切换。DataID值能够实现，某个具体的环境中，某个配置文件的切换嗷！！！



##### NameSpace

![image-20210811161925634](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811161925.png)



新增完成之后，配置列表也会发生一些改变了嗷！！！

![image-20210811164432866](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811164433.png)



- 以dev来做一个例子嗷！

![image-20210811164534464](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811164534.png)

![image-20210811165145741](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811165154.png)

配置的时候，需要加上对应的Namespace来进行配置嗷！！！

![image-20210811165656351](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811165656.png)

- 实现了类似于三级目录区分这样的功能嗷！！！



## Nacos集群和持久化配置（重要）：

- 解决问题：
  - 搭建集群
  - 持久化配置
  - Linux版本Nacos + MYSQL生产环境配置

- 架构图：

![image-20210811170206828](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811170207.png)

真实情况：Linux + Nginx + 高可用Mysql集群

![image-20210811170300387](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811170333.png)

Nacos如果不配置高可用的Mysql集群，使用默认的自带数据库，每一个Nacos带一个，自然会有不一致的问题。所以我们采用集中式存储的方法来支持集群化部署，目前只支持MySQL的存储。

- Nacos支持的三种部署模式：
  - 单级：测试和单机使用
  - 集群：生产环境，确保高可用
  - 多集群：多数据中心场景



### 持久化配置：

- Nacos默认自带的是嵌入式数据库derby

- 切换derby到mysql：

  - ![image-20210811171118190](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811171118.png)

  下载的nacos里面自带了mysql的脚本，把它执行一下嗷！！！

  - ![image-20210811171415670](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811171416.png)
  - 官网部署教程，[Nacos部署官网，相应的mysql部署配置](https://nacos.io/zh-cn/docs/deployment.html)
  - 配置完成之后，需要重启Nacos才能够生效嗷！！！注意，由于我的电脑版本的关系，我的电脑不支持Mysql5.x，对于8.x以上的版本可能还需要额外的解决方案，我在尝试换Nacos的版本嗷！！！
  - [Nacos2.0.3安装启动（实操可行）](https://www.cnblogs.com/leslen/p/15128180.html)，注意哈，这里的application中对于mysql要加上时区。然后就是启动2.0.3的时候，默认是集群，要单例启动的话：`startup.cmd -m standalone`

- 使用2.0.3，我们同样对于mysql进行配置，然后新建了一个配置管理，查看数据库中相应的结果：

![image-20210811212136035](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811212143.png)

![image-20210811212228785](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210811212229.png)

Nacos中的数据成功持久化存储到了mysql数据库中嗷！！！







### Nacos生产环境配置：

- 一个Nginx + 3个Nacos注册中心 + 1个mysql
- Nginx集群和Mysql主从复制还要下去自学嗷！！！
- 有点复杂，建议回去看教程嗷！！！个人觉着，Linux的水太深了orz，后面还要复习，要多学嗷！！！





# Sentinel流量控制和熔断组件：

- Hystrix问题：
  - 自己手工搭建监控平台
  - 没有web界面可以给我们更加细粒化的配置来流控，速率控制，服务熔断，服务降级。
- Sentinel：
  - 单独一个组件，可以独立出来
  - 直接界面化的细粒度统一控制。

## 下载安装：

- [Sentinel下载](https://github.com/alibaba/Sentinel/releases)，下载的是默认的一个jar包，java -jar就可以运行嗷！！！8848端口就可以访问到仪表盘，登录的用户名和密码都是sentinel，正常登录就说明没问题嗷！！！

## 实践操作：

- 这里要用Nacos + Sentinel
- Nacos:8848和Sentinel:8080都要先开启嗷！！！
- 8401微服务能够成功注册进8848Nacos并且能够被8080Sentinel保护着。
- yaml：

```yaml
server:
  port: 8401

spring:
  application:
    name: cloudalibaba-sentinel-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #Nacos服务注册中心地址
    sentinel:
      transport:
        dashboard: localhost:8080 #配置Sentinel dashboard地址
        port: 8719
#      datasource:
#        ds1:
#          nacos:
#            server-addr: localhost:8848
#            dataId: ${spring.application.name}
#            groupId: DEFAULT_GROUP
#            data-type: json
#            rule-type: flow

management:
  endpoints:
    web:
      exposure:
        include: '*'
```

- controller：

```java
@RestController
public class FlowLimitController {
    @GetMapping("/testA")
    public String testA(){
        return "------testA";
    }

    @GetMapping("/testB")
    public String testB(){
        return "-------testB";
    }
}
```

- 开启项目之后，sentinel中没有任何的反应orz。因为sentinel懒加载，必须要访问一个网页之后，才能够监控的到嗷！！！

![image-20210812095127814](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812095128.png)

果然访问了一次之后，就可以监控到啦！！！

### 实时监控：

- 最近一段时间的访问情况嗷！！！

- 第一次访问触发监控，不会被计入在内。后面的访问才会被监控到嗷！！！

### 簇点链路：

![image-20210812095627448](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812095627.png)

可以用于监控调用次数和调用情况

后面的四个东西还有用嗷，类似于熔断之类的。

### 流控规则：

- 流控的两个入口：

![image-20210812100257031](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812100257.png)

![image-20210812100326440](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812100326.png)

注意这里的/testA之类的时唯一的，类似于访问的资源名字

- 配置实例：

![image-20210812100436331](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812100436.png)

/testA这个资源路径的每秒请求数大于1这个阈值的时候，就会直接快速失败。

![image-20210812100631293](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812100631.png)

系统默认的保护嗷！！！

- Sentinal中的Dashboard流控模式思考：

现配现用，立即生效，不用重启！！

思考？我们想自定义兜底方法呢？如何自定义？别人帮我们做好了，最大的问题就是，我们的自定义？？？

以后思考的维度：1. 默认有无 ， 2. 可否自定义。

- 配置实例2：
  - QPS：每秒请求数量达到阈值限流
  - 线程数：请求线程数达到阈值限流

线程数的效果：

![image-20210812102059585](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812102059.png)

一个是在外面就限流了，另外一个是将线程放进来了，但是我只能处理一个线程，其他的线程排好队，一个一个来。这就是QPS和线程的不同嗷！！！（说白了就是同时访问的线程数的限制呗！！！）

#### 流控模式：

![image-20210812102358132](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812102358.png)

##### 直接：

自己停用自己，自己满足一定条件就把自己换掉嗷！！！

##### 关联：

![image-20210812102451986](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812102452.png)

作用：类似于支付订单到阈值的时候，就限流下订单的接口，防止连坐的效应。

![image-20210812102617824](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812102618.png)

这里是对于资源A设置的限流，B如果超过了单机阈值，就会限流A嗷！！！

这里可以使用Postman来进行模拟嗷！！！用Postman密集访问B，看是否A能够访问。

![image-20210812110625882](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812110626.png)

Postman中，要新建一个Collection并且把对应的GET请求设置好，才能够批量进行测试嗷！！！

![image-20210812110928075](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812110928.png)

20个线程，每间隔0.3秒去访问一次。然后在运行过程中去访问A嗷，就出不来了orz。

##### 链路：

- 限制当前两个服务之间的流量，一个爆了，另外一个也会跟着爆了orz。



#### 流控效果：

##### 快速失败：

- 快速失败，默认的流控处理就是这个。直接上来就是阈值，难顶啊！！！

##### 预热：

![image-20210812111658394](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812111658.png)

- 没有预热，从0直接到阈值，系统可能会裂开哦orz。

- 阈值/coldFactor(默认为3)，经过预热时长后才会达到阈值。

![image-20210812111843371](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812111843.png)

阈值是10，因子为3，要3.x秒，我给了你5s的时间，多让系统缓缓嗷！！！阈值开始为3，5s的时间慢慢增加到10嗷！！！

![image-20210812112104081](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812112104.png)

冷加载因子是在源码中写了的嗷！！！

刚开始不顺，后面越来越快嗷！！！秒杀系统中也经常用嗷！！！

##### 排队等待：

匀速排队通过

- 另外，排队等待这个只适配QPS嗷！！！

![image-20210812112459626](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812112459.png)



![image-20210812112549693](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812112550.png)



这个十分好用嗷，处于双赢的地位。你发了请求，排队处理，一个接一个。



### 降级规则：

#### RT：

原理：

![image-20210812123159699](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812123159.png)

操作界面：

![image-20210812113129339](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812113129.png)



- RT，平均响应时间（秒级）

平均响应时间超出阈值或在窗口时间内通过的请求>=5，两个条件同时满足才能够触发降级。

RT最大时4900，更大需要-Dcsp.sentinel.statistic.max.rt=XXXX才能够生效。

![image-20210812113749692](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812113749.png)

Sentinel熔断器是没有半开状态的嗷！！！

![image-20210812122725824](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812122726.png)

RT为平均响应时间，单位为毫秒，在1s内，如果无法保证平均响应时间小于200ms，就熔断啦！！！

![image-20210812123104022](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812123104.png)

- 降级策略：

![image-20210812123611564](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812123611.png)

#### 异常比例：

![image-20210812123524086](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812123524.png)

- 降级策略：

![image-20210812123709418](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812123709.png)

![image-20210812123917728](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812123918.png)



#### 异常数：

- 时间窗口一定要大于等于60秒嗷！！！这个东西是按照分钟来进行统计的嗷！！！
- ![image-20210812124142283](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812124142.png)

61s内，异常数大于5次之后，就会产生熔断嗷！！！

### 热点规则：

- 非常重要嗷！！！
- 用于热点参数限流嗷！！！（对于某一个参数进行更加精确的打击和限流嗷！！！）



#### @SentinelResource：

- 新增的测试controller：

```java
/**
 * @param p1
 * @param p2
 * @return
 */
@GetMapping("/testHotKey")
@SentinelResource(value = "testHotKey", blockHandler = "deal_testHotKey")
public String testHotKey(@RequestParam(value = "p1", required = false) String p1,
                         @RequestParam(value = "p2", required = false) String p2) {
    int age = 10 / 0;
    return "------testHotKey";
}

public String deal_testHotKey(String p1, String p2, BlockException exception) {
    //sentinel系统默认的提示：Blocked by Sentinel (flow limiting)
    return "------deal_testHotKey,o(╥﹏╥)o";
}
```

- @SentinelResource的用法：

注意，SentinelResource后面的value的值只要是唯一的就行嗷，建议和getMapping保持一致（少个/），方便区分和规范，下面的这个是兜底方法嗷！！！用法和@HystrixCommand没啥区别嗷！！！

- 下面例如，我们对于p1这个参数进行拦截：

![image-20210812162529887](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812162530.png)

这里设置下来的就是指的是参数p1哈，p1和p2是完全独立的，p2随便取啥都无所谓嗷！！！注意一下这里的资源名，资源名是你@SentinelResource中给的value值嗷！！！

当请求参数带有p1并且请求的频率高于1秒1次的时候，就采用了服务降级的方法：

![image-20210812163121337](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812163121.png)

- 如果SentinelResource后面那个blockHandler的参数去掉了，那就没用了呀orz。超过p1限定的频率之后，会直接报errorPage，就很不友好嗷QAQ！！！



#### 参数例外项：

- 参数例外项：

  - 我们希望当p1的值为某些特定值的时候，不限流！！！（更加灵活）

  ![image-20210812165235415](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812165235.png)

  - 这个下半部分就是对于特殊值的设定嗷，当参数的值取特定的值的时候，限流阈值会不一样嗷！！！所以这个才叫作参数例外项哇~

- 手动添加错误试试看？
  - Java运行时的错误老子不管哦
  - 我只管运行时的限流，我不管你运行的返回是啥哦！！！
  - 主管配置出错，运行出错不归我管，该走异常走异常。
- 那发生了错误不友好怎么办啊？@SentinelResource中有一个属性叫做fallback，也可以和之前一样指定方法，如果出错了该怎么处理嗷！！！



#### 系统规则：

- 这个是对于整个系统的大粒度的控制嗷！！！总包总揽，入口级别的控制嗷，整体控制！！！

- 自适应系统规则，在整个系统的最外层多包了一层嗷！！！但是粒度有点大，不是特别方便嗷！！！

![image-20210812173536871](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812173537.png)

例如入口QPS，设置为2，每秒最多2个，再多的就不行了！！！再多的无论你调用系统中的什么服务都进不来了嗷！！！



### @SentinelResource：

#### 按资源名称限流+后续处理：

- 按照@Sentinel中的资源名限流，如果有自定义的兜底，用自定义的。如果没有，用系统默认的。

- 第一个Controller中的方法：

```java
@GetMapping("/byResource")
@SentinelResource(value = "byResource", blockHandler = "handleException")
public CommonResult byResource() {
    return new CommonResult(200, "按资源名称限流测试OK", new Payment(2020L, "serial001"));
}

public CommonResult handleException(BlockException exception) {
    return new CommonResult(444, exception.getClass().getCanonicalName() + "\t 服务不可用");
}
```

可以看出来，兜底方法中的参数嗷！！！和上面的放一起看，就知道传参的规则是啥了嗷！！！

![image-20210812174639060](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812174639.png)

注意这里的资源名称哈，配置的时候的资源名用的是@SentinelResource中的资源名嗷！！！（如果用GetMapping也不是不行，但是那样就用不了我们自定义的到底规则嗷！！！）

- 额外问题：关闭服务8401看看？
  - Sentinel流控规则消失，说明流控规则的配置是零时的配置嗷！！！



#### 按Url地址限流+后续处理：

- 按照@GetMapping中的地址限流，使用Sentinel默认的兜底规则进行处理。

```java
@GetMapping("/rateLimit/byUrl")
@SentinelResource(value = "byUrl")
public CommonResult byUrl() {
    return new CommonResult(200, "按url限流测试OK", new Payment(2020L, "serial002"));
}
```



#### 兜底方案面临的问题：

- 和前面一样的，兜底方案和代码耦合
- 兜底方案太多太杂
- 代码膨胀
- 没有全局统一处理方法



#### 以上问题的解决：

- 和Hystrix一样的，单独建立一个类来存储降级方案：

```java
public class CustomerBlockHandler {
    public static CommonResult handlerException(BlockException exception) {
        return new CommonResult(4444, "按客戶自定义,global handlerException----1");
    }


    public static CommonResult handlerException2(BlockException exception) {
        return new CommonResult(4444, "按客戶自定义,global handlerException----2");
    }
}
```

- 配置：

```java
@GetMapping("/rateLimit/customerBlockHandler")
@SentinelResource(value = "customerBlockHandler",
        blockHandlerClass = CustomerBlockHandler.class,
        blockHandler = "handlerException2")
public CommonResult customerBlockHandler() {
    return new CommonResult(200, "按客戶自定义", new Payment(2020L, "serial003"));
}
```

相当于多了一个参数来引入对应的类而已嗷！！！

![image-20210812192927117](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812192927.png)





#### 代码配置：

![image-20210812193303536](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812193303.png)

不推荐嗷，有一点复杂呢。有GUI不是用起来配置更加舒服咩？

- 注解是不支持private的方法嗷！！！
- 三个核心API：
  - SphU定义资源
  - Tracer定义统计
  - ContextUtil定义上下文



### 服务熔断：

- sentinel整合ribbon + openFeign + fallback

#### Ribbon系列：

- 84 ， 9003 ， 9004
- ![image-20210812202448414](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812202449.png)
- 正常调用访问没有任何问题嗷！！！
- Sentinel只对于84客户端进行监控嗷（为了实验方便呀！！！）
- SentinelResource中的其他参数详解：
  - fallback：这个参数指定了Java运行时异常时的处理方法，相当于Hystrix中的服务降级！！！
  - blockHandler：这个参数只负责sentinel控制台配置违规的降级处理嗷！！！
  - 上面两个都有的时候，Java异常处理也是属于流水哦！！！当同时满足blockHandler和fallback的时候，blockHandler更猛！！！（本质Java异常处理也算一次流水，我直接截停你的流水，把你拦在门外，那你当然连错都不可能犯嗷！！！）
  - exceptionsToIgnore：忽略某个Java异常，在Java运行异常时正常抛出这个异常，不用兜底方法处理，让它自生自灭嗷！！！
- 综上，功能十分强大，参数都配上，功能和性能都十分强大啊！！！



#### Feign系列：

- Feign除了引入依赖之外，还要激活Sentinel对于Feign的支持嗷：

```yaml
feign: 
	sentinel:
		enable: true
```

- 客户端主启动类上要加上@EnableFeignClients嗷！！！
- 客户端的Service接口上要加上@FeignClient

![](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210813093800.png)

![image-20210813092248411](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210813092248.png)

@Component和@FeignClient不要忘了嗷！



### 规则持久化：

- 重启Sentinel服务或业务类，规则就消失了orz，裂开来！！！

- 可以将Sentinel配置进Nacos来实现持久化嗷！！！

- 步骤（以8401为例子）：

  - 加入sentinel-datasource-nacos依赖
  - yaml中添加数据源的配置：

  ```yaml
  datasource:
    ds1:
      nacos:
        server-addr: localhost:8848
        dataId: ${spring.application.name}
        groupId: DEFAULT_GROUP
        data-type: json
        rule-type: flow
  ```

  - 添加Nacos的配置：

  ![image-20210813095716810](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210813095717.png)

JSON中的参数含义：

![image-20210813095851897](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210813095852.png)

- 启动8401之后，发现就有流控规则嗷！

![image-20210813100238836](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210813100239.png)

![image-20210813100343838](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210813100344.png)

- 就是由于上面这个的配置，让sentinel会去nacos取出对应配置名的项作为流控规则啊！！！
- 重启8401发现，流控规则还是存在的嗷！！！！！说白了就是将配置规则写入Nacos，间接写入Mysql，然后Sentinel每次从Nacos中去取出来呗！！！





# Seata分布式事务：

- 多数据源，多中心，跨数据库处理。要处理全局事务一致性问题嗷！！！
- 专业术语：
- 1+3：
- 1
  - Transaction ID XID，全局唯一的ID
- 3
    - TC (Transaction Coordinator) - 事务协调者，维护全局和分支事务的状态，驱动全局事务提交或回滚。
  - TM (Transaction Manager) - 事务管理器，定义全局事务的范围：开始全局事务、提交或回滚全局事务。
    - RM (Resource Manager) - 资源管理器，管理分支事务处理的资源，与TC交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚。
- ![image-20210813111713201](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210813111722.png)



## 安装：

- 进入seata官网，找到对应的github页面，进入releases，下载seata-server（第二排看戏原则）
- 修改原始的file.conf文件（良好习惯，先备份，后修改嗷！！！）

![image-20210813153355897](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210813153356.png)

建立的数据库是需要脚本的嗷！！！看课程，并且在官网的scripts中找到mysql的脚本，在mysql中初始化对应的表才能够连的上嗷！！！

- 1.4.2新版本的配置文件分开了，file.conf和registery.conf各自拥有一部分的配置。
- file.conf需要修改数据库的连接（前提是数据库已经完成了相应的初始化）
- registry.conf里面有关于config和registry的配置。registry改成nacos，我们使用nacos来操作嗷！！！

## 实践：

- 创建三个微服务：
  - 订单
  - 库存
  - 账户
- 下订单 ---> 减余额 ---> 扣库存 ---> 改订单
- 要为三个微服务创建自己对应的微服务和微服务中的表嗷！！！
- 注意：电脑上下载了0.9.0和1.4.2两个版本，这两个版本有很大的问题，配置上非常多的不一样，这里记录一下自己踩的坑。

### 天坑：

0.9.0：

- 由于0.9.0的Lib中的mysql-connector的版本是5.x，我本机使用的是8.x，所以要手动移除5.x的jar包，添加8.x的jar包。
- 注意Mysql的8.x，在工程中使用的依赖是com.mysql.cj.jdbc.Driver，要引入这个Jar包嗷（默认就在connector依赖里面有这个Jar包）
- 1.4.2版本建立的表（官网给的）和0.9.0的Seata数据库的建表语句是**不一样的**！！！debug了整整一个上午，就是因为表不一样，找不到某个字段，把表重新建立一次就好。
- 不管是0.9.0还是1.4.2，启动的时候都会报Logback异常，这个没有关系，对于系统的运行没有影响。
- 一定要先启动Nacos，再启动Logback。
- 1.4.2和0.9.0相比，少了很多字段，具体缺了哪些，该怎么用，下去还要自己多看看官方文档学习嗷！！！
- 遇到了socket异常的问题，还有紧接着seata异常的问题，不知道怎么解决，晚上重启电脑之后自己好了？？？？注意嗷！！！重启是可以解决问题的嗷！！！
- @GlobalTransaction帮助我们撤销微服务有问题的操作嗷！！！保证整个微服务系统的一致性！！！
- 这一章很难，很多细节，复习建议看看代码和网课嗷！！！



## Seata原理（面试加分项）：

1. Seata：

- 1.0版本及以上才支持集群嗷！！！工作以上就要使用使用1.0版本及以上的Seata啦！！！
- [多看视频嗷！！！，这个讲的很好嗷！！！](https://www.bilibili.com/video/BV18E411x7eT?p=148)
- [官网介绍，涉及AT模式原理](https://seata.io/zh-cn/docs/dev/mode/at-mode.html)












