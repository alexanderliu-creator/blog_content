---
title: SpringBoot2-x
date: 2021-07-13 09:32:16
tags: 后端
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721174421.png
---

# 针对于Boot2的新特性，有了这一期的学习，连着两次学习boot，刚好巩固基础了嗷！！！尚硅谷还是香哇！！！



# 基础使用篇：

## Boot的升级：

- 响应式编程：

![image-20210713090303630](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210713090303.png)

- 接口中方法有默认实现，就不用把接口中的所有方法都写出来了，都有默认实现的，只用覆写我们要覆写的方法即可。（Java底层语法的升级，导致Adapter失效了，每个接口类中的抽象方法现在都是自带默认实现的嗷！！！我们直接对于需要覆写的方法进行覆写就可以啦！！！）





## 一些补充：

`@RestController = @Controller + @ResponseBody`

- @ResponseBody是将相应的内容直接以字符串的形式写到浏览器中嗷！！！就不会发生页面跳转啦！！！
- 相应的资料：
  - [SpringBoot2课程笔记](https://www.yuque.com/atguigu/springboot)
  - [boot官方文档怎么使用](https://www.bilibili.com/video/BV19K4y1L7MT/?p=4&spm_id_from=pageDriver)
  - [SpringBoot官网](https://spring.io/projects/spring-boot)

- 自定义版本号：

1、查看spring-boot-dependencies里面规定当前依赖的版本 用的 key。 

2、在当前项目里面重写配置：

```properties
<properties>
    <mysql.version>5.1.43</mysql.version>
</properties>
```



- boot中的默认包结构配置：
  - 自动配好SpringMVC
    - 引入SpringMVC全套组件
    - 自动配好SpringMVC常用组件（功能）
  - 自动配好Web常见功能，如：字符编码问题
    - SpringBoot帮我们配置好了所有web开发的常见场景
  - 默认的包结构
    - 主程序所在包及其下面的所有子包里面的组件都会被默认扫描进来
  - 无需以前的包扫描配置
  - 想要改变扫描路径，@SpringBootApplication(scanBasePackages=**"com.tuzi"**)
    - 或者@ComponentScan 指定扫描路径



```java
@SpringBootApplication
等同于
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.atguigu.boot")
```

- AutoConfigurer就是我们核心的自动配置包，只有引入对应的场景，对应的自动配置类才会生效嗷！！！



## 注解使用：

### @Configuration

- 这个配置类就相当于原来的xml配置文件，这个就告诉Boot这是一个配置类。
- 原来的配置类（xml）是配置好了，Spring自动给你创建好对象。现在你用配置类来配置，那当然也要手动去创建对象然后加入到容器中去啊！！！

```java
#############################Configuration使用示例######################################################
/**
 * 1、配置类里面使用@Bean标注在方法上给容器注册组件，默认也是单实例的
 * 2、配置类本身也是组件
 * 3、proxyBeanMethods：代理bean的方法
 *      Full(proxyBeanMethods = true)、【保证每个@Bean方法被调用多少次返回的组件都是单实例的】
 *      Lite(proxyBeanMethods = false)【每个@Bean方法被调用多少次返回的组件都是新创建的】
 *      组件依赖必须使用Full模式默认。其他默认是否Lite模式
 */
@Configuration(proxyBeanMethods = false) //告诉SpringBoot这是一个配置类 == 配置文件
public class MyConfig {

    /**
     * Full:外部无论对配置类中的这个组件注册方法调用多少次获取的都是之前注册容器中的单实例对象
     * @return
     */
    @Bean //给容器中添加组件。以方法名作为组件的id。返回类型就是组件类型。返回的值，就是组件在容器中的实例
    public User user01(){
        User zhangsan = new User("zhangsan", 18);
        //user组件依赖了Pet组件
        //但是如果标注了false的话，调用tomcatPet就会新建一个tomcatPet，而不是使用容器中的tomcatPet
        zhangsan.setPet(tomcatPet());
        return zhangsan;
    }

    @Bean("tom") //tom可以给我们添加到容器中的组件命名嗷！！！
    public Pet tomcatPet(){
        return new Pet("tomcat");
    }
}
```
以前是指定<bean id="..." class="..."\>，然后里面设置属性，现在就是手动去构造这个类，然后通过@Bean注解将我们构造的这个对象（返回值），来添加到容器中嗷！！！



```java

################################@Configuration测试代码如下########################################
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.atguigu.boot")
public class MainApplication {

    public static void main(String[] args) {
        //1、返回我们IOC容器
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);

        //2、查看容器里面的组件
        String[] names = run.getBeanDefinitionNames();
        for (String name : names) {
            System.out.println(name);
        }

        //3、从容器中获取组件
        Pet tom01 = run.getBean("tom", Pet.class);

        Pet tom02 = run.getBean("tom", Pet.class);

        System.out.println("组件："+(tom01 == tom02));


        //4、com.atguigu.boot.config.MyConfig$$EnhancerBySpringCGLIB$$51f1e1ca@1654a892
        MyConfig bean = run.getBean(MyConfig.class);
        System.out.println(bean);

        //如果@Configuration(proxyBeanMethods = true)代理对象调用方法。SpringBoot总会检查这个组件是否在容器中有。
        //保持组件单实例
        User user = bean.user01();
        User user1 = bean.user01();
        System.out.println(user == user1);


        User user01 = run.getBean("user01", User.class);
        Pet tom = run.getBean("tom", Pet.class);

        System.out.println("用户的宠物："+(user01.getPet() == tom));



    }
}
```

- true又叫做lite模式，如果容器中有，就从容器中将对应的对象拿出来，就可能会产生依赖关系。false的话又被称为full模式，每一次都会生成一个新的对象，就不会产生依赖关系。













### @Import

- 可以使用在自定的MyConfig文件或其他能被扫描到的位置都可以嗷

![image-20210713192105621](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210713192105.png)

- @Import导入的默认组件的名字是全类名嗷！！！

- 主方法中可以通过下面的代码进行测试：

![image-20210713192350950](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210713192351.png)

- 第一个是获取获取特定类型的所有组件，第二个是获取对应类型的特定组件。



### @Conditional

- 条件装配

- 满足条件的时候，才能够引入其后面跟着的类嗷！！！
- 它和儿子们：

![image-20210713192708656](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210713192708.png)

上面这些都是@Conditional和它延申出来的实现类嗷！！！

- 例如OnBean，有某个组件的时候，才干什么行为。
- run.containsBean()能够判断容器中是否有某个组件，根据组件名字去获取，返回值是boolean嗷！！！
- `@ConditionalOnBean(name = "tom")`这种形式是用来扫描容器中是否有名字为tom的组件。注解可以注释到某个方法或某个类上，只有判断成立，类中或者方法中相应的配置才会生效嗷！！！
- @Bean这个始终要有的哈，没有@Bean，就算条件装配也救不了你嗷！！！



### @ImportResource

- 导入对应的资源配置。（@ImportResource("classpath: beans.xml")）
- 这个注解还允许我们使用之前的xml配置，导入了对应的路径之后，这个配置就生效了嗷！！！就能够根据我们的xml以之前的形式生成对应bean，这个注解放在扫描类的最外面注解就行啦！！！





### 属性自动配置（@ComfigurationProperties）：

#### @Component + @ConfigurationProperties

- 如果单独使用@ConfigurationProperites的话，要和@Component捆绑使用嗷！！！

- 使用场景是我们之前的配置文件绑定到对应的对象，我们当时要写一个类。

```java
public class getProperties {
     public static void main(String[] args) throws FileNotFoundException, IOException {
         Properties pps = new Properties();
         pps.load(new FileInputStream("a.properties"));
         Enumeration enum1 = pps.propertyNames();//得到配置文件的名字
         while(enum1.hasMoreElements()) {
             String strKey = (String) enum1.nextElement();
             String strValue = pps.getProperty(strKey);
             System.out.println(strKey + "=" + strValue);
             //封装到JavaBean。
         }
     }
 }
```

- 上面这个就是我们之前写的，需要自己去解析配置文件，然后对应好相应的属性，封装到JavaBean中嗷！！！（有点复杂）
- ConfigurationProperties就是帮你完成这个的嗷！！！ConfigurationProperties的例子嗷！！！

```java
/**
 * 只有在容器中的组件，才会拥有SpringBoot提供的强大功能
 */
@Component
@ConfigurationProperties(prefix = "mycar")
public class Car {

    private String brand;
    private Integer price;

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public Integer getPrice() {
        return price;
    }

    public void setPrice(Integer price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", price=" + price +
                '}';
    }
}
```

- 标注了Prefix之后，这个类的配置就和我们的application.properties(.yml)绑定上了嗷！！！我们就可以通过在配置文件中配置相应的属性，来对于对应的类进行配置。
- application.properities中：

```properties
mycar.brand=tesla
mycar.price=300000
```

- 注意，由于@Component，这个时候组件已经在容器中了。是可以通过@Autowired来获取对应的组件的嗷！！！



#### @EnableConfigurationProperties + @ConfigurationProperties

- 必须在配置类中写或者在启动类中写
- @EnableConfigurationProperties（Car.class）
  - 它的作用：
    - 开启Car的属性配置功能
    - 并且把Car这个组件自动注册到容器中（这个时候就不用在对应的类上写@Component啦！！！）：
      - 解决了有的时候，别人的类是第三方写好的，上面改不了，加不了@Component注解这个问题嗷！！！



## 自动配置原理：

- 一顶三（三个核心注解）：

![image-20210713201014773](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210713201014.png)



### @SpringBootConfiguration:

![image-20210713201128058](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210713201128.png)

- 本质上就是一个@Configuration，代表当前类是一个配置类。



### @EnableAutoConfiguration(最重要)

- 由下面两个注解合成：

```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```

- @AutoConfigurationPackage:

  - 由下面的@组成：

  ```java
  @Import(AutoConfigurationPackages.Registrar.class)  //给容器中导入一个组件
  public @interface AutoConfigurationPackage {}
  
  //利用Registrar给容器中导入一系列组件
  //将指定的一个包下的所有组件导入进来？MainApplication 所在包下。
  ```

  - 自动配置包
  - 利用Registrar给容器中导入一系列组件(这里体现了，为什么主启动类同一层级所在的包和子包都能够被Boot识别和导入嗷！！！)
  - 由于ACP -> EAC -> SBC，因此，SBC即主配置类即AutoConfigurationPackage真正标注的地方，这个地方才导向到了我们所谓的“主配置所在层级的包，以及这个包以下的子包！！！”

- @Import({AutoConfigurationImportSelector.class})：

  - ![image-20210713220500529](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210713220500.png)

  - 上面的AutoConfigurationImportSelector的核心方法selectImports中的核心方法orz。

  - ```java
    1、利用getAutoConfigurationEntry(annotationMetadata);给容器中批量导入一些组件
    2、调用List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes)获取到所有需要导入到容器中的配置类
    3、利用工厂加载 Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader)；得到所有的组件
    4、从META-INF/spring.factories位置来加载一个文件。
    默认扫描我们当前系统里面所有META-INF/spring.factories位置的文件
    spring-boot-autoconfigure-2.3.4.RELEASE.jar包里面也有META-INF/spring.factories
    ```

  - getCandidateConfigurations这个方法很重要，获得了我们的configurations，如下图所示嗷：![image-20210713220752294](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210713220752.png)

  - ```xml
    文件里面写死了spring-boot一启动就要给容器中加载的所有配置类
    spring-boot-autoconfigure-2.3.4.RELEASE.jar/META-INF/spring.factories
    # Auto Configure
    org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
    org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
    org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
    org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
    org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
    org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
    org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
    org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
    org.springframework.boot.autoconfigure.context.LifecycleAutoConfiguration,\
    org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
    org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
    org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
    org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.cassandra.CassandraDataAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveDataAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.cassandra.CassandraRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.couchbase.CouchbaseDataAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveDataAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.couchbase.CouchbaseRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchDataAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRestClientAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.jdbc.JdbcRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.mongo.MongoReactiveDataAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.mongo.MongoReactiveRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.neo4j.Neo4jRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.solr.SolrRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.r2dbc.R2dbcDataAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.r2dbc.R2dbcRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.r2dbc.R2dbcTransactionManagerAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.redis.RedisReactiveAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.rest.RepositoryRestMvcAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.web.SpringDataWebAutoConfiguration,\
    org.springframework.boot.autoconfigure.elasticsearch.ElasticsearchRestClientAutoConfiguration,\
    org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration,\
    org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfiguration,\
    org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAutoConfiguration,\
    org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration,\
    org.springframework.boot.autoconfigure.h2.H2ConsoleAutoConfiguration,\
    org.springframework.boot.autoconfigure.hateoas.HypermediaAutoConfiguration,\
    org.springframework.boot.autoconfigure.hazelcast.HazelcastAutoConfiguration,\
    org.springframework.boot.autoconfigure.hazelcast.HazelcastJpaDependencyAutoConfiguration,\
    org.springframework.boot.autoconfigure.http.HttpMessageConvertersAutoConfiguration,\
    org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration,\
    org.springframework.boot.autoconfigure.influx.InfluxDbAutoConfiguration,\
    org.springframework.boot.autoconfigure.info.ProjectInfoAutoConfiguration,\
    org.springframework.boot.autoconfigure.integration.IntegrationAutoConfiguration,\
    org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration,\
    org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
    org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration,\
    org.springframework.boot.autoconfigure.jdbc.JndiDataSourceAutoConfiguration,\
    org.springframework.boot.autoconfigure.jdbc.XADataSourceAutoConfiguration,\
    org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
    org.springframework.boot.autoconfigure.jms.JmsAutoConfiguration,\
    org.springframework.boot.autoconfigure.jmx.JmxAutoConfiguration,\
    org.springframework.boot.autoconfigure.jms.JndiConnectionFactoryAutoConfiguration,\
    org.springframework.boot.autoconfigure.jms.activemq.ActiveMQAutoConfiguration,\
    org.springframework.boot.autoconfigure.jms.artemis.ArtemisAutoConfiguration,\
    org.springframework.boot.autoconfigure.jersey.JerseyAutoConfiguration,\
    org.springframework.boot.autoconfigure.jooq.JooqAutoConfiguration,\
    org.springframework.boot.autoconfigure.jsonb.JsonbAutoConfiguration,\
    org.springframework.boot.autoconfigure.kafka.KafkaAutoConfiguration,\
    org.springframework.boot.autoconfigure.availability.ApplicationAvailabilityAutoConfiguration,\
    org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration,\
    org.springframework.boot.autoconfigure.ldap.LdapAutoConfiguration,\
    org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration,\
    org.springframework.boot.autoconfigure.mail.MailSenderAutoConfiguration,\
    org.springframework.boot.autoconfigure.mail.MailSenderValidatorAutoConfiguration,\
    org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration,\
    org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,\
    org.springframework.boot.autoconfigure.mongo.MongoReactiveAutoConfiguration,\
    org.springframework.boot.autoconfigure.mustache.MustacheAutoConfiguration,\
    org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
    org.springframework.boot.autoconfigure.quartz.QuartzAutoConfiguration,\
    org.springframework.boot.autoconfigure.r2dbc.R2dbcAutoConfiguration,\
    org.springframework.boot.autoconfigure.rsocket.RSocketMessagingAutoConfiguration,\
    org.springframework.boot.autoconfigure.rsocket.RSocketRequesterAutoConfiguration,\
    org.springframework.boot.autoconfigure.rsocket.RSocketServerAutoConfiguration,\
    org.springframework.boot.autoconfigure.rsocket.RSocketStrategiesAutoConfiguration,\
    org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration,\
    org.springframework.boot.autoconfigure.security.servlet.UserDetailsServiceAutoConfiguration,\
    org.springframework.boot.autoconfigure.security.servlet.SecurityFilterAutoConfiguration,\
    org.springframework.boot.autoconfigure.security.reactive.ReactiveSecurityAutoConfiguration,\
    org.springframework.boot.autoconfigure.security.reactive.ReactiveUserDetailsServiceAutoConfiguration,\
    org.springframework.boot.autoconfigure.security.rsocket.RSocketSecurityAutoConfiguration,\
    org.springframework.boot.autoconfigure.security.saml2.Saml2RelyingPartyAutoConfiguration,\
    org.springframework.boot.autoconfigure.sendgrid.SendGridAutoConfiguration,\
    org.springframework.boot.autoconfigure.session.SessionAutoConfiguration,\
    org.springframework.boot.autoconfigure.security.oauth2.client.servlet.OAuth2ClientAutoConfiguration,\
    org.springframework.boot.autoconfigure.security.oauth2.client.reactive.ReactiveOAuth2ClientAutoConfiguration,\
    org.springframework.boot.autoconfigure.security.oauth2.resource.servlet.OAuth2ResourceServerAutoConfiguration,\
    org.springframework.boot.autoconfigure.security.oauth2.resource.reactive.ReactiveOAuth2ResourceServerAutoConfiguration,\
    org.springframework.boot.autoconfigure.solr.SolrAutoConfiguration,\
    org.springframework.boot.autoconfigure.task.TaskExecutionAutoConfiguration,\
    org.springframework.boot.autoconfigure.task.TaskSchedulingAutoConfiguration,\
    org.springframework.boot.autoconfigure.thymeleaf.ThymeleafAutoConfiguration,\
    org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration,\
    org.springframework.boot.autoconfigure.transaction.jta.JtaAutoConfiguration,\
    org.springframework.boot.autoconfigure.validation.ValidationAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.client.RestTemplateAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.embedded.EmbeddedWebServerFactoryCustomizerAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.reactive.HttpHandlerAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.reactive.ReactiveWebServerFactoryAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.reactive.WebFluxAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.reactive.error.ErrorWebFluxAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration,\
    org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
    org.springframework.boot.autoconfigure.websocket.reactive.WebSocketReactiveAutoConfiguration,\
    org.springframework.boot.autoconfigure.websocket.servlet.WebSocketServletAutoConfiguration,\
    org.springframework.boot.autoconfigure.websocket.servlet.WebSocketMessagingAutoConfiguration,\
    org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration,\
    org.springframework.boot.autoconfigure.webservices.client.WebServiceTemplateAutoConfiguration
    ```

- 开启自动配置项：

```java
虽然我们127个场景的所有自动配置启动的时候默认全部加载。xxxxAutoConfiguration
按照条件装配规则（@Conditional），最终会按需配置。
```

- 所以，启动先加载所有的所有的配置到Configuration变量中，最终生效是由条件配置来决定的嗷！！！          



### @ComponentScan("com.tuzi.boot")

- 对于扫描父包路径进行指定！！！（指定扫描某些package），从中获取Bean（类似于MyConfig类中的组件），记得这个是Scan to get Components！！！





### Skills：

- 回头看配置类是否能够生效，找到对应的包下的autoconfigurer文件夹，找到对应的功能（例如cache）和其对应的自动配置类（例如CacheAutoConfiguration），然后去看起的了作用不？
- [这一课讲的非常好，探索底层嗷！！！](https://www.bilibili.com/video/BV19K4y1L7MT?p=15&spm_id_from=pageDriver)
- 特殊语法：

```java
@Bean
@ConditionalOnBean(MultipartResolver.class)  //容器中有这个类型组件
@ConditionalOnMissingBean(name = DispatcherServlet.MULTIPART_RESOLVER_BEAN_NAME) //容器中没有这个名字 multipartResolver 的组件
public MultipartResolver multipartResolver(MultipartResolver resolver) {
    //给@Bean标注的方法传入了对象参数，这个参数的值就会从容器中找。
    //SpringMVC multipartResolver。防止有些用户配置的文件上传解析器不符合规范
    // Detect if the user has created a MultipartResolver but named it incorrectly
    return resolver;
}
//给容器中加入了文件上传解析器，相当于更名嗷！！！
```

- 设计模式：

  - 通过@ConditionalOnMissingBean这种方式，来实现会在底层配好所有的组件，但是如果用户自己配置了的话，以用户的优先。
  - 那用户怎么配置呢？那简单啊，你在用户的配置类中（标注有@Confuration），使用@Bean来向容器中加入我自己配置的组件，容器就不会配置默认的组件了嗷！！！

  ```java
  @Bean
  @ConditionalOnMissingBean
  public CharacterEncodingFilter characterEncodingFilter() {
  }
  ```





- 总结：
  - SpringBoot先加载所有的自动配置类  xxxxxAutoConfiguration
  - 每个自动配置类按照条件进行生效，默认都会绑定配置文件指定的值。xxxxProperties里面拿。xxxProperties和配置文件进行了绑定
  - 生效的配置类就会给容器中装配很多组件
  - 只要容器中有这些组件，相当于这些功能就有了
  - 定制化配置：
    - 用户直接自己@Bean替换底层的组件
    - 用户去看这个组件是获取的配置文件什么值就去修改。

**xxxxxAutoConfiguration ---> 组件  --->** **xxxxProperties里面拿值  ----> application.properties**

两个很有意思的东西：

- 想换掉源码中底层给你配置的某个默认配置类，使用@Bean自己写一个就完了嘛。
- 在Properties中使用绑定配置，所以可以在xxxProperties配置中找到你需要的值，然后在配置文件中去改嗷！！！



## 大彻大悟！！！上面讲的这一串太好了！！！



## 最佳实践：

- 引入场景依赖
  - https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter

- 查看自动配置了哪些（选做）
  - 自己分析，引入场景对应的自动配置一般都生效了
  - 配置文件中debug=true开启自动配置报告。Negative（不生效）\Positive（生效）

- 是否需要修改
  - 参照文档修改配置项
    - https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties
    - 自己分析。xxxxProperties绑定了配置文件的哪些。

- 自定义加入或者替换组件
  - @Bean、@Component......（用户配置优先）

- 自定义器  **XXXXXCustomizer**；
- ......



# 开发小技巧：

## Lombok：

- 简化JavaBean的开发：
  - getter and setter
  - constructor
  - tostring
  - 麻烦子！！！

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
<!--idea中搜索安装lombok插件-->
```

- 编译的时候会自动生成对应的setter和getter方法嗷！！！
- 引入了对应的插件之后，@Data这个注解就会自动帮忙在编译的时候生成getter和setter方法嗷。
- 当引入@Data时，原本灰色的属性编程紫色了，说明这个注解引入成功了嗷！！！
- 同理，@ToString就能够帮助我们在编译的时候自动生成ToString方法
- 总结，引入了Maven并且安装了lombok插件后，有如下可用的注解：
  - @Data：getter and setter
  - @ToString：toString method
  - @NoArgsConstructor：无参构造器
  - @AllArgsConstructor：全参构造器
  - @EqualsAndHashCode：哈希码方法
  - @Slf4j：日志构造器，就可以直接使用日志啦！！！
- 根据情况使用，比如全参不好用，那我们就不注解，除此之外，我们仍然可以定制自己的构造器嗷！！！

```java
===============================简化JavaBean开发===================================
@NoArgsConstructor
//@AllArgsConstructor
@Data
@ToString
@EqualsAndHashCode
public class User {

    private String name;
    private Integer age;

    private Pet pet;

    public User(String name,Integer age){
        this.name = name;
        this.age = age;
    }


}



================================简化日志开发===================================
@Slf4j
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String handle01(@RequestParam("name") String name){
        
        log.info("请求进来了....");
        
        return "Hello, Spring Boot 2!"+"你好："+name;
    }
}
```





## dev-tools：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

- `Ctrl+F9`就能够快速实现“热更新”（本质上如果时动态文件就重启，静态页面就刷新嗷！！！）



## Spring Initializer：

- 这个我就熟啦，不介绍啦，经常用嗷！！！
- 引入依赖，创建好了我们的项目结构嗷之类的嗷！！！





# 核心功能篇：

## Yaml：

- 语法：

  ### 1.2.3、数据类型

  

  - 字面量：单个的、不可再分的值。date、boolean、string、number、null

  ```yaml
  k: v
  ```

  - 对象：键值对的集合。map、hash、set、object 

  ```yaml
  行内写法：  k: {k1:v1,k2:v2,k3:v3}
  #或
  k: 
    k1: v1
    k2: v2
    k3: v3
  ```

  - 数组：一组按次序排列的值。array、list、queue

  ```yaml
  行内写法：  k: [v1,v2,v3]
  #或者
  k:
   - v1
   - v2
   - v3
  ```

- 示例：

```java
@Data
@Component
@ConfigurationProperities(prefix="person")
public class Person {
	
	private String userName;
	private Boolean boss;
	private Date birth;
	private Integer age;
	private Pet pet;
	private String[] interests;
	private List<String> animal;
	private Map<String, Object> score;
	private Set<Double> salarys;
	private Map<String, List<Pet>> allPets;
}

@Data
public class Pet {
	private String name;
	private Double weight;
}
```

- yaml和properties配置文件都存在的情况下会同时生效嗷！！！

```yaml
# yaml表示以上对象
person:
  userName: zhangsan
  boss: false
  birth: 2019/12/12 20:12:33
  age: 18
  pet: 
    name: tomcat
    weight: 23.4
  interests: [篮球,游泳]
  animal: 
    - jerry
    - mario
  score:
    english: 
      first: 30
      second: 40
      third: 50
    math: [131,140,148]
    chinese: {first: 128,second: 136}
  salarys: [3999,4999.98,5999.99]
  allPets:
    sick:
      - {name: tom}
      - {name: jerry,weight: 47}
    health: [{name: mario,weight: 47}]
```

- List和String[]和Set在yaml中的配置都是一样的嗷，有行内写法和数组写法两种嗷！！！
- 测试是否书写正确：

![image-20210714110023281](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210714110023.png)

- yaml中不用加单双引号，加了也没事儿，都是字符串嗷！！！但是有\n之类的，双引号就会转义为换行。单引号就不会转义而是选择作为字符串正常输出，有点像shell编程嗷！！！



## 注释处理器：

- 我们自己写的prefix没有提示，就很容易出问题嗷！！！自定义的类和配置文件绑定一般没有提示。

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-configuration-processor</artifactId>
      <optional>true</optional>
  </dependency>
  
  
   <build>
       <plugins>
           <plugin>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-maven-plugin</artifactId>
               <configuration>
                   <excludes>
                       <exclude>
                           <groupId>org.springframework.boot</groupId>
                           <artifactId>spring-boot-configuration-processor</artifactId>
                       </exclude>
                   </excludes>
               </configuration>
           </plugin>
       </plugins>
  </build>
  ```

- 导入之后：

![image-20210714111433692](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210714111433.png)

把项目重新跑一次，跑完之后生成了对应的meta能用了嗷！！！

- 运行然后停止，再一次在yaml中输入对应的内容的时候，就有提示啦！！！如下图所示：

![image-20210714111748902](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210714111749.png)

- 这个功能和业务无关，开发好用，部署不用。所以打包的时候将其排除在外，所以才有了下面那一段的配置，打包的时候排除了这个提示的类嗷！！！



## Web开发：

### 1、SpringMVC自动配置概览

Spring Boot provides auto-configuration for Spring MVC that **works well with most applications.(大多场景我们都无需自定义配置)**

The auto-configuration adds the following features on top of Spring’s defaults:

- Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.
  - 内容协商视图解析器和BeanName视图解析器

- Support for serving static resources, including support for WebJars (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content))).
  - 静态资源（包括webjars）

- Automatic registration of `Converter`, `GenericConverter`, and `Formatter` beans.
  - 自动注册 `Converter，GenericConverter，Formatter `

- Support for `HttpMessageConverters` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-message-converters)).
  - 支持 `HttpMessageConverters` （后来我们配合内容协商理解原理）

- Automatic registration of `MessageCodesResolver` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-message-codes)).
  - 自动注册 `MessageCodesResolver` （国际化用）

- Static `index.html` support.
  - 静态index.html 页支持

- Custom `Favicon` support (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-favicon)).
  - 自定义 `Favicon`  

- Automatic use of a `ConfigurableWebBindingInitializer` bean (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-web-binding-initializer)).
  - 自动使用 `ConfigurableWebBindingInitializer` ，（DataBinder负责将请求数据绑定到JavaBean上）



#### Boot提供的自动配置方法：

>If you want to keep those Spring Boot MVC customizations and make more [MVC customizations](https://docs.spring.io/spring/docs/5.2.9.RELEASE/spring-framework-reference/web.html#mvc) (interceptors, formatters, view controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` but **without** `@EnableWebMvc`.

**不用@EnableWebMvc注解。使用** `**@Configuration**` **+** `**WebMvcConfigurer**` **自定义规则**





> If you want to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, and still keep the Spring Boot MVC customizations, you can declare a bean of type `WebMvcRegistrations` and use it to provide custom instances of those components.

**声明** `**WebMvcRegistrations**` **改变默认底层组件**



> If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`, or alternatively add your own `@Configuration`-annotated `DelegatingWebMvcConfiguration` as described in the Javadoc of `@EnableWebMvc`.

**使用** `**@EnableWebMvc+@Configuration+DelegatingWebMvcConfiguration 全面接管SpringMVC**`





### 2、简单功能分析

#### 2.1、静态资源访问

##### 1、静态资源目录

只要静态资源放在类路径下： called `/static` (or `/public` or `/resources` or `/META-INF/resources`

浏览器访问 ： 当前项目根路径/ + 静态资源名 



原理： 静态映射/**。

请求进来，先去找**Controller**看能不能处理。不能处理的所有请求又都交给**静态资源处理器**。静态资源也找不到则响应404页面。（先动态处理，后静态处理，找不到就404）



改变默认的静态资源路径：解决拦截器会拦截所有页面的问题，可以通过前缀的这种方式来放行对于静态资源的请求嗷！！！

```yaml
spring:
  mvc:
    static-path-pattern: /res/**

  resources:
    static-locations: [classpath:/haha/]
```



##### 2、静态资源访问前缀

默认无前缀

```yaml
spring:
  mvc:
    static-path-pattern: /res/**
```

当前项目 + static-path-pattern + 静态资源名 = 静态资源文件夹下找对应的静态文件嗷！！！

##### 3、静态资源访问位置修改

默认就是上面那一些，但是可以修改：

```yaml
spring:
  resources:
    static-locations: [classpath:/haha/]
```





##### 4、webjar（使用位置）

- 将css和js打成了jar包方便使用，例如jquery哇，就是webjar嗷！！！（提供了一些配置好的样式和行为之类的嗷！！！）
- 这一类的内容都是从专门的webjars官网上扒相关的依赖扒出来的，这种jar包有自己独特的结构。boot整合了这些jar包，并且能够访问这种结构下的静态资源，就是我们现在所见到的webjars
- 引入了新的依赖，我们需要重新加载项目进行加载嗷！！！

自动映射 /[webjars](http://localhost:8080/webjars/jquery/3.5.1/jquery.js)/**

https://www.webjars.org/

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.5.1</version>
</dependency>
```

访问地址：[http://localhost:8080/webjars/**jquery/3.5.1/jquery.js**](http://localhost:8080/webjars/jquery/3.5.1/jquery.js)   后面地址要按照依赖里面的包路径



#### 2.2、欢迎页支持

- 静态资源路径下  index.html
  - 可以配置静态资源路径
  - 但是不可以配置静态资源的访问前缀。否则导致 index.html不能被默认访问

```yaml
spring:
#  mvc:
#    static-path-pattern: /res/**   这个会导致welcome page功能失效

  resources:
    static-locations: [classpath:/haha/]
```

- controller能处理/index









#### 2.3、自定义 `Favicon`

favicon.ico 放在静态资源目录下即可。

```yaml
spring:
#  mvc:
#    static-path-pattern: /res/**   这个会导致 Favicon 功能失效
```





#### 2.4、静态资源配置原理

- SpringBoot启动默认加载  xxxAutoConfiguration 类（自动配置类）
- SpringMVC功能的自动配置类 WebMvcAutoConfiguration，生效

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
		ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {}
```

- 给容器中配了什么。

```java
@Configuration(proxyBeanMethods = false)
@Import(EnableWebMvcConfiguration.class)
@EnableConfigurationProperties({ WebMvcProperties.class, ResourceProperties.class })
@Order(0)
public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer {}
```

- 配置文件的相关属性和xxx进行了绑定。WebMvcProperties\==**spring.mvc**、ResourceProperties\==**spring.resources**











##### 1、配置类只有一个有参构造器

```java
	//有参构造器所有参数的值都会从容器中确定
//ResourceProperties resourceProperties；获取和spring.resources绑定的所有的值的对象
//WebMvcProperties mvcProperties 获取和spring.mvc绑定的所有的值的对象
//ListableBeanFactory beanFactory Spring的beanFactory
//HttpMessageConverters 找到所有的HttpMessageConverters
//ResourceHandlerRegistrationCustomizer 找到 资源处理器的自定义器。=========
//DispatcherServletPath  
//ServletRegistrationBean   给应用注册Servlet、Filter....
public WebMvcAutoConfigurationAdapter(ResourceProperties resourceProperties, WebMvcProperties mvcProperties,
                                      ListableBeanFactory beanFactory, ObjectProvider<HttpMessageConverters> messageConvertersProvider,
                                      ObjectProvider<ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider,
                                      ObjectProvider<DispatcherServletPath> dispatcherServletPath,
                                      ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
    this.resourceProperties = resourceProperties;
    this.mvcProperties = mvcProperties;
    this.beanFactory = beanFactory;
    this.messageConvertersProvider = messageConvertersProvider;
    this.resourceHandlerRegistrationCustomizer = resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
    this.dispatcherServletPath = dispatcherServletPath;
    this.servletRegistrations = servletRegistrations;
}
```









##### 2、资源处理的默认规则

```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    //resourceProperties是从容器中拿的，静态资源访问的配置。
    //如果这里整成false，往下走不了，后面所有的静态资源都生效不了嗷！！！
    if(!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
        return;
    }
    Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
    CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
    //webjars的规则
    if (!registry.hasMappingForPattern("/webjars/**")) {
        customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
     .addResourceLocations("classpath:/META-INF/resources/webjars/")
                                             .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
    }

    //
    String staticPathPattern = this.mvcProperties.getStaticPathPattern();
    if (!registry.hasMappingForPattern(staticPathPattern)) {
        customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
                                             .addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
                                             .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
    }
}

```



```yaml
spring:
#  mvc:
#    static-path-pattern: /res/**

  resources:
    add-mappings: false   # 禁用所有静态资源规则
@ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
public class ResourceProperties {

	private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
			"classpath:/resources/", "classpath:/static/", "classpath:/public/" };

	/**
	 * Locations of static resources. Defaults to classpath:[/META-INF/resources/,
	 * /resources/, /static/, /public/].
	 */
	//private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
```



##### 3、欢迎页的处理规则

```java
//HandlerMapping：处理器映射。保存了每一个Handler能处理哪些请求。（非常重要的组件，用不同的处理方法处理不同的请求）	

@Bean
//下面参数中的这些东西都是从容器中拿出来嗷！！！
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
    WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
    new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
    this.mvcProperties.getStaticPathPattern());
    welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
    welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
    return welcomePageHandlerMapping;
}

WelcomePageHandlerMapping(TemplateAvailabilityProviders templateAvailabilityProviders,
ApplicationContext applicationContext, Optional<Resource> welcomePage, String staticPathPattern) {
    if (welcomePage.isPresent() && "/**".equals(staticPathPattern)) {
    //要用欢迎页功能，必须是/**
    logger.info("Adding welcome page: " + welcomePage.get());
    setRootViewName("forward:index.html");
    }
    else if (welcomeTemplateExists(templateAvailabilityProviders, applicationContext)) {
    // 调用Controller  /index
    logger.info("Adding welcome page template: index");
    setRootViewName("index");S
    }
}
```



##### 4、favicon



### 3、请求参数处理

#### 0、请求映射

##### 0.1、rest使用与原理

- @xxxMapping；
  - Rest风格支持（*使用**HTTP**请求方式动词来表示对资源的操作*）
  - *以前：**/getUser*  *获取用户*    */deleteUser* *删除用户*   */editUser*  *修改用户*      */saveUser* *保存用户*
  - *现在： /user*    *GET-**获取用户*    *DELETE-**删除用户*     *PUT-**修改用户*      *POST-**保存用户*
  - 核心Filter；HiddenHttpMethodFilter
    - 用法： 表单method=post，隐藏域 _method=put
    - SpringBoot中手动开启
  - 扩展：如何把_method 这个名字换成我们自己喜欢的。

```java
@RequestMapping(value = "/user",method = RequestMethod.GET)
public String getUser(){
    return "GET-张三";
}

@RequestMapping(value = "/user",method = RequestMethod.POST)
public String saveUser(){
    return "POST-张三";
}


@RequestMapping(value = "/user",method = RequestMethod.PUT)
public String putUser(){
    return "PUT-张三";
}

@RequestMapping(value = "/user",method = RequestMethod.DELETE)
public String deleteUser(){
    return "DELETE-张三";
}


@Bean	
@ConditionalOnMissingBean(HiddenHttpMethodFilter.class)
@ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter", name = "enabled", matchIfMissing = false)
public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
    return new OrderedHiddenHttpMethodFilter();
}


//自定义filter(主要是_method的解析，将自己写的加入到配置类中去从而加入到容器中去，而不用容器为我们生成嗷！！！注意，这里自己定义的容器，可以参照默认的容器来改嗷！！！)
@Bean
public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
    HiddenHttpMethodFilter methodFilter = new HiddenHttpMethodFilter();
    //这里新建一个Filter，然后改变里面这个Filter的的MethodParam嗷！！！
    methodFilter.setMethodParam("_m");
    return methodFilter;
}
```

Rest原理（表单提交要使用REST的时候）

- 表单提交会带上**_method=PUT**
- **请求过来被**HiddenHttpMethodFilter拦截
  - 请求是否正常，并且是POST:
    - 获取到**_method**的值。
    - 兼容以下请求；**PUT**.**DELETE**.**PATCH**
    - **原生request（post），包装模式requesWrapper重写了getMethod方法，返回的是传入的值。**
    - **过滤器链放行的时候用wrapper（原request的包装类，主要是重写了getMethod方法）。以后的方法调用getMethod是调用requesWrapper的。**
- 本质上就是Filter传入参数（原request和request带有的_method的值），重新构造一个“带有\_method方法的request”，作为包装后的request嗷！！！





**Rest使用客户端工具，**

- 如PostMan直接发送Put、delete等方式请求，无需Filter。（表单的Http层只有Get和Post，因此需要Filter的处理。而PostMan等客户端可以直接在Http层发送类似于Put这样的请求，因此就不需要Filter的处理了嗷！！！）

```yaml
spring:
  mvc:
    hiddenmethod:
      filter:
        enabled: true   #开启页面表单的Rest功能
```

- 上面这一段是可以根据`OrderedHiddenHttpMethodFilter`的源码看到的嗷，只有配置了以上的配置，才能够使得我们Rest风格生效嗷！！！
- 前后端分离，Rest风格必须要掌握嗷！！！

- 新的东西嗷！！！
  - @GetMapping
  - @PostMapping
  - @PutMapping
  - @DeleteMapping





##### 0.2、请求映射原理

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603181171918-b8acfb93-4914-4208-9943-b37610e93864.png)

SpringMVC功能分析都从 org.springframework.web.servlet.DispatcherServlet     ->doDispatch（）

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    boolean multipartRequestParsed = false;

    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

    try {
        ModelAndView mv = null;
        Exception dispatchException = null;

    try {
        processedRequest = checkMultipart(request);
        multipartRequestParsed = (processedRequest != request);

        // 找到当前请求使用哪个Handler（Controller的方法）处理，Handler其实就是我们编写的Controller，对应的方法嗷！！！
        mappedHandler = getHandler(processedRequest);

        //HandlerMapping：处理器映射。/xxx->>xxxx
```

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603181460034-ba25f3c0-9cfd-4432-8949-3d1dd88d8b12.png)

**RequestMappingHandlerMapping**：保存了所有@RequestMapping 和handler的映射规则。

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603181662070-9e526de8-fd78-4a02-9410-728f059d6aef.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

所有的请求映射都在HandlerMapping中（SpringBoot默人的和我们配置的都有嗷！！！）



- SpringBoot自动配置欢迎页的 WelcomePageHandlerMapping 。访问 /能访问到index.html；
- SpringBoot自动配置了默认 的 RequestMappingHandlerMapping

- 请求进来，挨个尝试所有的HandlerMapping看是否有请求信息。

- 如果有就找到这个请求对应的handler
- 如果没有就是下一个 HandlerMapping

- 我们需要一些自定义的映射处理，我们也可以自己给容器中放**HandlerMapping**。自定义 **HandlerMapping**（使用场景比如：不同版本情况下，大量的配置规则需要更改。这个时候我们就可以自定义HandlerMapping来指定处理的请求和其对应的Handler）

```java
protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
	if (this.handlerMappings != null) {
		for (HandlerMapping mapping : this.handlerMappings) {
            //这个getHandler就直接帮助我们找到了对应的Controller，就很有意思
			HandlerExecutionChain handler = mapping.getHandler(request);
			if (handler != null) {
				return handler;
			}
		}
	}
	return null;
}
```







#### 1、普通参数与基本注解

##### 1.1、注解：

@PathVariable、@RequestHeader、@ModelAttribute、@RequestParam、@MatrixVariable、@CookieValue、@RequestBody、@RequestAttribute

```java
@RestController
public class ParameterTestController {

	//下面这些都是rest风格中常用的嗷！！！
    //car/2/owner/zhangsan
    //注意Map的用法嗷，可以一次把所有的路径变量都提取出来嗷！！！
    //请求头的用法和其他的感觉都是一样的嗷！！！
    @GetMapping("/car/{id}/owner/{username}")
    public Map<String,Object> getCar(@PathVariable("id") Integer id,
                                     @PathVariable("username") String name,
                                     @PathVariable Map<String,String> pv,
                                     @RequestHeader("User-Agent") String userAgent,
                                     @RequestHeader Map<String,String> header,
                                     @RequestParam("age") Integer age,
                                     @RequestParam("inters") List<String> inters,
                                     @RequestParam Map<String,String> params,
                                     @CookieValue("_ga") String _ga,
                                     @CookieValue("_ga") Cookie cookie){


        Map<String,Object> map = new HashMap<>();

//        map.put("id",id);
//        map.put("name",name);
//        map.put("pv",pv);
//        map.put("userAgent",userAgent);
//        map.put("headers",header);
        map.put("age",age);
        map.put("inters",inters);
        map.put("params",params);
        map.put("_ga",_ga);
        System.out.println(cookie.getName()+"===>"+cookie.getValue());
        return map;
    }

//这里直接获得请求体中的数据嗷！！！
    @PostMapping("/save")
    public Map postMethod(@RequestBody String content){
        Map<String,Object> map = new HashMap<>();
        map.put("content",content);
        return map;
    }


    //1、语法： 请求路径：/cars/sell;low=34;brand=byd,audi,yd
    //2、SpringBoot默认是禁用了矩阵变量的功能
    //手动开启：原理。对于路径的处理UrlPathHelper进行解析。
    //removeSemicolonContent（移除分号内容）支持矩阵变量的
    //3、矩阵变量必须有url路径变量才能被解析,从获取到的路径来看，矩阵变量并不算url的一部分！！！
    @GetMapping("/cars/{path}")
    public Map carsSell(@MatrixVariable("low") Integer low,
                        @MatrixVariable("brand") List<String> brand,
                        @PathVariable("path") String path){
        Map<String,Object> map = new HashMap<>();

        map.put("low",low);
        map.put("brand",brand);
        map.put("path",path);
        return map;
    }

    // /boss/1;age=20/2;age=10
	// 有两个路径变量，同时matrix中也有两个嗷！！！因此可以通过不同的pathVar来获得不同的值嗷！！！不指定的话，默认获取的都是第一个的值嗷！！！
    @GetMapping("/boss/{bossId}/{empId}")
    public Map boss(@MatrixVariable(value = "age",pathVar = "bossId") Integer bossAge,
                    @MatrixVariable(value = "age",pathVar = "empId") Integer empAge){
        Map<String,Object> map = new HashMap<>();

        map.put("bossAge",bossAge);
        map.put("empAge",empAge);
        return map;

    }

}
```



- @RequestAttribute一般用在页面跳转的时候，request.set之后，forward到不同的页面，然后request.get拿出来对应的属性进行处理。这个时候，就可以用到@RequestAttribute直接以参数的形式拿到对应的参数，方便进一步进行处理嗷！！！
- 矩阵变量：

![image-20210715102235551](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210715102235.png)

- 由于我们的boot自动关闭了这个功能，我们需要重写底层的一些东西来打开矩阵处理对应的功能嗷！！！

- 手动开启Matrix的两种方式：

1. 使用@Bean向容器中放上一个WebMvcConfigurer类型的组件：

![image-20210715104704788](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210715104704.png)



2. 让配置类实现WebMvcConfigurer接口，修改我们要修改的方法即可：

![image-20210715104535073](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210715104535.png)

- 上面两个的本质是一样的嗷！！！都是向容器中加了一个WebMvcConfigurer组件而已，一个是通过继承加上@Configuration添加的。另外一个是在@Configuration下的@Bean中添加的嗷！！！

- 原理讲解都是从dispatcherServlet的doDispatch方法开始的嗷，我们一定要注意嗷！！！这个才是核心方法嗷！！！





##### 1.2、Servlet API：

WebRequest、ServletRequest、MultipartRequest、 HttpSession、javax.servlet.http.PushBuilder、Principal、InputStream、Reader、HttpMethod、Locale、TimeZone、ZoneId



**ServletRequestMethodArgumentResolver  以上的部分参数**

```java
@Override
	public boolean supportsParameter(MethodParameter parameter) {
		Class<?> paramType = parameter.getParameterType();
		return (WebRequest.class.isAssignableFrom(paramType) ||
				ServletRequest.class.isAssignableFrom(paramType) ||
				MultipartRequest.class.isAssignableFrom(paramType) ||
				HttpSession.class.isAssignableFrom(paramType) ||
				(pushBuilder != null && pushBuilder.isAssignableFrom(paramType)) ||
				Principal.class.isAssignableFrom(paramType) ||
				InputStream.class.isAssignableFrom(paramType) ||
				Reader.class.isAssignableFrom(paramType) ||
				HttpMethod.class == paramType ||
				Locale.class == paramType ||
				TimeZone.class == paramType ||
				ZoneId.class == paramType);
	}
```



##### 1.3、复杂参数：

- Model很重要，Model可以设置全局的值，model中设置的值最终会被放到request的请求域中嗷！！！



- **Map**、**Model（map、model里面的数据会被放在request的请求域  request.setAttribute）、**Errors/BindingResult、**RedirectAttributes（ 重定向携带数据）**、**ServletResponse（response）**、SessionStatus、UriComponentsBuilder、ServletUriComponentsBuilder

```java
Map<String,Object> map,  Model model, HttpServletRequest request 都是可以给request域中放数据，
request.getAttribute();
```

**Map、Model类型的参数**，会返回 mavContainer.getModel（）；---> BindingAwareModelMap 是Model 也是Map

- Map和Model的底层本质上都是调用的**mavContainer**.getModel(); 获取到值的

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603271442869-63b4c3c7-c721-4074-987d-cbe5999273ae.png)

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603271678813-d8e1a1e5-94fa-412c-a7f1-6f27174fd127.png)

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603271813894-037be041-92a5-49af-a49c-c350b3dd587a.png)

- 所有的数据都放在了ModelAndView中嗷，操作都是对于ModelAndView的更新

##### 1.4、自定义对象参数：

可以自动类型转换与格式化，可以级联封装。

- 使用级联封装，Boot可以帮助我们直接进行对象的封装，封装好的对象作为参数传入到对应的Controller方法中！！！

```java
/**
 *     姓名： <input name="userName"/> <br/>
 *     年龄： <input name="age"/> <br/>
 *     生日： <input name="birth"/> <br/>
 *     宠物姓名：<input name="pet.name"/><br/>
 *     宠物年龄：<input name="pet.age"/>
 */
@Data
public class Person {
    
    private String userName;
    private Integer age;
    private Date birth;
    private Pet pet;
    
}

@Data
public class Pet {

    private String name;
    private String age;

}

result
```



#### 2、POJO封装过程

- **ServletModelAttributeMethodProcessor**

- 查看3.5.3嗷！！！

#### 3、参数处理原理

- DispatcherServlet和其中的doDispatch方法很重要嗷！！！

- HandlerMapping中找到能处理请求的Handler（Controller.method()）
- 为当前Handler 找一个适配器 HandlerAdapter； **RequestMappingHandlerAdapter**

- 适配器执行目标方法并确定方法参数的每一个值







##### 3.1、HandlerAdapter

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603262942726-107353bd-f8b7-44f6-93cf-2a3cad4093cf.png)

0 - 支持方法上标注@RequestMapping 

1 - 支持函数式编程的

xxxxxx

##### 3.2、执行目标方法

```java
// Actually invoke the handler.
//DispatcherServlet -- doDispatch
mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
mav = invokeHandlerMethod(request, response, handlerMethod); //执行目标方法


//ServletInvocableHandlerMethod
Object returnValue = invokeForRequest(webRequest, mavContainer, providedArgs);
//获取方法的参数值
Object[] args = getMethodArgumentValues(request, mavContainer, providedArgs);
```

##### 3.3、参数解析器-HandlerMethodArgumentResolver

确定将要执行的目标方法的每一个参数的值是什么;

SpringMVC目标方法能写多少种参数类型。取决于参数解析器。

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603263283504-85bbd4d5-a9af-4dbf-b6a2-30b409868774.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603263394724-33122714-9d06-42ec-bf45-e440e8b49c05.png)

- 当前解析器是否支持解析这种参数
- 支持就调用 resolveArgument



##### 3.4、返回值处理器

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603263524227-386da4be-43b1-4b17-a2cc-8cf886346af9.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



##### 3.5、如何确定目标方法每一个参数的值

```java
============InvocableHandlerMethod==========================
protected Object[] getMethodArgumentValues(NativeWebRequest request, @Nullable ModelAndViewContainer mavContainer,
			Object... providedArgs) throws Exception {

		MethodParameter[] parameters = getMethodParameters();
		if (ObjectUtils.isEmpty(parameters)) {
			return EMPTY_ARGS;
		}

		Object[] args = new Object[parameters.length];
		for (int i = 0; i < parameters.length; i++) {
			MethodParameter parameter = parameters[i];
			parameter.initParameterNameDiscovery(this.parameterNameDiscoverer);
			args[i] = findProvidedArgument(parameter, providedArgs);
			if (args[i] != null) {
				continue;
			}
			if (!this.resolvers.supportsParameter(parameter)) {
				throw new IllegalStateException(formatArgumentError(parameter, "No suitable resolver"));
			}
			try {
				args[i] = this.resolvers.resolveArgument(parameter, mavContainer, request, this.dataBinderFactory);
			}
			catch (Exception ex) {
				// Leave stack trace for later, exception may actually be resolved and handled...
				if (logger.isDebugEnabled()) {
					String exMsg = ex.getMessage();
					if (exMsg != null && !exMsg.contains(parameter.getExecutable().toGenericString())) {
						logger.debug(formatArgumentError(parameter, exMsg));
					}
				}
				throw ex;
			}
		}
		return args;
	}
```

###### 3.5.1、挨个判断所有参数解析器那个支持解析这个参数

```java
@Nullable
private HandlerMethodArgumentResolver getArgumentResolver(MethodParameter parameter) {
    HandlerMethodArgumentResolver result = this.argumentResolverCache.get(parameter);
    if (result == null) {
        for (HandlerMethodArgumentResolver resolver : this.argumentResolvers) {
            if (resolver.supportsParameter(parameter)) {
                result = resolver;
                this.argumentResolverCache.put(parameter, result);
                break;
            }
        }
    }
    return result;
}
```

###### 3.5.2、解析这个参数的值

```
调用各自 HandlerMethodArgumentResolver 的 resolveArgument 方法即可
```

###### 3.5.3、自定义类型参数 封装POJO

**ServletModelAttributeMethodProcessor  这个参数处理器支持**

 **是否为简单类型。**

```java
public static boolean isSimpleValueType(Class<?> type) {
		return (Void.class != type && void.class != type &&
				(ClassUtils.isPrimitiveOrWrapper(type) ||
				Enum.class.isAssignableFrom(type) ||
				CharSequence.class.isAssignableFrom(type) ||
				Number.class.isAssignableFrom(type) ||
				Date.class.isAssignableFrom(type) ||
				Temporal.class.isAssignableFrom(type) ||
				URI.class == type ||
				URL.class == type ||
				Locale.class == type ||
				Class.class == type));
	}
@Override
	@Nullable
public final Object resolveArgument(MethodParameter parameter, @Nullable ModelAndViewContainer mavContainer,
                                    NativeWebRequest webRequest, @Nullable WebDataBinderFactory binderFactory) throws Exception {

    Assert.state(mavContainer != null, "ModelAttributeMethodProcessor requires ModelAndViewContainer");
    Assert.state(binderFactory != null, "ModelAttributeMethodProcessor requires WebDataBinderFactory");

    String name = ModelFactory.getNameForParameter(parameter);
    ModelAttribute ann = parameter.getParameterAnnotation(ModelAttribute.class);
    if (ann != null) {
        mavContainer.setBinding(name, ann.binding());
    }

    Object attribute = null;
    BindingResult bindingResult = null;

    if (mavContainer.containsAttribute(name)) {
        attribute = mavContainer.getModel().get(name);
    }
    else {
        // Create attribute instance
        try {
            attribute = createAttribute(name, parameter, binderFactory, webRequest);
        }
        catch (BindException ex) {
            if (isBindExceptionRequired(parameter)) {
                // No BindingResult parameter -> fail with BindException
                throw ex;
            }
            // Otherwise, expose null/empty value and associated BindingResult
            if (parameter.getParameterType() == Optional.class) {
                attribute = Optional.empty();
            }
            bindingResult = ex.getBindingResult();
        }
    }

    if (bindingResult == null) {
        // Bean property binding and validation;
        // skipped in case of binding failure on construction.
        WebDataBinder binder = binderFactory.createBinder(webRequest, attribute, name);
        if (binder.getTarget() != null) {
            if (!mavContainer.isBindingDisabled(name)) {
                bindRequestParameters(binder, webRequest);
            }
            validateIfApplicable(binder, parameter);
            if (binder.getBindingResult().hasErrors() && isBindExceptionRequired(binder, parameter)) {
                throw new BindException(binder.getBindingResult());
            }
        }
        // Value type adaptation, also covering java.util.Optional
        if (!parameter.getParameterType().isInstance(attribute)) {
            attribute = binder.convertIfNecessary(binder.getTarget(), parameter.getParameterType(), parameter);
        }
        bindingResult = binder.getBindingResult();
    }

    // Add resolved attribute and BindingResult at the end of the model
    Map<String, Object> bindingResultModel = bindingResult.getModel();
    mavContainer.removeAttributes(bindingResultModel);
    mavContainer.addAllAttributes(bindingResultModel);

    return attribute;
}
```

 

**WebDataBinder binder = binderFactory.createBinder(webRequest, attribute, name);**

**WebDataBinder :web数据绑定器，将请求参数的值绑定到指定的JavaBean里面**

**WebDataBinder 利用它里面的 Converters 将请求数据转成指定的数据类型。再次封装到JavaBean中**



**GenericConversionService：在设置每一个值的时候，找它里面的所有converter那个可以将这个数据类型（request带来参数的字符串）转换到指定的类型（JavaBean -- Integer）**

**byte -- > file**



@FunctionalInterface**public interface** Converter<S, T>

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603337871521-25fc1aa1-133a-4ce0-a146-d565633d7658.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)





![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603338486441-9bbd22a9-813f-49bd-b51b-e66c7f4b8598.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)





未来我们可以给WebDataBinder里面放自己的Converter；

**private static final class** StringToNumber<T **extends** Number> **implements** Converter<String, T>



自定义 Converter

```java
//1、WebMvcConfigurer定制化SpringMVC的功能
@Bean
public WebMvcConfigurer webMvcConfigurer(){
    return new WebMvcConfigurer() {
        @Override
        public void configurePathMatch(PathMatchConfigurer configurer) {
            UrlPathHelper urlPathHelper = new UrlPathHelper();
            // 不移除；后面的内容。矩阵变量功能就可以生效
            urlPathHelper.setRemoveSemicolonContent(false);
            configurer.setUrlPathHelper(urlPathHelper);
        }

        @Override
        public void addFormatters(FormatterRegistry registry) {
            registry.addConverter(new Converter<String, Pet>() {

                @Override
                public Pet convert(String source) {
                    // 啊猫,3
                    if(!StringUtils.isEmpty(source)){
                        Pet pet = new Pet();
                        String[] split = source.split(",");
                        pet.setName(split[0]);
                        pet.setAge(Integer.parseInt(split[1]));
                        return pet;
                    }
                    return null;
                }
            });
        }
    };
}
```



- [POJO的Converter的定制视频嗷！！！](https://www.bilibili.com/video/BV19K4y1L7MT/?p=36&spm_id_from=pageDriver)

##### 3.6、目标方法执行完成

将所有的数据都放在 **ModelAndViewContainer**；包含要去的页面地址View。还包含Model数据。

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1603272018605-1bce3142-bdd9-4834-a028-c753e91c52ac.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

##### 3.7、处理派发结果

**processDispatchResult**(processedRequest, response, mappedHandler, mv, dispatchException);



renderMergedOutputModel(mergedModel, getRequestToExpose(request), response);



```java
InternalResourceView：
@Override
	protected void renderMergedOutputModel(
			Map<String, Object> model, HttpServletRequest request, HttpServletResponse response) throws Exception {

		// Expose the model object as request attributes.
		exposeModelAsRequestAttributes(model, request);

		// Expose helpers as request attributes, if any.
		exposeHelpers(request);

		// Determine the path for the request dispatcher.
		String dispatcherPath = prepareForRendering(request, response);

		// Obtain a RequestDispatcher for the target resource (typically a JSP).
		RequestDispatcher rd = getRequestDispatcher(request, dispatcherPath);
		if (rd == null) {
			throw new ServletException("Could not get RequestDispatcher for [" + getUrl() +
					"]: Check that the corresponding file exists within your web application archive!");
		}

		// If already included or response already committed, perform include, else forward.
		if (useInclude(request, response)) {
			response.setContentType(getContentType());
			if (logger.isDebugEnabled()) {
				logger.debug("Including [" + getUrl() + "]");
			}
			rd.include(request, response);
		}

		else {
			// Note: The forwarded resource is supposed to determine the content type itself.
			if (logger.isDebugEnabled()) {
				logger.debug("Forwarding to [" + getUrl() + "]");
			}
			rd.forward(request, response);
		}
	}
```

```java
//暴露模型作为请求域属性
// Expose the model object as request attributes.
		exposeModelAsRequestAttributes(model, request);
```

```java
protected void exposeModelAsRequestAttributes(Map<String, Object> model,
			HttpServletRequest request) throws Exception {

    //model中的所有数据遍历挨个放在请求域中
		model.forEach((name, value) -> {
			if (value != null) {
				request.setAttribute(name, value);
			}
			else {
				request.removeAttribute(name);
			}
		});
	}
```





### 4、数据响应与内容协商

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606043749073-2573e24a-9ea9-459e-ad94-a433e1082624.png)



#### 1、响应JSON

##### 1.1、jackson.jar+@ResponseBody

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!--web场景自动引入了json场景-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-json</artifactId>
    <version>2.3.4.RELEASE</version>
    <scope>compile</scope>
</dependency>
```

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605151090728-f7c60e6f-d0c0-4541-bfa3-8cc805dfd5d6.png)



给前端自动返回json数据；





###### 1.1.1、返回值解析器

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605151359370-01cd1fbe-628a-4eea-9430-d79a78f59125.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

- 上面这个和参数解析器类似，对于返回值进行分析的嗷！！！

```java
try {
    this.returnValueHandlers.handleReturnValue(
        returnValue, getReturnValueType(returnValue), mavContainer, webRequest);
}


@Override
public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType,
                              ModelAndViewContainer mavContainer, NativeWebRequest webRequest) throws Exception {

    HandlerMethodReturnValueHandler handler = selectHandler(returnValue, returnType);
    if (handler == null) {
        throw new IllegalArgumentException("Unknown return value type: " + returnType.getParameterType().getName());
    }
    handler.handleReturnValue(returnValue, returnType, mavContainer, webRequest);
}
RequestResponseBodyMethodProcessor  


    @Override
    public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType,
                                  ModelAndViewContainer mavContainer, NativeWebRequest webRequest)
    throws IOException, HttpMediaTypeNotAcceptableException, HttpMessageNotWritableException {

    mavContainer.setRequestHandled(true);
    ServletServerHttpRequest inputMessage = createInputMessage(webRequest);
    ServletServerHttpResponse outputMessage = createOutputMessage(webRequest);

    // Try even with null return value. ResponseBodyAdvice could get involved.
    // 使用消息转换器进行写出操作
    writeWithMessageConverters(returnValue, returnType, inputMessage, outputMessage);
}
```



###### 1.1.2、返回值解析器原理

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605151728659-68c8ce8a-1b2b-4ab0-b86d-c3a875184672.png)



- 1、返回值处理器判断是否支持这种类型返回值 supportsReturnType
- 2、返回值处理器调用 handleReturnValue 进行处理

- 3、RequestResponseBodyMethodProcessor 可以处理返回值标了@ResponseBody 注解的：
  - 1. 利用 MessageConverters 进行处理，将数据写为json：
       - 1、内容协商（浏览器默认会以请求头的方式告诉服务器他能接受什么样的内容类型）
       - 2、服务器最终根据自己自身的能力，决定服务器能生产出什么样内容类型的数据，
       - 3、SpringMVC会挨个遍历所有容器底层的 HttpMessageConverter ，看谁能处理？
         - 1、得到MappingJackson2HttpMessageConverter可以将对象写为json
         - 2、利用MappingJackson2HttpMessageConverter将对象转为json再写出去。













![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605163005521-a20d1d8e-0494-43d0-8135-308e7a22e896.png)





##### 1.2、SpringMVC到底支持哪些返回值

```java
ModelAndView
Model
View
ResponseEntity 
ResponseBodyEmitter
StreamingResponseBody
HttpEntity
HttpHeaders
Callable
DeferredResult
ListenableFuture
CompletionStage
WebAsyncTask
有 @ModelAttribute 且为对象类型的
@ResponseBody 注解 ---> RequestResponseBodyMethodProcessor；
```

##### 1.3、HTTPMessageConverter原理



###### 1.3.1、MessageConverter规范

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605163447900-e2748217-0f31-4abb-9cce-546b4d790d0b.png)

HttpMessageConverter: 看是否支持将此 Class类型的对象，转为MediaType类型的数据。

例子：Person对象转为JSON。或者 JSON转为Person



###### 1.3.2、默认的MessageConverter

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605163584708-e19770d6-6b35-4caa-bf21-266b73cb1ef1.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

0 - 只支持Byte类型的

1 - String

2 - String

3 - Resource

4 - ResourceRegion

5 - DOMSource.class \\SAXSource.class) \ StAXSource.class \\StreamSource.class \Source.class

6 - MultiValueMap

**7 - true** 

8 - true

9 - 支持注解方式xml处理的。



最终 MappingJackson2HttpMessageConverter  把对象转为JSON（利用底层的jackson的objectMapper转换的）

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605164243168-1a31e9af-54a4-463e-b65a-c28ca7a8a2fa.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



#### 2、内容协商

- 根据客户端接收能力不同，返回不同媒体类型的数据。(比如这里我们引入xml，就能够相应xml依赖啦！！！)

##### 2.1、引入xml依赖

```xml
 <dependency>
            <groupId>com.fasterxml.jackson.dataformat</groupId>
            <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

##### 2.2、postman分别测试返回json和xml

只需要改变请求头中Accept字段。Http协议中规定的，告诉服务器本客户端可以接收的数据类型。

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605173127653-8a06cd0f-b8e1-4e22-9728-069b942eba3f.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



##### 2.3、开启浏览器参数方式内容协商功能

- 这个主要就能够实现，我想给浏览器写json，而不是xml，这个时候就需要开启额外的功能（不像Postman，浏览器发送的请求头不好改Content-type的嗷！！！）

为了方便内容协商，开启基于请求参数的内容协商功能。

```yaml
spring:
    contentnegotiation:
      favor-parameter: true  #开启请求参数内容协商模式
```

发请求： http://localhost:8080/test/person?format=json 

[http://localhost:8080/test/person?format=xml](http://localhost:8080/test/person?format=xml)



![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605230907471-b0ed34bc-6782-40e7-84b7-615726312f01.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

确定客户端接收什么样的内容类型；

1、Parameter策略优先确定是要返回json数据（获取请求头中的format的值）

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605231074299-25f5b062-2de1-4a09-91bf-11e018d6ec0e.png)

2、最终进行内容协商返回给客户端json即可。

3、多个策略中，按照优先级排序，获得第一个不是\*/*的就返回。如果获得的是\*/\*，那就继续循环去遍历下一个策略。如果策略遍历一遍之后，都是\*/\*，那我们就直接只能返回这个了嗷。



##### 2.4、内容协商原理

- 1、判断当前响应头中是否已经有确定的媒体类型。MediaType
- **2、获取客户端（PostMan、浏览器）支持接收的内容类型。（获取客户端Accept请求头字段）【application/xml】**

  - **contentNegotiationManager 内容协商管理器 默认使用基于请求头的策略**
  - ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605230462280-ef98de47-6717-4e27-b4ec-3eb0690b55d0.png)

  - **HeaderContentNegotiationStrategy  确定客户端可以接收的内容类型** 
  - ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605230546376-65dcf657-7653-4a58-837a-f5657778201a.png)

- 3、遍历循环所有当前系统的 **MessageConverter**，看谁**支持操作这个对象**（Person）
- 4、找到支持操作Person的converter，把converter支持的媒体类型统计出来。

- 5、客户端需要【application/xml】。服务端处理总能力【一共10种，包括json、xml在内，还需要循环遍历再排除来得到真正能够处理这个对象的东西！！！】
-   选择出来能够真正处理这个对象的如下统计所示：![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605173876646-f63575e2-50c8-44d5-9603-c2d11a78adae.png)

- 6、上面只是能够处理指定对象而已，和我们的浏览器的要求匹配吗？不一定吧。所以下面就是把我们找出来能够处理的对象，和我们浏览器的要求进行匹配。进行内容协商的最佳匹配媒体类型。（最佳匹配，找出服务端能够提供的，同时满足客户端需求的方式。有可能有多个，还需要对于这些进行排序，然后得到最佳的那一个对象提供服务。**最终选中的永远只有一个！！！**）
- 7、用支持将对象转为最佳匹配媒体类型 的converter。调用它进行转化 。





![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605173657818-73331882-6086-490c-973b-af46ccf07b32.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

导入了jackson处理xml的包，xml的converter就会自动进来

```java
WebMvcConfigurationSupport
jackson2XmlPresent = ClassUtils.isPresent("com.fasterxml.jackson.dataformat.xml.XmlMapper", classLoader);

if (jackson2XmlPresent) {
			Jackson2ObjectMapperBuilder builder = Jackson2ObjectMapperBuilder.xml();
			if (this.applicationContext != null) {
				builder.applicationContext(this.applicationContext);
			}
			messageConverters.add(new MappingJackson2XmlHttpMessageConverter(builder.build()));
		}
```







##### 2.5、自定义 MessageConverter

**实现多协议数据兼容。json、xml、x-guigu**

0、@ResponseBody 响应数据出去 调用 **RequestResponseBodyMethodProcessor** 处理

1、Processor 处理方法返回值。通过 **MessageConverter** 处理

2、所有 **MessageConverter** 合起来可以支持各种媒体类型数据的操作（读、写）

3、内容协商找到最终的 **messageConverter**；



SpringMVC的什么功能。一个入口给容器中添加一个  WebMvcConfigurer。

- 下面这一段代码就是在配置文件MyConfig中，对于Configurer的扩写，添加了我们自定义的GuiguMessageConverter

```java
@Bean
public WebMvcConfigurer webMvcConfigurer(){
    return new WebMvcConfigurer() {

        @Override
        public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
			convert.add(new GuiguMessageConverter());
        }
    }
}
```





![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605260623995-8b1f7cec-9713-4f94-9cf1-8dbc496bd245.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)







![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605261062877-0a27cc41-51cb-4018-a9af-4e0338a247cd.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

- 自定义功能的添加（一个是解析浏览器参数策略，另外一个是本地服务器处理功能的添加）：

![image-20210716201259824](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210716201441.png)









- **有可能我们添加的自定义的功能会覆盖默认很多功能，比如默认方法添加的一些东西，会导致一些默认的功能失效。因此我们还需要查看源码，看看源码中某些类型是如何进行配置的嗷！！！**





大家考虑，上述功能除了我们完全自定义外？SpringBoot有没有为我们提供基于配置文件的快速修改媒体类型功能？怎么配置呢？【提示：参照SpringBoot官方文档web开发内容协商章节】

- [自定义MessageConverter的教程](https://www.bilibili.com/video/BV19K4y1L7MT?p=41&spm_id_from=pageDriver)
- [Converter课后作业的教程](https://www.bilibili.com/video/BV19K4y1L7MT?p=42&spm_id_from=pageDriver)



### 5、视图解析与模板引擎

视图解析：**SpringBoot默认不支持 JSP，需要引入第三方模板引擎技术实现页面渲染。**

#### 1、视图解析

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606043749039-cefbf687-4feb-441d-bad8-c6d933248d3c.png)

##### 1、视图解析原理流程

1、目标方法处理的过程中，所有数据都会被放在 **ModelAndViewContainer 里面。包括数据和视图地址**

**2、方法的参数是一个自定义类型对象（从请求参数中确定的），把他重新放在** **ModelAndViewContainer** 

**3、任何目标方法执行完成以后都会返回 ModelAndView**（**数据和视图地址**）。

**4、processDispatchResult  处理派发结果（页面该如何响应）**

- 1、**render**(**mv**, request, response); 进行页面渲染逻辑
  - 1、根据方法的String返回值得到 **View** 对象【定义了页面的渲染逻辑】

    - 1、所有的视图解析器尝试是否能根据当前返回值得到**View**对象
    - 2、得到了  **redirect:/main.html** --> Thymeleaf new **RedirectView**()

    - 3、ContentNegotiationViewResolver 里面包含了下面所有的视图解析器，内部还是利用下面所有视图解析器得到视图对象。
    - 4、view.render(mv.getModelInternal(), request, response);   视图对象调用自定义的render进行页面渲染工作
      - **RedirectView 如何渲染【重定向到一个页面】**
      - **1、获取目标url地址**

      - **2、response.sendRedirect(encodedURL);**





**视图解析：**

- **返回值以 forward: 开始： new InternalResourceView(forwardUrl); -->  转发**request.getRequestDispatcher(path).forward(request, response);
- **返回值以** **redirect: 开始：** **new RedirectView() --》 render就是重定向** 

- **返回值是普通字符串： new ThymeleafView（）--->** 自定义视图解析器+自定义视图；









![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605680247945-088b0f17-185c-490b-8889-103e8b4d8c07.png)

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605679959020-54b96fe7-f2fc-4b4d-a392-426e1d5413de.png)



![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605679471537-7db702dc-b165-4dc6-b64a-26459ee5fd6c.png)



![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605679913592-151a616a-c754-4da3-a2c1-91dc0230a48d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

#### 2、模板引擎-Thymeleaf

##### 1、thymeleaf简介

Thymeleaf is a modern server-side Java template engine for both web and standalone environments, capable of processing HTML, XML, JavaScript, CSS and even plain text.

**现代化、服务端Java模板引擎**



##### 2、基本语法

###### 1、表达式

| 表达式名字 | 语法   | 用途                               |
| ---------- | ------ | ---------------------------------- |
| 变量取值   | ${...} | 获取请求域、session域、对象等值    |
| 选择变量   | *{...} | 获取上下文对象值                   |
| 消息       | #{...} | 获取国际化等值                     |
| 链接       | @{...} | 生成链接                           |
| 片段表达式 | ~{...} | jsp:include 作用，引入公共页面片段 |



###### 2、字面量

文本值: **'one text'** **,** **'Another one!'** **,…**数字: **0** **,** **34** **,** **3.0** **,** **12.3** **,…**布尔值: **true** **,** **false**

空值: **null**

变量： one，two，.... 变量不能有空格

###### 3、文本操作

字符串拼接: **+**

变量替换: **|The name is ${name}|** 



###### 4、数学运算

运算符: + , - , * , / , %



###### 5、布尔运算

运算符:  **and** **,** **or**

一元运算: **!** **,** **not** 





###### 6、比较运算

比较: **>** **,** **<** **,** **>=** **,** **<=** **(** **gt** **,** **lt** **,** **ge** **,** **le** **)**等式: **==** **,** **!=** **(** **eq** **,** **ne** **)** 



###### 7、条件运算

If-then: **(if) ? (then)**

If-then-else: **(if) ? (then) : (else)**

Default: (value) **?: (defaultvalue)** 



###### 8、特殊操作

无操作： _





##### 3、设置属性值-th:attr

设置单个值

```html
<form action="subscribe.html" th:attr="action=@{/subscribe}">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="Subscribe!" th:attr="value=#{subscribe.submit}"/>
  </fieldset>
</form>
```

设置多个值

```html
<img src="../../images/gtvglogo.png"  th:attr="src=@{/images/gtvglogo.png},title=#{logo},alt=#{logo}" />
```

以上两个的代替写法 th:xxxx

```html
<input type="submit" value="Subscribe!" th:value="#{subscribe.submit}"/>
<form action="subscribe.html" th:action="@{/subscribe}">
```



所有h5兼容的标签写法

https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#setting-value-to-specific-attributes



##### 4、迭代

```html
<tr th:each="prod : ${prods}">
        <td th:text="${prod.name}">Onions</td>
        <td th:text="${prod.price}">2.41</td>
        <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>
```



```html
<tr th:each="prod,iterStat : ${prods}" th:class="${iterStat.odd}? 'odd'">
  <td th:text="${prod.name}">Onions</td>
  <td th:text="${prod.price}">2.41</td>
  <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>
```



##### 5、条件运算

```html
<a href="comments.html"
th:href="@{/product/comments(prodId=${prod.id})}"
th:if="${not #lists.isEmpty(prod.comments)}">view</a>
```



```html
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
  <p th:case="*">User is some other thing</p>
</div>
```



##### 6、属性优先级

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605498132699-4fae6085-a207-456c-89fa-e571ff1663da.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



#### 3、thymeleaf使用

##### 1、引入Starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

##### 2、自动配置好了thymeleaf

```java
@Configuration(proxyBeanMethods = false)
@EnableConfigurationProperties(ThymeleafProperties.class)
@ConditionalOnClass({ TemplateMode.class, SpringTemplateEngine.class })
@AutoConfigureAfter({ WebMvcAutoConfiguration.class, WebFluxAutoConfiguration.class })
public class ThymeleafAutoConfiguration { }
```



自动配好的策略

- 1、所有thymeleaf的配置值都在 ThymeleafProperties
- 2、配置好了 **SpringTemplateEngine** 

- **3、配好了** **ThymeleafViewResolver** 
- 4、我们只需要直接开发页面

```java
	public static final String DEFAULT_PREFIX = "classpath:/templates/";

	public static final String DEFAULT_SUFFIX = ".html";  //xxx.html
```

##### 3、页面开发

- 学的教训：记得要加名称空间啊啊啊啊orz。
- 所有的域中的取值，都是${}这种方式来进行的嗷！！！

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 th:text="${msg}">哈哈</h1>
<h2>
    <a href="www.atguigu.com" th:href="${link}">去百度</a>  <br/>
    <a href="www.atguigu.com" th:href="@{link}">去百度2</a>
</h2>
</body>
</html>
```

- @{}中填充的就是真正的连接，不能够是变量嗷！！！默认的起始地址就是当前的项目的地址嗷！！！



#### 4、构建后台管理系统

##### 1、项目创建

thymeleaf、web-starter、devtools、lombok



##### 2、静态资源处理

自动配置好，我们只需要把所有静态资源放到 static 文件夹下



![image-20210716213802653](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210716213802.png)

##### 3、路径构建

- 注意，加入到form的action才有意义嗷！！！

th:action="@{/login}"



##### 4、模板抽取

- 前后加上div标签最好啦，div比较保险嗷，include , insert , replace三者有所不同，注意康康嗷！！！
- replace感觉用到的最多，直接拿公共抽取部分来整个替换原有的div，感觉非常方便嗷！！！
- 使用公共模板之后，相当于导航的功能的页面跳转，只需要在公共页面中修改即可嗷！！！
- 注意，有的时候bug的出现很有可能是抽取的公共页面中引入了某些资源，原本的代码中也引入了某些资源导致了冲突嗷！！！

th:insert/replace/include



##### 5、页面跳转

```java
@PostMapping("/login")
public String main(User user, HttpSession session, Model model){

    if(StringUtils.hasLength(user.getUserName()) && "123456".equals(user.getPassword())){
        //把登陆成功的用户保存起来
        session.setAttribute("loginUser",user);
        //登录成功重定向到main.html;  重定向防止表单重复提交
        return "redirect:/main.html";
    }else {
        model.addAttribute("msg","账号密码错误");
        //回到登录页面
        return "login";
    }

}
```



##### 6、数据渲染

```java
@GetMapping("/dynamic_table")
public String dynamic_table(Model model){
//表格内容的遍历
List<User> users = Arrays.asList(new User("zhangsan", "123456"),
	new User("lisi", "123444"),
	new User("haha", "aaaaa"),
	new User("hehe ", "aaddd"));
	model.addAttribute("users",users);
	
	return "table/dynamic_table";
}
```


```html
<table class="display table table-bordered" id="hidden-table-info">
    <thead>
    	<tr>
    		<th>#</th>
    		<th>用户名</th>
    		<th>密码</th>
    	</tr>
    </thead>
    <tbody>
        <tr class="gradeX" th:each="user,stats:${users}">
            <td th:text="${stats.count}">Trident</td>
            <td th:text="${user.userName}">Internet</td>
            <td >[[${user.password}]]</td>
        </tr>
    </tbody>
 </table>
```



##### 7、工程问题

- 刷新页面表单重复提交比较耗费资源咋办呢？
  - 使用重定向，这样做的目的是重定向到某一个页面，并且使得网页的url发生了改变。这个时候再次刷新页面本质上就不会再次提交表单，而是请求静态资源（通过另外一个Controller来处理），就会快嗷！！！
  - 第一次登录过程中使用重定向之后，后续刷新页面相当于直接请求资源！！！

- 就像上面所说，如果获得了登录页面的绝对地址，访问就能登录，显然非常不合理，那这该怎么办捏？

```java
/**
 * 处理登录提交的表单，并且控制跳转到main页面
 * @param user 页面表单封装对象
 * @param session 存储登录的用户
 * @param model 添加request域的数据，例如登录失败等信息
 * @return
 */
@PostMapping("/login")
public String main(User user, HttpSession session, Model model){

    if (StringUtils.hasLength(user.getUserName()) && StringUtils.hasLength(user.getPassword())){
        //把登录成功的用户保存起来
        session.setAttribute("loginUser",user);
        //重定向到main亚目
        return "redirect:/main.html";
    }else{
        //用户名密码不符合要求嗷！！！
        model.addAttribute("msg","账号密码错误，请重新登录");
        //回到登录页面
        return "login";
    }
}

/**
 * 和上面的重定向搭配使用，解决了页面刷新过程中，表单重复提交的问题嗷！！！
 * @param session 获取用户Session判断是否登录
 * @param model 向request全局变量添加信息
 * @return 返回具体页面
 */
@GetMapping("/main.html")
public String mainPage(HttpSession session, Model model){

    //判断是否登录，登录了才能访问对应的页面
    User loginUser = (User) session.getAttribute("loginUser");
    if (loginUser != null){
        return "main";
    }else {
        //回到登录页
        model.addAttribute("msg","未登录系统，请重新登录");
        return "login";
    }
}
```

上面两个搭配使用，完成登录的验证功能。

并且session作为全局作用域，后续保存的数据（例如封装的用户名和密码等用户数据），还能在后续的页面中取出来，继续发挥作用嗷！！！例如`[[${session.loginUser.userName}]]`

- 正式退出的时候，还要对于Session等进行处理嗷！！！
- ![image-20210717115241895](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210717115242.png)

可以通过status作为变量参数去取出一些类似于count，index这样的属性，方便我们遍历的时候使用嗷！！！



### 6、拦截器

#### 1、HandlerInterceptor 接口

```java
/**
 * 登录检查
 * 1、配置好拦截器要拦截哪些请求
 * 2、把这些配置放在容器中
 */
@Slf4j
public class LoginInterceptor implements HandlerInterceptor {

    /**
     * 目标方法执行之前
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        String requestURI = request.getRequestURI();
        log.info("preHandle拦截的请求路径是{}",requestURI);

        //登录检查逻辑
        HttpSession session = request.getSession();

        Object loginUser = session.getAttribute("loginUser");

        if(loginUser != null){
            //放行
            return true;
        }

        //拦截住。未登录。跳转到登录页
        request.setAttribute("msg","请先登录");
//        re.sendRedirect("/");
        request.getRequestDispatcher("/").forward(request,response);
        return false;
    }

    /**
     * 目标方法执行完成以后
     * @param request
     * @param response
     * @param handler
     * @param modelAndView
     * @throws Exception
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        log.info("postHandle执行{}",modelAndView);
    }

    /**
     * 页面渲染以后
     * @param request
     * @param response
     * @param handler
     * @param ex
     * @throws Exception
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        log.info("afterCompletion执行异常{}",ex);
    }
}
```



#### 2、配置拦截器

```java
/**
 * 1、编写一个拦截器实现HandlerInterceptor接口
 * 2、拦截器注册到容器中（实现WebMvcConfigurer的addInterceptors）
 * 3、指定拦截规则【如果是拦截所有，静态资源也会被拦截】
 */
@Configuration
public class AdminWebConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns("/**")  //所有请求都被拦截包括静态资源
                .excludePathPatterns("/","/login","/css/**","/fonts/**","/images/**","/js/**"); //放行的请求
    }
}
```

- 一开始为了方便测试在Controller中设置的拦截器要撤掉嗷！！！

![image-20210717165414903](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210717165415.png)

- 存放信息处理：

![image-20210717165810843](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210717165810.png)

这里和前端一样，只能从request中将相应的值取出来。sendRedirect这个时候是不可以的嗷，因为sendRedirect本质上是让客户端发送第二次请求。第二次请求就不会带有我们设置的信息了，所以这里应该使用forward（也可以是我们上面这种写法），将请求转发给"/"处理，传入我们现在的request和response，方便我们我们页面渲染的时候进行处理。

- Log.info()可以打印输出对应位置的某些信息到控制台，方便我们debug或研究程序的执行顺序嗷！！！









#### 3、拦截器原理

1、根据当前请求，找到**HandlerExecutionChain【**可以处理请求的handler以及handler的所有 拦截器】

2、先来**顺序执行** 所有拦截器的 preHandle方法

- 1、如果当前拦截器prehandler返回为true。则执行下一个拦截器的preHandle
- 2、如果当前拦截器返回为false。直接    倒序执行所有已经执行了的拦截器的  afterCompletion；

**3、如果任何一个拦截器返回false。直接跳出不执行目标方法**

**4、所有拦截器都返回True。执行目标方法**

**5、倒序执行所有拦截器的postHandle方法。**

**6、前面的步骤有任何异常都会直接倒序触发** afterCompletion

7、页面成功渲染完成以后，也会倒序触发 afterCompletion



![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605764129365-5b31a748-1541-4bee-9692-1917b3364bc6.png)



![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605765121071-64cfc649-4892-49a3-ac08-88b52fb4286f.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



### 7、文件上传

#### 1、页面表单

```xml
<form method="post" action="/upload" enctype="multipart/form-data">
    <input type="file" name="file"><br>
    <input type="submit" value="提交">
</form>
```



#### 2、文件上传代码



```java
/**
     * MultipartFile 自动封装上传过来的文件
     * @param email
     * @param username
     * @param headerImg
     * @param photos
     * @return
 */
@PostMapping("/upload")
public String upload(@RequestParam("email") String email,
                     @RequestParam("username") String username,
                     @RequestPart("headerImg") MultipartFile headerImg,
                     @RequestPart("photos") MultipartFile[] photos) throws IOException {

    log.info("上传的信息：email={}，username={}，headerImg={}，photos={}",
             email,username,headerImg.getSize(),photos.length);

    if(!headerImg.isEmpty()){
        //保存到文件服务器，OSS服务器
        String originalFilename = headerImg.getOriginalFilename();
        headerImg.transferTo(new File("H:\\cache\\"+originalFilename));
    }

    if(photos.length > 0){
        //这里无非是进行遍历而已嘛！！！
        for (MultipartFile photo : photos) {
            if(!photo.isEmpty()){
                String originalFilename = photo.getOriginalFilename();
                //下面这里保存了文件嗷！！！
                photo.transferTo(new File("H:\\cache\\"+originalFilename));
            }
        }
    }


    return "main";
}
```

#### 3、自动配置原理

**文件上传自动配置类-MultipartAutoConfiguration-----MultipartProperties**

- 自动配置好了 **StandardServletMultipartResolver   【文件上传解析器】**
- **原理步骤**

  - **1、请求进来使用文件上传解析器判断（**isMultipart**）并封装（**resolveMultipart，**返回**MultipartHttpServletRequest**）文件上传请求**
  - **2、参数解析器来解析请求中的文件内容封装成MultipartFile**

  - **3、将request中文件信息封装为一个Map；**MultiValueMap<String, MultipartFile>

**FileCopyUtils**。实现文件流的拷贝

```java
@PostMapping("/upload")
public String upload(@RequestParam("email") String email,
                     @RequestParam("username") String username,
                     @RequestPart("headerImg") MultipartFile headerImg,
                     @RequestPart("photos") MultipartFile[] photos)
```

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605847414866-32b6cc9c-5191-4052-92eb-069d652dfbf9.png)





### 8、异常处理

#### 1、错误处理

##### 1、默认规则

- 默认情况下，Spring Boot提供`/error`处理所有错误的映射
- 对于机器客户端，它将生成JSON响应，其中包含错误，HTTP状态和异常消息的详细信息。对于浏览器客户端，响应一个“ whitelabel”错误视图，以HTML格式呈现相同的数据
- ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606024421363-77083c34-0b0e-4698-bb72-42da351d3944.png)
- 上面的JSON中附带的默认对象，其实都是可以在模板引擎中当作变量来获取的嗷！！！
- ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606024616835-bc491bf0-c3b1-4ac3-b886-d4ff3c9874ce.png)
- **要对其进行自定义，添加**`**View**`**解析为**`**error**``**
  **`
- 要完全替换默认行为，可以实现 `ErrorController `并注册该类型的Bean定义，或添加`ErrorAttributes类型的组件`以使用现有机制但替换其内容。
- error/下的4xx，5xx页面会被自动解析；
- ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606024592756-d4ab8a6b-ec37-426b-8b39-010463603d57.png)

#### 2、定制错误处理逻辑

- 自定义错误页

  - error/404.html   error/5xx.html；有精确的错误状态码页面就匹配精确，没有就找 4xx.html；如果都没有就触发白页
- @ControllerAdvice+@ExceptionHandler处理全局异常；底层是 **ExceptionHandlerExceptionResolver 支持的**

```java
package com.tuzi.exception;

import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@Slf4j
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler({ArithmeticException.class,NullPointerException.class})
    public String handlerArithException(Exception e){

        log.error("异常是:{}",e);
        return "login"; //视图地址
    }
}
```

- @ResponseStatus+自定义异常 ；底层是 **ResponseStatusExceptionResolver ，把responsestatus注解的信息底层调用** **response.sendError(statusCode, resolvedReason)；tomcat发送的/error**

```java
package com.tuzi.exception;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseStatus(value = HttpStatus.FORBIDDEN,reason = "用户数量太多")
public class UserTooManyException extends RuntimeException{
    public UserTooManyException(){

    }

    public UserTooManyException(String message){
        super(message);
    }

}
```

- Spring底层的异常，如 参数类型转换异常；**DefaultHandlerExceptionResolver 处理框架底层的异常。**

- response.sendError(HttpServletResponse.**SC_BAD_REQUEST**, ex.getMessage()); 

- ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606114118010-f4aaf5ee-2747-4402-bc82-08321b2490ed.png)

- 自定义实现 HandlerExceptionResolver 处理异常；可以作为默认的全局异常处理规则

  - ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606114688649-e6502134-88b3-48db-a463-04c23eddedc7.png)

  - ```java
    package com.tuzi.exception;
    
    import org.springframework.core.Ordered;
    import org.springframework.core.annotation.Order;
    import org.springframework.stereotype.Component;
    import org.springframework.web.servlet.HandlerExceptionResolver;
    import org.springframework.web.servlet.ModelAndView;
    
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;
    
    //自定义异常处理
    @Order(value = Ordered.HIGHEST_PRECEDENCE) //优先级最高
    @Component
    public class CustomerHandlerExceptionResolver implements HandlerExceptionResolver {
        @Override
        public ModelAndView resolveException(HttpServletRequest httpServletRequest,
                                             HttpServletResponse httpServletResponse,
                                             Object o,
                                             Exception e) {
            try {
                httpServletResponse.sendError(511,"我喜欢的错误嗷！！！");
            } catch (IOException ioException) {
                ioException.printStackTrace();
            }
    
            return new ModelAndView();
        }
    }
    ```

- **ErrorViewResolver**  实现自定义处理异常；

  - response.sendError 。error请求就会转给controller
  - 你的异常没有任何人能处理。tomcat底层 response.sendError。error请求就会转给controller
  - **basicErrorController 要去的页面地址是** **ErrorViewResolver**  ；





#### 3、异常处理自动配置原理

- **ErrorMvcAutoConfiguration  自动配置异常处理规则**
  - **容器中的组件：类型：DefaultErrorAttributes ->** **id：errorAttributes**

    - **public class** **DefaultErrorAttributes** **implements** **ErrorAttributes**, **HandlerExceptionResolver**
    - **DefaultErrorAttributes**：定义错误页面中可以包含哪些数据。

    - ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606044430037-8d599e30-1679-407c-96b7-4df345848fa4.png)
    - ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606044487738-8cb1dcda-08c5-4104-a634-b2468512e60f.png)
  - **容器中的组件：类型：**BasicErrorController --> id：basicErrorController（json+白页 适配响应）**
    - **处理默认** **/error 路径的请求；页面响应** **new** ModelAndView(**"error"**, model)；
    - **容器中有组件 View**->**id是error**；（响应默认错误页）

    - 容器中放组件 **BeanNameViewResolver（视图解析器）；按照返回的视图名作为组件的id去容器中找View对象。**

  - **容器中的组件：**类型：**DefaultErrorViewResolver -> id：**conventionErrorViewResolver

    - 如果发生错误，会以HTTP的状态码 作为视图页地址（viewName），找到真正的页面
    - error/404、5xx.html



- 如果想要返回页面；就会找error视图【**StaticView**】。(默认是一个白页)





![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606043870164-3770e116-344f-448e-8bff-8f32438edc9a.png)写出去json

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606043904074-50b7f088-2d2b-4da5-85e2-0a756da74dca.png) 错误页





#### 4、异常处理步骤流程

1、执行目标方法，目标方法运行期间有任何异常都会被catch、而且标志当前请求结束；并且用 **dispatchException** 进行封装

2、进入视图解析流程（页面渲染？） 

processDispatchResult(processedRequest, response, mappedHandler, **mv**, **dispatchException**);

3、**mv** = **processHandlerException**；处理handler发生的异常，处理完成返回ModelAndView；

- 1、遍历所有的 **handlerExceptionResolvers，看谁能处理当前异常【HandlerExceptionResolver处理器异常解析器】**
- ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606047252166-ce71c3a1-0e0e-4499-90f4-6d80014ca19f.png)
- **2、系统默认的  异常解析器；**
- ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606047109161-c68a46c1-202a-4db1-bbeb-23fcae49bbe9.png)

  - **1、DefaultErrorAttributes先来处理异常。把异常信息保存到request域，并且返回null；**
  - **2、默认没有任何人能处理异常，所以异常会被抛出**

    - **1、如果没有任何人能处理最终底层就会发送 /error 请求。会被底层的BasicErrorController处理**
    - **2、解析错误视图；遍历所有的**  **ErrorViewResolver  看谁能解析。**
    - ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606047900473-e31c1dc3-7a5f-4f70-97de-5203429781fa.png)
    - **3、默认的** **DefaultErrorViewResolver ,作用是把响应状态码作为错误页的地址，error/500.html** 
    - **4、模板引擎最终响应这个页面** **error/500.html** 
- 总结：出了错误的时候，系统会先去找有没有什么方法是能够处理异常的（如果自己没写的话，就找不到，返回null），处理不了的话，当然就得告诉用户出了问题了嗷orz。系统就会默认发送/error的请求，通过ErrorViewResolver，返回对应的出错的视图给用户展示。



### 9、Web原生组件注入（Servlet、Filter、Listener）

#### 1、使用Servlet API

@ServletComponentScan(basePackages = **"com.atguigu.admin"**) :指定原生Servlet组件都放在那里(位于主配置类中)

@WebServlet(urlPatterns = **"/my"**)：效果：直接响应，**没有经过Spring的拦截器？**（位于编写的Servlet的相应类中，这个Servlet应该继承HttpServlet嗷！！！）

@WebFilter(urlPatterns={**"/css/\*"**,**"/images/\*"**})

@WebListener

```java
package com.tuzi.servlet;


import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(urlPatterns = "/my")
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("这是兔兔的测试嗷！！！");
    }
}
```

```java
package com.tuzi.servlet;

import lombok.extern.slf4j.Slf4j;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

@Slf4j
@WebListener
public class MyListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        log.info("MyListener监听到项目初始化完成");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        log.info("MyListener监听到项目销毁");
    }
}
```

```java
package com.tuzi.servlet;

import lombok.extern.slf4j.Slf4j;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

@Slf4j
@WebFilter(urlPatterns = {"/css/*","/images/*"}) //拦截的对象
public class MyFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("MyFilter初始化完成");
    }

    @Override
    public void destroy() {
        log.info("MyFilter销毁");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        log.info("MyFilter工作");
        filterChain.doFilter(servletRequest,servletResponse);
    }
}
```



- 推荐可以这种方式；

- 注意：Servlet下的写法是单星，Spring中的写法是双星





扩展：DispatchServlet 如何注册进来

- 容器中自动配置了  DispatcherServlet  属性绑定到 WebMvcProperties；对应的配置文件配置项是 **spring.mvc。**
- **通过** **ServletRegistrationBean**<DispatcherServlet> 把 DispatcherServlet  配置进来。

- 默认映射的是 / 路径。

`spring.mvc.servlet.path=/mvc/`

- 可以通过上面这种方式来配置对应的路径嗷！！！（DispatcherServlet对应的拦截的路径嗷！！！）

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606284869220-8b63d54b-39c4-40f6-b226-f5f095ef9304.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

Tomcat-Servlet；

多个Servlet都能处理到同一层路径，精确优选原则(有点像计算机网络中学习到的最长前缀匹配原则。)

A： /my/

B： /my/1





#### 2、使用RegistrationBean

- 就相当于不用注解啦，直接再构造一个类，将我们写好的Servlet加入到容器中就可以生效了嗷！！！

```java
`ServletRegistrationBean`, `FilterRegistrationBean`, and `ServletListenerRegistrationBean
@Configuration
public class MyRegistConfig {

    @Bean
    public ServletRegistrationBean myServlet(){
        MyServlet myServlet = new MyServlet();

        return new ServletRegistrationBean(myServlet,"/my","/my02");
    }


    @Bean
    public FilterRegistrationBean myFilter(){

        MyFilter myFilter = new MyFilter();
//        return new FilterRegistrationBean(myFilter,myServlet());这就默认拦截和myServlet一样的路径嗷！
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(myFilter);
        filterRegistrationBean.setUrlPatterns(Arrays.asList("/my","/css/*"));
        return filterRegistrationBean;
    }

    @Bean
    public ServletListenerRegistrationBean myListener(){
        MySwervletContextListener mySwervletContextListener = new MySwervletContextListener();
        return new ServletListenerRegistrationBean(mySwervletContextListener);
    }
}
```

- 一定要记得@Bean，@Bean才是把对应返回的组件放到容器中来。
- 这里用单实例的最好嗷，一个就够了，不然可能会有很多杂的东西在容器里面嗷！！！

### 10、嵌入式Servlet容器

#### 1、切换嵌入式Servlet容器

- 默认支持的webServer
  - `Tomcat`, `Jetty`, or `Undertow`
  - `ServletWebServerApplicationContext 容器启动寻找ServletWebServerFactory 并引导创建服务器`

- 切换服务器

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606280937533-504d0889-b893-4a01-af68-2fc31ffce9fc.png)

```xml
<dependency>
 <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```





- 原理
  - SpringBoot应用启动发现当前是Web应用。web场景包-导入tomcat
  - web应用会创建一个web版的ioc容器 `ServletWebServerApplicationContext` 
  - `ServletWebServerApplicationContext` 启动的时候寻找 `**ServletWebServerFactory**``（Servlet 的web服务器工厂---> Servlet 的web服务器）` 
  - SpringBoot底层默认有很多的WebServer工厂；`TomcatServletWebServerFactory`, `JettyServletWebServerFactory`, or `UndertowServletWebServerFactory`
  - `底层直接会有一个自动配置类。ServletWebServerFactoryAutoConfiguration`
  - `ServletWebServerFactoryAutoConfiguration导入了ServletWebServerFactoryConfiguration（配置类）`
  - `ServletWebServerFactoryConfiguration 配置类 根据动态判断系统中到底导入了哪个Web服务器的包。（默认是web-starter导入tomcat包），容器中就有 TomcatServletWebServerFactory`
  - `TomcatServletWebServerFactory 创建出Tomcat服务器并启动；TomcatWebServer 的构造器拥有初始化方法initialize---this.tomcat.start();`
  - `内嵌服务器，就是手动把启动服务器的代码调用（tomcat核心jar包存在）`
  - ``

#### 2、定制Servlet容器

- 实现  **WebServerFactoryCustomizer\<ConfigurableServletWebServerFactory> **
  - 把配置文件的值和**ServletWebServerFactory 进行绑定**
  - ![image-20210718160633431](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210718160633.png)

- 修改配置文件 **server.xxx**
- 直接自定义 **ConfigurableServletWebServerFactory** ：

![image-20210718160406922](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210718160407.png)



**xxxxxCustomizer**：定制化器，可以改变xxxx的默认规则

```java
import org.springframework.boot.web.server.WebServerFactoryCustomizer;
import org.springframework.boot.web.servlet.server.ConfigurableServletWebServerFactory;
import org.springframework.stereotype.Component;

@Component
public class CustomizationBean implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory> {
    @Override
    public void customize(ConfigurableServletWebServerFactory server) {
        server.setPort(9000);
    }
}
```





### 11、定制化原理

#### 1、定制化的常见方式 

- 修改配置文件；
- **xxxxxCustomizer；**

- **编写自定义的配置类   xxxConfiguration；+** **@Bean替换、增加容器中默认组件；视图解析器** 
- **Web应用 编写一个配置类实现** **WebMvcConfigurer 即可定制化web功能；+ @Bean给容器中再扩展一些组件**(一个类，搞定所有问题，可以加入@Bean拓展组件，也可以覆盖某些方法，加入自己写的组件嗷！！！)

![image-20210718162631570](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210718162702.png)

```java
@Configuration
public class AdminWebConfig implements WebMvcConfigurer
```

上面这两个就是例子，可以使用实现WebMvcConfigurer加上@Configuration的方式，覆盖一些函数加入某些功能或者是在对应的类中通过@Bean将我们需要的组件放入到容器中来。

- @EnableWebMvc + WebMvcConfigurer —— @Bean  可以全面接管SpringMVC，所有规则全部自己重新配置； 实现定制和扩展功能
  - 原理
  - 1、WebMvcAutoConfiguration  默认的SpringMVC的自动配置功能类。静态资源、欢迎页.....
  - 2、一旦使用 @EnableWebMvc 。会 @Import(DelegatingWebMvcConfiguration.**class**)
  - 3、**DelegatingWebMvcConfiguration** 的 作用，只保证SpringMVC最基本的使用
    - 把所有系统中的 WebMvcConfigurer 拿过来。所有功能的定制都是这些 WebMvcConfigurer  合起来一起生效
    - 自动配置了一些非常底层的组件。**RequestMappingHandlerMapping**、这些组件依赖的组件都是从容器中获取
    - **public class** DelegatingWebMvcConfiguration **extends** **WebMvcConfigurationSupport**
  - 4、**WebMvcAutoConfiguration** 里面的配置要能生效 必须  @ConditionalOnMissingBean(**WebMvcConfigurationSupport**.**class**)
  - 5、@EnableWebMvc  导致了 **WebMvcAutoConfiguration  没有生效。**
  - ... ...



#### 2、原理分析套路

**场景starter** **- xxxxAutoConfiguration - 导入xxx组件 - 绑定xxxProperties --** **绑定配置文件项** 







## 数据访问：

### 1、SQL

#### 1、数据源的自动配置-**HikariDataSource**

##### 1、导入JDBC场景

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>
```

- 导入的starter中包含了以下的内容：

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606366100317-5e0199fa-6709-4d32-bce3-bb262e2e5e6a.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)





- 数据库驱动？

为什么导入JDBC场景，官方不导入驱动？官方不知道我们接下要操作什么数据库。如果我们使用的是MySql，我们还要引入如下的MySQL驱动：

数据库版本要和驱动版本对应

```xml
默认版本：
<mysql.version>8.0.22</mysql.version>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <!--<version>5.1.49</version>-->
</dependency>
想要修改版本
1、直接依赖引入具体版本（maven的就近依赖原则）
2、重新声明版本（maven的属性的就近优先原则）
<properties>
    <java.version>1.8</java.version>
    <mysql.version>5.1.49</mysql.version>
</properties>
```

![image-20210719162954595](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210719162954.png)



#### 2、分析自动配置

##### 1、自动配置的类

- DataSourceAutoConfiguration ： 数据源的自动配置
  - 修改数据源相关的配置：**spring.datasource**
  - **数据库连接池的配置，是自己容器中没有DataSource才自动配置的**
  - 底层配置好的连接池是：**HikariDataSource**

```java
@Configuration(proxyBeanMethods = false)
@Conditional(PooledDataSourceCondition.class)
@ConditionalOnMissingBean({ DataSource.class, XADataSource.class })
@Import({ DataSourceConfiguration.Hikari.class, DataSourceConfiguration.Tomcat.class,
		DataSourceConfiguration.Dbcp2.class, DataSourceConfiguration.OracleUcp.class,
		DataSourceConfiguration.Generic.class, DataSourceJmxConfiguration.class })
protected static class PooledDataSourceConfiguration
```

- DataSourceTransactionManagerAutoConfiguration： 事务管理器的自动配置
- JdbcTemplateAutoConfiguration： **JdbcTemplate的自动配置，可以来对数据库进行crud**
  - 可以修改这个配置项@ConfigurationProperties(prefix = **"spring.jdbc"**) 来修改JdbcTemplate
  - @Bean@Primary    JdbcTemplate；容器中有这个组件

- JndiDataSourceAutoConfiguration： jndi的自动配置
- XADataSourceAutoConfiguration： 分布式事务相关的





#### 3、修改配置项

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db_account
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver
```



#### 4、测试

```java
@Slf4j
@SpringBootTest
class Boot05WebAdminApplicationTests {

    @Autowired
    JdbcTemplate jdbcTemplate;


    @Test
    void contextLoads() {

//        jdbcTemplate.queryForObject("select * from account_tbl")
//        jdbcTemplate.queryForList("select * from account_tbl",)
        Long aLong = jdbcTemplate.queryForObject("select count(*) from account_tbl", Long.class);
        log.info("记录总数：{}",aLong);
    }

}
```

### 2、使用Druid数据源

#### 1、druid官方github地址

https://github.com/alibaba/druid



整合第三方技术的两种方式

- 自定义
- 找starter



#### 2、自定义方式

[这一课讲的十分好，如何把druid手动整合。如何通过官方的Spring文档来导入到SpringBoot中嗷！！！](https://www.bilibili.com/video/BV19K4y1L7MT/?p=61&spm_id_from=pageDriver)

##### 1、创建数据源

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.17</version>
</dependency>

//下面是Spring中常见的Druid配置，在boot中，我们通过配置类的方式来对于数据源进行配置嗷！！！
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
		destroy-method="close">
<property name="url" value="${jdbc.url}" />
<property name="username" value="${jdbc.username}" />
<property name="password" value="${jdbc.password}" />
<property name="maxActive" value="20" />
<property name="initialSize" value="1" />
<property name="maxWait" value="60000" />
<property name="minIdle" value="1" />
<property name="timeBetweenEvictionRunsMillis" value="60000" />
<property name="minEvictableIdleTimeMillis" value="300000" />
<property name="testWhileIdle" value="true" />
<property name="testOnBorrow" value="false" />
<property name="testOnReturn" value="false" />
<property name="poolPreparedStatements" value="true" />
<property name="maxOpenPreparedStatements" value="20" />
```

- 官网给的所有的配置文件，都是可以通过配置类以及配置类的相关方法进行配置的嗷！！！可以去看druid的官方文档嗷！！！

![image-20210719170200031](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210719170200.png)

- 如果我们这里使用了@ConfigurationProperities，这些属性也可以通过在配置文件中进行配置来进行自动的绑定嗷！！！

![image-20210719170433151](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210719170434.png)

- 官方文档中对于什么Servlet的配置啊之类的，我们只需要翻译成boot中对应的内容，类似于ServletRegistrationBean，将对应的组件加入到容器中即可嗷！！！

















##### 2、StatViewServlet

StatViewServlet的用途包括：

- 提供监控信息展示的html页面
- 提供监控信息的JSON API

```xml
<servlet>
	<servlet-name>DruidStatView</servlet-name>
	<servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>DruidStatView</servlet-name>
	<url-pattern>/druid/*</url-pattern>
</servlet-mapping>
```



##### 3、StatFilter

用于统计监控信息；如SQL监控、URI监控

```xml
需要给数据源中配置如下属性；可以允许多个filter，多个用，分割；如：

<property name="filters" value="stat,slf4j" />
```

系统中所有filter：

| 别名          | Filter类名                                              |
| ------------- | ------------------------------------------------------- |
| default       | com.alibaba.druid.filter.stat.StatFilter                |
| stat          | com.alibaba.druid.filter.stat.StatFilter                |
| mergeStat     | com.alibaba.druid.filter.stat.MergeStatFilter           |
| encoding      | com.alibaba.druid.filter.encoding.EncodingConvertFilter |
| log4j         | com.alibaba.druid.filter.logging.Log4jFilter            |
| log4j2        | com.alibaba.druid.filter.logging.Log4j2Filter           |
| slf4j         | com.alibaba.druid.filter.logging.Slf4jLogFilter         |
| commonlogging | com.alibaba.druid.filter.logging.CommonsLogFilter       |

**慢SQL记录配置**

```xml
<bean id="stat-filter" class="com.alibaba.druid.filter.stat.StatFilter">
    <property name="slowSqlMillis" value="10000" />
    <property name="logSlowSql" value="true" />
</bean>

使用 slowSqlMillis 定义慢SQL的时长
```









#### 3、使用官方starter方式

##### 1、引入druid-starter

```xml
<dependency>
	<groupId>com.alibaba</groupId>
	<artifactId>druid-spring-boot-starter</artifactId>
	<version>1.1.17</version>
</dependency>
```



##### 2、分析自动配置

- 扩展配置项 **spring.datasource.druid**
- DruidSpringAopConfiguration.**class**,   监控SpringBean的；配置项：**spring.datasource.druid.aop-patterns**

- DruidStatViewServletConfiguration.**class**, 监控页的配置：**spring.datasource.druid.stat-view-servlet；默认开启**
-  DruidWebStatFilterConfiguration.**class**, web监控配置；**spring.datasource.druid.web-stat-filter；默认开启**

- DruidFilterConfiguration.**class**}) 所有Druid自己filter的配置

```java
private static final String FILTER_STAT_PREFIX = "spring.datasource.druid.filter.stat";
private static final String FILTER_CONFIG_PREFIX = "spring.datasource.druid.filter.config";
private static final String FILTER_ENCODING_PREFIX = "spring.datasource.druid.filter.encoding";
private static final String FILTER_SLF4J_PREFIX = "spring.datasource.druid.filter.slf4j";
private static final String FILTER_LOG4J_PREFIX = "spring.datasource.druid.filter.log4j";
private static final String FILTER_LOG4J2_PREFIX = "spring.datasource.druid.filter.log4j2";
private static final String FILTER_COMMONS_LOG_PREFIX = "spring.datasource.druid.filter.commons-log";
private static final String FILTER_WALL_PREFIX = "spring.datasource.druid.filter.wall";
```

- 上面这些是官方已经为我们配置好了的，我们只需要在配置文件中对于其进行配置即可嗷！！！

##### 3、配置示例

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db_account
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver

    druid:
      aop-patterns: com.atguigu.admin.*  #监控SpringBean
      filters: stat,wall     # 底层开启功能，stat（sql监控），wall（防火墙）

      stat-view-servlet:   # 配置监控页功能
        enabled: true
        login-username: admin
        login-password: admin
        resetEnable: false #这个就是重置按钮

      web-stat-filter:  # 监控web
        enabled: true
        urlPattern: /*
        exclusions: '*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*'


      filter:
        stat:    # 对上面filters里面的stat的详细配置
          slow-sql-millis: 1000
          logSlowSql: true
          enabled: true
        wall:
          enabled: true
          config:
            drop-table-allow: false
```

SpringBoot配置示例

https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter



配置项列表[https://github.com/alibaba/druid/wiki/DruidDataSource%E9%85%8D%E7%BD%AE%E5%B1%9E%E6%80%A7%E5%88%97%E8%A1%A8](https://github.com/alibaba/druid/wiki/DruidDataSource配置属性列表)



### 3、整合MyBatis操作

https://github.com/mybatis

starter

SpringBoot官方的Starter：spring-boot-starter-*

第三方的： *-spring-boot-starter

```xml
<dependency>
	<groupId>org.mybatis.spring.boot</groupId>
	<artifactId>mybatis-spring-boot-starter</artifactId>
	<version>2.1.4</version>
</dependency>
```

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606704096118-53001250-a04a-4210-80ee-6de6a370be2e.png)

#### 1、配置模式

- 全局配置文件
- SqlSessionFactory: 自动配置好了

- SqlSession：自动配置了 **SqlSessionTemplate 组合了SqlSession**
- @Import(**AutoConfiguredMapperScannerRegistrar**.**class**）；

- Mapper： 只要我们写的操作MyBatis的接口标准了 **@Mapper 就会被自动扫描进来**

```java
@EnableConfigurationProperties(MybatisProperties.class) ： MyBatis配置项绑定类。
@AutoConfigureAfter({ DataSourceAutoConfiguration.class, MybatisLanguageDriverAutoConfiguration.class })
public class MybatisAutoConfiguration{}

@ConfigurationProperties(prefix = "mybatis")
public class MybatisProperties
```

可以修改配置文件中 mybatis 开始的所有；



```yaml
# 配置mybatis规则
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml  #全局配置文件位置
  mapper-locations: classpath:mybatis/mapper/*.xml  #sql映射文件位置
```
- 上面是配置文件，下面是和以前的MyBatis用法一样的Mapper映射文件，需要搭配mapper抽象类接口一起使用嗷！！！
- 注意，以下配置文件不是我们所说的全局配置文件，全局配置文件可以上官方文档中拷贝出来嗷！！！
- 注意Mapper抽象类上面要加上@Mapper注解。

```xml
Mapper接口--->绑定Xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.atguigu.admin.mapper.AccountMapper">
<!--    public Account getAcct(Long id); -->
    <select id="getAcct" resultType="com.atguigu.admin.bean.Account">
        select * from  account_tbl where  id=#{id}
    </select>
</mapper>
```



- 全局配置文件：

![image-20210719203601908](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210719203602.png)

可以在配置文件中开启对应的驼峰命名规则。同理，xml太麻烦，在我们的配置文件中有没有默认绑定上对应的规则呢？如下所示嗷！！！





配置 **private** Configuration **configuration**; mybatis.**configuration下面的所有，就是相当于改mybatis全局配置文件中的值**



```yaml
# 配置mybatis规则
mybatis:
#  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml
  configuration:
    map-underscore-to-camel-case: true
    
#可以不写全局；配置文件，所有全局配置文件的配置都放在configuration配置项中即可
```

- 下面的配置，和我们上面指定的全局配置文件不能够同时存在嗷！！！



- MyBatis使用规则：
  - 导入mybatis官方starter
  - 编写mapper接口。标准@Mapper注解

  - 编写sql映射文件并绑定mapper接口
  - 在application.yaml中指定Mapper配置文件的位置，以及指定全局配置文件的信息 （建议；**配置在mybatis.configuration**中，不用全局配置文件嗷，太麻烦啦！！！）





#### 2、纯注解模式

```Java
@Mapper
public interface CityMapper {

    @Select("select * from city where id=#{id}")
    public City getById(Long id);

    public void insert(City city);

}
```







#### 3、混合模式

```java
@Mapper
public interface CityMapper {

    @Select("select * from city where id=#{id}")
    public City getById(Long id);

    public void insert(City city);

}
```

- 复杂的专门放在对应的Mapper文件中即可。

- 拿到对应的Id：

![image-20210719205324303](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210719205324.png)

可以通过注解的形式来拿到对应的id嗷！！！并且可以把拿到的id放入对应的City的属性中。

- 这个例子在MyBatis的官方文档中的spring-boot-starter的Quick Start中嗷！！！

**最佳实战：**

- 引入mybatis-starter
- **配置application.yaml中，指定mapper-location位置即可**

- 编写Mapper接口并标注@Mapper注解
- 简单方法直接注解方式

- 复杂方法编写mapper.xml进行绑定映射
- *@MapperScan("com.atguigu.admin.mapper") 简化，其他的接口就可以不用标注@Mapper注解*（专门整一个mapper的包出来，里面全放对应的文件，就可以不在每个类上面标注mapper了，只用扫描整个包就好了。建议还是用Mapper来标注上嗷！！！）



### 4、整合 MyBatis-Plus 完成CRUD

#### 1、什么是MyBatis-Plus

[MyBatis-Plus](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis](http://www.mybatis.org/mybatis-3/) 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

[mybatis plus 官网](https://baomidou.com/)

建议安装 **MybatisX** 插件 



#### 2、整合MyBatis-Plus

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.1</version>
</dependency>
```

自动配置

- MybatisPlusAutoConfiguration 配置类，MybatisPlusProperties 配置项绑定。**mybatis-plus：xxx 就是对****mybatis-plus的定制**
- **SqlSessionFactory 自动配置好。底层是容器中默认的数据源**

- **mapperLocations 自动配置好的。有默认值。**classpath\*:/mapper/\**/\*.xml；任意包的类路径下的所有mapper文件夹下任意路径下的所有xml都是sql映射文件。  建议以后sql映射文件，放在 mapper下。
- **容器中也自动配置好了** **SqlSessionTemplate**

- **@Mapper 标注的接口也会被自动扫描；建议直接** @MapperScan(**"com.tuzi.mapper"**) 批量扫描就行





**优点：**

-  只需要我们的Mapper继承 **BaseMapper** 就可以拥有crud能力

![image-20210720155244605](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720155244.png)

- 相当于上面这样就直接写好了orz





#### 3、CRUD功能

```java
    @GetMapping("/user/delete/{id}")
    public String deleteUser(@PathVariable("id") Long id,
                             @RequestParam(value = "pn",defaultValue = "1")Integer pn,
                             RedirectAttributes ra){

        userService.removeById(id);

        ra.addAttribute("pn",pn);
        return "redirect:/dynamic_table";
    }


    @GetMapping("/dynamic_table")
    public String dynamic_table(@RequestParam(value="pn",defaultValue = "1") Integer pn,Model model){
        //表格内容的遍历
//        response.sendError
//     List<User> users = Arrays.asList(new User("zhangsan", "123456"),
//                new User("lisi", "123444"),
//                new User("haha", "aaaaa"),
//                new User("hehe ", "aaddd"));
//        model.addAttribute("users",users);
//
//        if(users.size()>3){
//            throw new UserTooManyException();
//        }
        //从数据库中查出user表中的用户进行展示

        //构造分页参数
        Page<User> page = new Page<>(pn, 2);
        //调用page进行分页
        Page<User> userPage = userService.page(page, null);


//        userPage.getRecords()
//        userPage.getCurrent()
//        userPage.getPages()


        model.addAttribute("users",userPage);

        return "table/dynamic_table";
    }
```



```java
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper,User> implements UserService {


}

public interface UserService extends IService<User> {

}
```



#### 4、工程问题

- 代码如下：

```java
package com.tuzi.bean;

import com.baomidou.mybatisplus.annotation.TableField;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.ToString;

@Data
@ToString
@NoArgsConstructor
@AllArgsConstructor
public class User {

    //下面是用户登录的字段
    /**
     * 所有属性都应该在数据库中嗷！！！
     */
    @TableField(exist = false)
    private String userName;
    @TableField(exist = false)
    private String password;

    //以下是数据库的字段
    private Long id;
    private String name;
    private Integer age;
    private String email;

    public User(String s1, String s) {
        this.userName = s1;
        this.password = s;
    }
}
//不在表中的属性可以使用plus的注解来注标记嗷！！！
```



- 属性：

```java
public interface UserMapper extends BaseMapper<User> {
}
```

上面这种写法默认是去数据库种寻找同名的User表，如果表的名字和这个类的名字不一样，就要采用注解来注释，帮助MyBatisPlus找到这种表：

`@TableName("user_tbl")`，这个注解是添加在User这个类上，而不是Mapper类上嗷！！！

- Controller -> Service -> Mapper(Dao层)，先有接口，后有实现类，注入接口来进行编写的嗷！！！（对于Service层来说的嗷！！！）
- `ctrl + alt + l`可以格式化html文件格式嗷
- Service层结构：

```java
package com.tuzi.service;

import com.baomidou.mybatisplus.extension.service.IService;
import com.tuzi.bean.User;

public interface UserService extends IService<User> {
}
```

- 对应的Impl的代码：

```java
package com.tuzi.service.impl;

import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.tuzi.bean.User;
import com.tuzi.mapper.UserMapper;
import com.tuzi.service.UserService;
import org.springframework.stereotype.Service;

@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
}
```

- 上面这个Impl就是结合之前的Mapper和User等一起使用的嗷，默认也有很多功能。而且@Service了之后，就可以在Controller中自动注入来使用了嗷！！！

```java
@Autowired
UserService userService;

@GetMapping("/dynamic_table")
public String dynamic_table(Model model){
	List<User> list = userService.list(); //上面的抽象类和实现并未自己实现方法，但是可以使用默认的方法来实现功能嗷！！！
	model.addAttribute("users",list);
	return "table/dynamic_table";
}
```

- 原理一样的，Mapper(Dao层)先打好（继承BasePackage，传入Bean，就有默认的Dao层方法），然后处理Service层业务（可以是空的接口继承IService传入Bean，实现类则继承ServiceImpl，传入Mapper和对应的Bean，实现类要@Service，然后在Controller中就可以通过自动注入的方式，调用Service层的功能实现服务嗷！！！）
- 实例后续：

![image-20210720165749288](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720165749.png)

![image-20210720165822715](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720165822.png)

总的来说就是分页条是通过JavaScript来实现的，我们可以把相应的JS注释掉，从我们的HTML中审查元素手动取出我们的分页条，然后添加到我们的页面中，利用后端的数据库传入的数据再次来实现分页条。

- 分页数据查询：`userService.Page()`

![image-20210720172714217](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720172721.png)

系统内部设置好了，多少页，每页多少条数据。拿到Page之后，Page中还有其他的属性嗷！！！

![image-20210720172918768](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720172918.png)

- 直接把上面得到的Page对象放入Model中就好了嗷！！！
- 流程：用户点击第几页 -> 后端收到之后传入对应的参数 -> 返回page对象 -> 前端拿到Page对象取值进行渲染。





- ![image-20210720203340544](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720203340.png)
- 上面这个东西在thymeleaf文档中都有嗷！！！这里就是在路径上添加变量的方法嗷！！！

![image-20210720203523780](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720203523.png)







#### 5、带翻页的列表展示功能

- ![image-20210720192137023](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720192137.png)注释掉前端JS实现的翻页功能。
- ![image-20210720192211048](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720192211.png)从JS文件渲染的页面中，提取出我们要的翻页的组件，并将其加入到我们的页面中来。
- 放入对应的page全局变量，方便我们后续取值：

![image-20210720192049444](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720192049.png)

- 修改我们的HTML页面：

![image-20210720192235319](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720192235.png)



![image-20210720192312200](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720192312.png)



- 除此之外，我们还需要引入对应的插件：

```java
package com.tuzi.config;


import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MyBatisConfig {
    // 最新版
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
        // paginationInterceptor.setOverflow(false);
        // 设置最大单页限制数量，默认 500 条，-1 不受限制
        // paginationInterceptor.setLimit(500);
        // 开启 count 的 join 优化,只针对部分 left join
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        //这是分页拦截器
        PaginationInnerInterceptor paginationInnerInterceptor = new PaginationInnerInterceptor();
        paginationInnerInterceptor.setOverflow(true);
        paginationInnerInterceptor.setMaxLimit(500L);

        interceptor.addInnerInterceptor(paginationInnerInterceptor);
        return interceptor;
    }
}	//一定要记得加上@Configuration
```

- 接着对于页面逻辑进行进一步的修改：









### 5、NoSQL

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、**缓存**和消息中间件。 它支持多种类型的数据结构，如 [字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://www.redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://www.redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://www.redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://www.redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://www.redis.cn/topics/lru-cache.html)，[事务（transactions）](http://www.redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://www.redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://www.redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://www.redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）。

#### 1、Redis自动配置

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606745732785-17d1227a-75b9-4f00-a3f1-7fc4137b5113.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



自动配置：

- RedisAutoConfiguration 自动配置类。RedisProperties 属性类 --> **spring.redis.xxx是对redis的配置**
- 连接工厂是准备好的。**Lettuce**ConnectionConfiguration、**Jedis**ConnectionConfiguration

- **自动注入了RedisTemplate**<**Object**, **Object**> ： xxxTemplate；
- **自动注入了StringRedisTemplate；k：v都是String**

- **key：value**
- **底层只要我们使用** **StringRedisTemplate、**RedisTemplate就可以操作redis





**redis环境搭建**

**1、阿里云按量付费redis。经典网络**

**2、申请redis的公网连接地址**

**3、修改白名单  允许0.0.0.0/0 访问**







#### 2、RedisTemplate与Lettuce



```java
@Test
void testRedis(){
    ValueOperations<String, String> operations = redisTemplate.opsForValue();

    operations.set("hello","world");

    String hello = operations.get("hello");
    System.out.println(hello);
}
```









#### 3、切换至jedis

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<!--导入jedis-->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```


```yaml
spring:
  redis:
      host: r-bp1nc7reqesxisgxpipd.redis.rds.aliyuncs.com
      port: 6379
      password: lfy:Lfy123456
      client-type: jedis
      jedis:
        pool:
          max-active: 10
```



- 二者区别：![image-20210720211242900](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720211243.png)
- 计数功能的实现：

1. ![image-20210720211446266](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720211446.png)
2. 和以前一样直接在对应的Config中new出我们写的上面这个拦截器并且加入到容器中可以吗？不可以，因为我们自己new的，里面的@AutoWired没法用啊orz，只有容器中的组件，Spring才会操作注解。那怎么办？
3. 你自动注入啊，这个配置类本身就是@Component，你先在对应的Config类中，自动注入我们放入容器中的这个Interceptor，再用registry注册给对应的管理器就行啦！！！
4. ![image-20210720212207022](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210720212207.png)

自动注入对应的redisTemplate，然后使用对应的方法取出对应的值之后，放入model中，即可以被前端页面获取到。







## 单元测试：

[JUnit5官方文档](https://doczhcn.gitbook.io/junit5/)


### 1、JUnit5 的变化 

Spring Boot 2.2.0 版本开始引入 JUnit 5 作为单元测试默认库

作为最新版本的JUnit框架，JUnit5与之前版本的Junit框架有很大的不同。由三个不同子项目的几个不同模块组成。

JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage

JUnit Platform: Junit Platform是在JVM上启动测试框架的基础，不仅支持Junit自制的测试引擎，其他测试引擎也都可以接入。

JUnit Jupiter: JUnit Jupiter提供了JUnit5的新的编程模型，是JUnit5新特性的核心。内部 包含了一个测试引擎，用于在Junit Platform上运行。

JUnit Vintage: 由于JUint已经发展多年，为了照顾老的项目，JUnit Vintage提供了兼容JUnit4.x,Junit3.x的测试引擎。



![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606796395719-eb57ab48-ae44-45e5-8d2e-c4d507aff49a.png?x-oss-process=image%2Fresize%2Cw_683)



注意：

SpringBoot 2.4 以上版本移除了默认对 Vintage 的依赖。如果需要兼容junit4需要自行引入（不能使用junit4的功能 @Test）

**JUnit 5’s Vintage Engine Removed from spring-boot-starter-test,如果需要继续兼容junit4需要自行引入vintage**

```xml
<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```







orgjunit.jupiterjunit-jupiter:5.7.0test)

iorgjunitjupiterjunit-jupiterapi5.7.0(

org.apiguardian:apiguardian-ap1.1.0

org.opentest4i:opentest4j:1.2.0(test)

orgjunit.platfommjunit-platform-commons1.7.0()

orgjunitjupiterjuiit-jupiter-params5.7.0(est)

dorgjunit.jupiterjunit-jupiterengine.7.()

org.apiguardian:apiguadinai.1.mitdoulicate)

lorgjunit.platformjunit-platform-engine;1.7.0(test)

org.apiguardianapiguardianapi.1.1.0(tsomittedforduplic

org.opentest4iopentes41.2.0ioduica)

org.junit.platfomjuitlafomcommo.

org.junitjupiterjunituitapi..duic

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606797616337-e73010e9-9cac-496d-a177-64b677af5a3d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <scope>test</scope>
</dependency>
```

现在版本：

```java
@SpringBootTest
class Boot05WebAdminApplicationTests {
    @Test
    void contextLoads() {

    }
}
```





以前：

@SpringBootTest + @RunWith(SpringTest.class)





SpringBoot整合Junit以后：

- 编写测试方法：@Test标注（注意需要使用junit5版本的注解）

- Junit类具有Spring的功能，@Autowired、比如 @Transactional 标注测试方法，测试完成后自动回滚



### 2、JUnit5常用注解 

- 有官方文档嗷！！！
- 如果需要SpringBoot中的功能，例如自动注入功能，就需要@SpringBootTest注解嗷！！！

JUnit5的注解与JUnit4的注解有所变化

https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations

- @Test :表示方法是测试方法。但是与JUnit4的@Test不同，他的职责非常单一不能声明任何属性，拓展的测试将会由Jupiter提供额外测试

- @ParameterizedTest :表示方法是参数化测试，下方会有详细介绍

- @RepeatedTest :表示方法可重复执行，下方会有详细介绍

- @DisplayName :为测试类或者测试方法设置展示名称

![image-20210721154738122](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721154738.png)

- @BeforeEach :表示在每个单元测试之前执行

- @AfterEach :表示在每个单元测试之后执行

- @BeforeAll :表示在所有单元测试之前执行

- @AfterAll :表示在所有单元测试之后执行

上面这个就类似于我们软件测试中学习到的方法的升级版本嗷！！！

![image-20210721155156226](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721155156.png)

- @Tag :表示单元测试类别，类似于JUnit4中的@Categories

- @Disabled :表示测试类或测试方法不执行，类似于JUnit4中的@Ignore

- @Timeout :表示测试方法运行如果超过了指定时间将会返回错误

![image-20210721155459034](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721155459.png)

- @ExtendWith :为测试类或测试方法提供扩展类引用

```java
import org.junit.jupiter.api.Test; //注意这里使用的是jupiter的Test注解！！


public class TestDemo {

  @Test
  @DisplayName("第一次测试")
  public void firstTest() {
      System.out.println("hello world");
  }
}
```

- @RepeatedTest，重复测试，可以设置这个方法重复测试的次数：

![image-20210721155811853](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721155812.png)





### 3、断言（assertions） 

断言（assertions）是测试方法中的核心部分，用来对测试需要满足的条件进行验证。这些断言方法都是 org.junit.jupiter.api.Assertions 的静态方法。JUnit 5 内置的断言可以分成如下几个类别：

检查业务逻辑返回的数据是否合理。

所有的测试运行结束以后，会有一个详细的测试报告；

#### 3.1、简单断言 

用来对单个值进行简单的验证。如：

| 方法            | 说明                                 |
| --------------- | ------------------------------------ |
| assertEquals    | 判断两个对象或两个原始类型是否相等   |
| assertNotEquals | 判断两个对象或两个原始类型是否不相等 |
| assertSame      | 判断两个对象引用是否指向同一个对象   |
| assertNotSame   | 判断两个对象引用是否指向不同的对象   |
| assertTrue      | 判断给定的布尔值是否为 true          |
| assertFalse     | 判断给定的布尔值是否为 false         |
| assertNull      | 判断给定的对象引用是否为 null        |
| assertNotNull   | 判断给定的对象引用是否不为 null      |

```java
@Test
@DisplayName("simple assertion")
public void simple() {
     assertEquals(3, 1 + 2, "simple math");  //最后一个参数是自定义的错误参数嗷！！！
     assertNotEquals(3, 1 + 1);

     assertNotSame(new Object(), new Object());
     Object obj = new Object();
     assertSame(obj, obj);

     assertFalse(1 > 2);
     assertTrue(1 < 2);

     assertNull(null);
     assertNotNull(new Object());
}
```



#### 3.2、数组断言 

通过 assertArrayEquals 方法来判断两个对象或原始类型的数组是否相等

```java
@Test
@DisplayName("array assertion")
public void array() {
    assertArrayEquals(new int[]{1, 2}, new int[] {1, 2});
}
```



#### 3.3、组合断言 

assertAll 方法接受多个 org.junit.jupiter.api.Executable 函数式接口的实例作为要验证的断言，可以通过 lambda 表达式很容易的提供这些断言

```java
@Test
@DisplayName("assert all")
public void all() {
 assertAll("Math",
    	() -> assertEquals(2, 1 + 1),
    	() -> assertTrue(1 > 0)
 	);
}
```



#### 3.4、异常断言 

在JUnit4时期，想要测试方法的异常情况时，需要用@Rule注解的ExpectedException变量还是比较麻烦的。而JUnit5提供了一种新的断言方式Assertions.assertThrows() ,配合函数式编程就可以进行使用。(断定业务逻辑一定会出现某些异常，如果正常执行则会报错嗷！！！)

```java
@Test
@DisplayName("异常测试")
public void exceptionTest() {
    ArithmeticException exception = Assertions.assertThrows(
           //扔出断言异常
            ArithmeticException.class, () -> System.out.println(1 % 0));

}
```



#### 3.5、超时断言 

Junit5还提供了Assertions.assertTimeout() 为测试方法设置了超时时间

- 断定业务逻辑一定在我指定的时间内完成

```java
@Test
@DisplayName("超时测试")
public void timeoutTest() {
    //如果测试方法时间超过1s将会异常
    Assertions.assertTimeout(Duration.ofMillis(1000), () -> Thread.sleep(500));
}
```



#### 3.6、快速失败 

通过 fail 方法直接使得测试失败

```java
@Test
@DisplayName("fail")
public void shouldFail() {
    fail("This should fail");
}
```





### 4、前置条件（assumptions） 

JUnit 5 中的前置条件（assumptions【假设】）类似于断言，不同之处在于不满足的断言会使得测试方法失败，而不满足的前置条件只会使得测试方法的执行终止。前置条件可以看成是测试方法执行的前提，当该前提不满足时，就没有继续执行的必要。（即跳过当前的方法不执行剩下的代码嗷！！！）

```java
@DisplayName("前置条件")
public class AssumptionsTest {
 private final String environment = "DEV";
}
    
 @Test
 @DisplayName("simple")
 public void simpleAssume() {
    assumeTrue(Objects.equals(this.environment, "DEV"));
    assumeFalse(() -> Objects.equals(this.environment, "PROD"));
 }
 
 @Test
 @DisplayName("assume then do")
 public void assumeThenDo() {
    assumingThat(
       Objects.equals(this.environment, "DEV"),
       () -> System.out.println("In DEV")
    );
}
```

assumeTrue 和 assumFalse 确保给定的条件为 true 或 false，不满足条件会使得测试执行终止。assumingThat 的参数是表示条件的布尔值和对应的 Executable 接口的实现对象。只有条件满足时，Executable 对象才会被执行；当条件不满足时，测试执行并不会终止。



### 5、嵌套测试 

JUnit 5 可以通过 Java 中的内部类和@Nested 注解实现嵌套测试，从而可以更好的把相关的测试方法组织在一起。在内部类中可以使用@BeforeEach 和@AfterEach 注解，而且嵌套的层次没有限制。

```java
@DisplayName("A stack")
class TestingAStackDemo {

    Stack<Object> stack;

    @Test
    @DisplayName("is instantiated with new Stack()")
    void isInstantiatedWithNew() {
        new Stack<>();
    }

    @Nested
    @DisplayName("when new")
    class WhenNew {

        @BeforeEach
        void createNewStack() {
            stack = new Stack<>();
        }

        @Test
        @DisplayName("is empty")
        void isEmpty() {
            assertTrue(stack.isEmpty());
        }

        @Test
        @DisplayName("throws EmptyStackException when popped")
        void throwsExceptionWhenPopped() {
            assertThrows(EmptyStackException.class, stack::pop);
        }

        @Test
        @DisplayName("throws EmptyStackException when peeked")
        void throwsExceptionWhenPeeked() {
            assertThrows(EmptyStackException.class, stack::peek);
        }

        @Nested
        @DisplayName("after pushing an element")
        class AfterPushing {

            String anElement = "an element";

            @BeforeEach
            void pushAnElement() {
                stack.push(anElement);
            }

            @Test
            @DisplayName("it is no longer empty")
            void isNotEmpty() {
                assertFalse(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when popped and is empty")
            void returnElementWhenPopped() {
                assertEquals(anElement, stack.pop());
                assertTrue(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when peeked but remains not empty")
            void returnElementWhenPeeked() {
                assertEquals(anElement, stack.peek());
                assertFalse(stack.isEmpty());
            }
        }
    }
}
```



![image-20210721165618766](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721165618.png)





- 栈的驱动的规则：

![image-20210721165828334](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721165828.png)

- 明显的层级关系：

![image-20210721165946971](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721165947.png)

### 6、参数化测试 

参数化测试是JUnit5很重要的一个新特性，它使得用不同的参数多次运行测试成为了可能，也为我们的单元测试带来许多便利。



利用@ValueSource等注解，指定入参，我们将可以使用不同的参数进行多次单元测试，而不需要每新增一个参数就新增一个单元测试，省去了很多冗余代码。



@ValueSource: 为参数化测试指定入参来源，支持八大基础类以及String类型,Class类型

@NullSource: 表示为参数化测试提供一个null的入参

@EnumSource: 表示为参数化测试提供一个枚举入参

@CsvFileSource：表示读取指定CSV文件内容作为参数化测试入参

@MethodSource：表示读取指定方法的返回值作为参数化测试入参(注意方法返回需要是一个流)



当然如果参数化测试仅仅只能做到指定普通的入参还达不到让我觉得惊艳的地步。让我真正感到他的强大之处的地方在于他可以支持外部的各类入参。如:CSV,YML,JSON 文件甚至方法的返回值也可以作为入参。只需要去实现ArgumentsProvider接口，任何外部文件都可以作为它的入参。



```java
@ParameterizedTest
@ValueSource(strings = {"one", "two", "three"})
@DisplayName("参数化测试1")
public void parameterizedTest1(String string) {
    System.out.println(string);
    Assertions.assertTrue(StringUtils.isNotBlank(string));
}


@ParameterizedTest
@MethodSource("method")    //指定方法名
@DisplayName("方法来源参数")
public void testWithExplicitLocalMethodSource(String name) {
    System.out.println(name);
    Assertions.assertNotNull(name);
}

static Stream<String> method() {
    return Stream.of("apple", "banana");
}
```



### 7、迁移指南 

在进行迁移的时候需要注意如下的变化：

- 注解在 org.junit.jupiter.api 包中，断言在 org.junit.jupiter.api.Assertions 类中，前置条件在 org.junit.jupiter.api.Assumptions 类中。

- 把@Before 和@After 替换成@BeforeEach 和@AfterEach。

- 把@BeforeClass 和@AfterClass 替换成@BeforeAll 和@AfterAll。

- 把@Ignore 替换成@Disabled。

- 把@Category 替换成@Tag。

- 把@RunWith、@Rule 和@ClassRule 替换成@ExtendWith。



## 指标监控：

### 1、SpringBoot Actuator

#### 1、简介

未来每一个微服务在云上部署以后，我们都需要对其进行监控、追踪、审计、控制等。SpringBoot就抽取了Actuator场景，使得我们每个微服务快速引用即可获得生产级别的应用监控、审计等功能。

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606886483335-697ee1c1-2f69-43ab-bddc-3a038382319c.png)

- 加入了对应的配置之后，我们需要对应配置文件中，关于web暴露的EndPoints进行配置，才能够更加方便的使用嗷！！！
- 使用步骤：
  - 引入依赖
  - 配置文件中暴露出来嗷！！！

#### 2、1.x与2.x的不同



![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606884394162-ac7f2d8e-7abb-44df-9998-fb0f2705f238.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)





#### 3、如何使用

- 引入场景
- 访问 http://localhost:8080/actuator/**

- 暴露所有监控信息为HTTP

```yaml
management:
  endpoints:
    enabled-by-default: true #暴露所有端点信息
    web:
      exposure:
        include: '*'  #以web方式暴露
```

- 测试

http://localhost:8080/actuator/beans

http://localhost:8080/actuator/configprops

http://localhost:8080/actuator/metrics

http://localhost:8080/actuator/metrics/jvm.gc.pause

[http://localhost:8080/actuator/](http://localhost:8080/actuator/metrics)endpointName/detailPath
。。。。。。





#### 4、可视化

https://github.com/codecentric/spring-boot-admin

- 打开官方文档，根据Quick Start中新建一个项目，引入对应的依赖和注解之后，就可以学怎么使用了嗷！！！

### 2、Actuator Endpoint

#### 1、最常使用的端点



| ID                 | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| `auditevents`      | 暴露当前应用程序的审核事件信息。需要一个`AuditEventRepository组件`。 |
| `beans`            | 显示应用程序中所有Spring Bean的完整列表。                    |
| `caches`           | 暴露可用的缓存。                                             |
| `conditions`       | 显示自动配置的所有条件信息，包括匹配或不匹配的原因。         |
| `configprops`      | 显示所有`@ConfigurationProperties`。                         |
| `env`              | 暴露Spring的属性`ConfigurableEnvironment`                    |
| `flyway`           | 显示已应用的所有Flyway数据库迁移。 需要一个或多个`Flyway`组件。 |
| `health`           | 显示应用程序运行状况信息。                                   |
| `httptrace`        | 显示HTTP跟踪信息（默认情况下，最近100个HTTP请求-响应）。需要一个`HttpTraceRepository`组件。 |
| `info`             | 显示应用程序信息。                                           |
| `integrationgraph` | 显示Spring `integrationgraph` 。需要依赖`spring-integration-core`。 |
| `loggers`          | 显示和修改应用程序中日志的配置。                             |
| `liquibase`        | 显示已应用的所有Liquibase数据库迁移。需要一个或多个`Liquibase`组件。 |
| `metrics`          | 显示当前应用程序的“指标”信息。                               |
| `mappings`         | 显示所有`@RequestMapping`路径列表。                          |
| `scheduledtasks`   | 显示应用程序中的计划任务。                                   |
| `sessions`         | 允许从Spring Session支持的会话存储中检索和删除用户会话。需要使用Spring Session的基于Servlet的Web应用程序。 |
| `shutdown`         | 使应用程序正常关闭。默认禁用。                               |
| `startup`          | 显示由`ApplicationStartup`收集的启动步骤数据。需要使用`SpringApplication`进行配置`BufferingApplicationStartup`。 |
| `threaddump`       | 执行线程转储。                                               |





如果您的应用程序是Web应用程序（Spring MVC，Spring WebFlux或Jersey），则可以使用以下附加端点：

| ID           | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| `heapdump`   | 返回`hprof`堆转储文件。                                      |
| `jolokia`    | 通过HTTP暴露JMX bean（需要引入Jolokia，不适用于WebFlux）。需要引入依赖`jolokia-core`。 |
| `logfile`    | 返回日志文件的内容（如果已设置`logging.file.name`或`logging.file.path`属性）。支持使用HTTP`Range`标头来检索部分日志文件的内容。 |
| `prometheus` | 以Prometheus服务器可以抓取的格式公开指标。需要依赖`micrometer-registry-prometheus`。 |





最常用的Endpoint

- **Health：监控状况**
- **Metrics：运行时指标**
- **Loggers：日志记录**





- 默认show-details都是关闭的，需要手动设为开启的，方便我们查看内部的详细信息。![image-20210721224151384](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721224151.png)





#### 2、Health Endpoint

健康检查端点，我们一般用于在云平台，平台会定时的检查应用的健康状况，我们就需要Health Endpoint可以为平台返回当前应用的一系列组件健康状况的集合。

重要的几点：

- health endpoint返回的结果，应该是一系列健康检查后的一个汇总报告
- 很多的健康检查默认已经自动配置好了，比如：数据库、redis等

- 可以很容易的添加自定义的健康检查机制

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606908975702-4f9a3208-15ca-4a78-9f76-939ef986db7e.png)

#### 3、Metrics Endpoint

提供详细的、层级的、空间指标信息，这些信息可以被pull（主动推送）或者push（被动获取）方式得到；

- 通过Metrics对接多种监控系统
- 简化核心Metrics开发

- 添加自定义Metrics或者扩展已有Metrics



![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606909073222-c6e77ca3-4b1c-4f38-a1c6-8614dec4f7bc.png)







#### 4、管理Endpoints

##### 1、开启与禁用Endpoints

- 默认所有的Endpoint除过shutdown都是开启的。
- 需要开启或者禁用某个Endpoint。配置模式为  **management.endpoint.\<endpointName>.enabled = true**

```yaml
management:
  endpoint:
    beans:
      enabled: true
```

- 或者禁用所有的Endpoint然后手动开启指定的Endpoint

```yaml
management:
  endpoints:
    enabled-by-default: false
  endpoint:
    beans:
      enabled: true
    health:
      enabled: true
```





##### 2、暴露Endpoints

支持的暴露方式

- HTTP：默认只暴露**health**和**info** Endpoint
- **JMX**：默认暴露所有Endpoint

- 除过health和info，剩下的Endpoint都应该进行保护访问。如果引入SpringSecurity，则会默认配置安全访问规则

| ID                 | JMX  | Web  |
| ------------------ | ---- | ---- |
| `auditevents`      | Yes  | No   |
| `beans`            | Yes  | No   |
| `caches`           | Yes  | No   |
| `conditions`       | Yes  | No   |
| `configprops`      | Yes  | No   |
| `env`              | Yes  | No   |
| `flyway`           | Yes  | No   |
| `health`           | Yes  | Yes  |
| `heapdump`         | N/A  | No   |
| `httptrace`        | Yes  | No   |
| `info`             | Yes  | Yes  |
| `integrationgraph` | Yes  | No   |
| `jolokia`          | N/A  | No   |
| `logfile`          | N/A  | No   |
| `loggers`          | Yes  | No   |
| `liquibase`        | Yes  | No   |
| `metrics`          | Yes  | No   |
| `mappings`         | Yes  | No   |
| `prometheus`       | N/A  | No   |
| `scheduledtasks`   | Yes  | No   |
| `sessions`         | Yes  | No   |
| `shutdown`         | Yes  | No   |
| `startup`          | Yes  | No   |
| `threaddump`       | Yes  | No   |



手动配置暴露Web端口的对应的EndPoints，方便前后端的交互：

![image-20210721223231643](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210721223231.png)



### 3、定制 Endpoint

#### 1、定制 Health 信息

- 实现接口

```java
import org.springframework.boot.actuate.health.Health;
import org.springframework.boot.actuate.health.HealthIndicator;
import org.springframework.stereotype.Component;

@Component
public class MyHealthIndicator implements HealthIndicator {

    @Override
    public Health health() {
        int errorCode = check(); // perform some specific health check
        if (errorCode != 0) {
            return Health.down().withDetail("Error Code", errorCode).build();
        }
        return Health.up().build();
    }

}

构建Health
Health build = Health.down()
                .withDetail("msg", "error service")
                .withDetail("code", "500")
                .withException(new RuntimeException())
                .build();
management:
    health:
      enabled: true
      show-details: always #总是显示详细信息。可显示每个模块的状态信息
```



- 继承抽象类

```java
@Component
public class MyComHealthIndicator extends AbstractHealthIndicator {

    /**
     * 真实的检查方法
     * @param builder
     * @throws Exception
     */
    @Override
    protected void doHealthCheck(Health.Builder builder) throws Exception {
        //mongodb。  获取连接进行测试
        Map<String,Object> map = new HashMap<>();
        // 检查完成
        if(1 == 2){
//            builder.up(); //健康
            builder.status(Status.UP);
            map.put("count",1);
            map.put("ms",100);
        }else {
//            builder.down();
            builder.status(Status.OUT_OF_SERVICE);
            map.put("err","连接超时");
            map.put("ms",3000);
        }


        builder.withDetail("code",100)
                .withDetails(map);

    }
}
```

![image-20210722160510274](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210722160510.png)

#### 2、定制info信息

常用两种方式：

##### 1、编写配置文件

```yaml
info:
  appName: boot-admin
  version: 2.0.1
  mavenProjectName: @project.artifactId@  #使用@@可以获取maven的pom文件值
  mavenProjectVersion: @project.version@
```

![image-20210722160756108](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210722160756.png)

- 如果要去父类中的信息也可以取到的，`@project.parent.Information@`这种形式就可以取到嗷！！！



##### 2、编写InfoContributor

```java
import java.util.Collections;

import org.springframework.boot.actuate.info.Info;
import org.springframework.boot.actuate.info.InfoContributor;
import org.springframework.stereotype.Component;

@Component
public class ExampleInfoContributor implements InfoContributor {

    @Override
    public void contribute(Info.Builder builder) {
        builder.withDetail("example",
                Collections.singletonMap("key", "value"));
    }

}
```

![image-20210722161321910](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210722161322.png)



http://localhost:8080/actuator/info 会输出以上方式返回的所有info信息

#### 3、定制Metrics信息

##### 1、SpringBoot支持自动适配的Metrics

- JVM metrics, report utilization of:
  - Various memory and buffer pools
  - Statistics related to garbage collection
  - Threads utilization
  - Number of classes loaded/unloaded

- CPU metrics
- File descriptor metrics

- Kafka consumer and producer metrics
- Log4j2 metrics: record the number of events logged to Log4j2 at each level

- Logback metrics: record the number of events logged to Logback at each level
- Uptime metrics: report a gauge for uptime and a fixed gauge representing the application’s absolute start time

- Tomcat metrics (`server.tomcat.mbeanregistry.enabled` must be set to `true` for all Tomcat metrics to be registered)
- [Spring Integration](https://docs.spring.io/spring-integration/docs/5.4.1/reference/html/system-management.html#micrometer-integration) metrics



##### 2、增加定制Metrics

```java
class MyService{
    Counter counter;
    public MyService(MeterRegistry meterRegistry){
         counter = meterRegistry.counter("myservice.method.running.counter");
    }

    public void hello() {
        counter.increment();
        //统计hello被调用的次数，每次被调用就加一
    }
}


//也可以使用下面的方式
@Bean
MeterBinder queueSize(Queue queue) {
    return (registry) -> Gauge.builder("queueSize", queue::size).register(registry);
}
```





#### 4、定制Endpoint

```java
@Component
@Endpoint(id = "myservice")
public class DockerEndpoint {
    @ReadOperation
    public Map getDockerInfo(){
        return Collections.singletonMap("info","docker started...");
    }

    @WriteOperation
    private void restartDocker(){
        System.out.println("docker restarted....");
    }
}
```

场景：开发**ReadinessEndpoint**来管理程序是否就绪，或者**Liveness****Endpoint**来管理程序是否存活；



- 可以结合ConfigurationProperties开启对应的配置功能。

当然，这个也可以直接使用 https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-kubernetes-probes



## 原理解析

### 1、Profile功能

为了方便多环境适配，springboot简化了profile功能。

#### 1、application-profile功能

- 默认配置文件  application.yaml；任何时候都会加载
- 指定环境配置文件  application-{env}.yaml

- 激活指定环境
  - 配置文件激活
  - 命令行激活：java -jar xxx.jar --**spring.profiles.active=prod  --person.name=haha**
    - **修改配置文件的任意值，命令行优先**

- 默认配置与环境配置同时生效
- 同名配置项，profile配置优先





#### 2、@Profile条件装配功能

```java
@Configuration(proxyBeanMethods = false)
@Profile("production")
public class ProductionConfiguration {

    // ...

}
```

- production环境下下面的才生效嗷，比较常用在@Bean上面，在对应的Profile下，相应的配置才会生效（有点像ConditionOnBean的那种感觉，现在是ConditionalOnProfile）。可以标注在方法上或者是类上。

- @Profile可以设置值为一个list，里面可以加上“default”，就是默认情况下后面的配置会生效。

#### 3、profile分组

```properties
spring.profiles.group.production[0]=proddb
spring.profiles.group.production[1]=prodmq

使用：--spring.profiles.active=production  激活
```

- 上面这个会加载production整个组，这个组中实际对应的application-proddb/-prodmq这两个配置文件嗷！！！（本质就是分组嘛！！！）

#### 4、小例子：

![image-20210722192435511](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210722192435.png)



![image-20210722192905058](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210722192905.png)



![image-20210722193108097](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210722193108.png)













### 2、外部化配置

https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config



1. Default properties (specified by setting `SpringApplication.setDefaultProperties`).
2. `@PropertySource` annotations on your `@Configuration` classes. Please note that such property sources are not added to the `Environment` until the application context is being refreshed. This is too late to configure certain properties such as `logging.*` and `spring.main.*` which are read before refresh begins.

1. **Config data (such as** `**application.properties**` **files)**
2. A `RandomValuePropertySource` that has properties only in `random.*`.

1. OS environment variables.
2. Java System properties (`System.getProperties()`).

1. JNDI attributes from `java:comp/env`.
2. `ServletContext` init parameters.

1. `ServletConfig` init parameters.
2. Properties from `SPRING_APPLICATION_JSON` (inline JSON embedded in an environment variable or system property).

1. Command line arguments.
2. `properties` attribute on your tests. Available on `@SpringBootTest` and the [test annotations for testing a particular slice of your application](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-testing-spring-boot-applications-testing-autoconfigured-tests).

1. `@TestPropertySource` annotations on your tests.
2. [Devtools global settings properties](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-devtools-globalsettings) in the `$HOME/.config/spring-boot` directory when devtools is active.



#### 1、外部配置源

常用：**Java属性文件**、**YAML文件**、**环境变量**、**命令行参数**；



#### 2、配置文件查找位置

(1) classpath 根路径

(2) classpath 根路径下config目录

(3) jar包当前目录

(4) jar包当前目录的config目录

(5) /config子目录的直接子目录

- 优先级越往下越高嗷！！！！！



#### 3、配置文件加载顺序：

1. 　当前jar包内部的application.properties和application.yml
2. 　当前jar包内部的application-{profile}.properties 和 application-{profile}.yml
1. 　引用的外部jar包的application.properties和application.yml
2. 　引用的外部jar包的application-{profile}.properties 和 application-{profile}.yml

- 优先级越往下越高嗷！！！！！





#### 4、指定环境优先，外部优先，后面的可以覆盖前面的同名配置项















### 3、自定义starter

[这个自定义的starter太强了！！！](https://www.bilibili.com/video/BV19K4y1L7MT?p=83&spm_id_from=pageDriver)

#### 1、starter启动原理

- starter-pom引入 autoconfigurer 包

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606995919308-b2c7ccaa-e720-4cc5-9801-2e170b3102e1.png)

- autoconfigure包中配置使用 **META-INF/spring.factories** 中 **EnableAutoConfiguration 的值，使得项目启动加载指定的自动配置类**
- **编写自动配置类 xxxAutoConfiguration -> xxxxProperties**

- **@Configuration**
- **@Conditional**

- **@EnableConfigurationProperties**
- **@Bean**

- ......

**引入starter** **--- xxxAutoConfiguration --- 容器中放入组件 ---- 绑定xxxProperties ----** **配置项**

#### 2、自定义starter

**atguigu-hello-spring-boot-starter（启动器）**

**atguigu-hello-spring-boot-starter-autoconfigure（自动配置包）**







### 4、SpringBoot原理

Spring原理【[Spring注解](https://www.bilibili.com/video/BV1gW411W7wy?p=1)】、**SpringMVC**原理、**自动配置原理**、SpringBoot原理

#### 1、SpringBoot启动过程

- 创建 **SpringApplication**

  - 保存一些信息。
  - 判定当前应用的类型。ClassUtils。Servlet

  - **bootstrappers****：初始启动引导器（**List<Bootstrapper>**）：去spring.factories文件中找** org.springframework.boot.**Bootstrapper**
  - 找 **ApplicationContextInitializer**；去**spring.factories找ApplicationContextInitializer**

    - List<ApplicationContextInitializer<?>> **initializers**

  - **找** **ApplicationListener  ；应用监听器。**去**spring.factories****找** **ApplicationListener**

    - List<ApplicationListener<?>> **listeners**




- 运行 **SpringApplication**

  - **StopWatch**：用于监控整个应用的启停的嗷！！！
  - **记录应用的启动时间**

  - **创建引导上下文（Context环境）**createBootstrapContext()

    - 获取到所有之前的 **bootstrappers（这个上面有哦！！！） 挨个执行** intitialize() 来完成对引导启动器上下文环境设置

  - 让当前应用进入**headless**模式。**java.awt.headless**
  - **获取所有** **RunListener**（运行监听器）【为了方便所有Listener进行事件感知】

    - getSpringFactoriesInstances 去**spring.factories**中找**SpringApplicationRunListener**. 
    - 遍历 **SpringApplicationRunListener 调用 starting 方法；**
    - **相当于通知所有感兴趣系统正在启动过程的人，项目正在 starting。**

  - 保存命令行参数；ApplicationArguments
  - 准备环境 prepareEnvironment（）;

    - 返回或者创建基础环境信息对象。**StandardServletEnvironment**
    - **配置环境信息对象。**

      - **读取所有的配置源的配置属性值。**

    - 绑定环境信息
    - 监听器调用 listener.environmentPrepared()；通知所有的监听器当前环境准备完成

  - 创建IOC容器（createApplicationContext（））

    - 根据项目类型（Servlet）创建容器，
    - 当前会创建 **AnnotationConfigServletWebServerApplicationContext**

  - **准备ApplicationContext IOC容器的基本信息**  **prepareContext()**

    - 保存环境信息
    - IOC容器的后置处理流程。

    - 应用初始化器；applyInitializers；

      - 遍历所有的 **ApplicationContextInitializer 。调用** **initialize.。来对ioc容器进行初始化扩展功能**
      - 遍历所有的 listener 调用 **contextPrepared。EventPublishRunListenr；通知所有的监听器****contextPrepared**

    - **所有的监听器 调用** **contextLoaded。通知所有的监听器** **contextLoaded；**

  - **刷新IOC容器。**refreshContext

    - 创建容器中的所有组件（Spring注解）

  - 容器刷新完成后工作？afterRefresh
  - 所有监听器调用 listeners.**started**(context); **通知所有的监听器** **started**

  - 调用所有runners；callRunners()

    - **获取容器中的** **ApplicationRunner** 
    - **获取容器中的**  **CommandLineRunner**

    - **合并所有runner并且按照@Order进行排序**
    - **遍历所有的runner。调用 run** **方法**
  - **如果以上有异常，**

    - **调用Listener 的 failed**

  - **调用所有监听器的 running 方法**  listeners.running(context); **通知所有的监听器** **running** 
  - running如果有问题。继续通知 failed 。调用所有 Listener 的failed；通知所有的监听器failed







```java
public interface Bootstrapper {

	/**
	 * Initialize the given {@link BootstrapRegistry} with any required registrations.
	 * @param registry the registry to initialize
	 */
	void intitialize(BootstrapRegistry registry);

}
```

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1607005958877-bf152e3e-4d2d-42b6-a08c-ceef9870f3b6.png)

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1607004823889-8373cea4-6305-40c1-af3b-921b071a28a8.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1607006112013-6ed5c0a0-3e02-4bf1-bdb7-423e0a0b3f3c.png)

```java
@FunctionalInterface
public interface ApplicationRunner {

	/**
	 * Callback used to run the bean.
	 * @param args incoming application arguments
	 * @throws Exception on error
	 */
	void run(ApplicationArguments args) throws Exception;

}
@FunctionalInterface
public interface CommandLineRunner {

	/**
	 * Callback used to run the bean.
	 * @param args incoming main method arguments
	 * @throws Exception on error
	 */
	void run(String... args) throws Exception;

}
```



#### 2、Application Events and Listeners

https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-application-events-and-listeners

**ApplicationContextInitializer**

**ApplicationListener**

**SpringApplicationRunListener**



![image-20210724161047849](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724161055.png)



#### 3、ApplicationRunner 与 CommandLineRunner

- 放入容器中，就能够得到嗷！！！

- ![image-20210724161802363](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210724161802.png)