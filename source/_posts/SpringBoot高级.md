---
title: SpringBoot高级
date: 2021-08-30 19:20:23
tags: 后端
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830192215.png
---




# 雷神SpringBoot高级部分



<!--more-->



## 缓存

### JSR-107：

- CachingProvider：创建，配置，获取，管理和控制多个CacheManager
- CacheManager：创建，配置，获取，管理和控制多个唯一命名的Cache
- Cache：类似于Key-Value结构，一个Cache只被一个CacheManger所有
- Entry：存储在Cache中的Key-Value对
- Expiry：每一个存储在Cache中的条目有一个定义的有效期。超过了这个时间，条目变为过期的状态。过期了的话，条目将不可访问嗷！！！

![image-20210830193351223](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830193351.png)

- JSR107的实现嗷！！！



### Spring的缓存抽象：

![image-20210830193806780](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830193806.png)



### 工程测试：

- 基本环境搭建：

![image-20210830194545065](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830194545.png)

- SQL的编写（DAO层)：

![image-20210830194645150](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830194645.png)

- Service层的编写：

![image-20210830194922592](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830194922.png)

- Controller的编写：

![image-20210830195014918](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830195015.png)

- 驼峰命名法的开启：

![image-20210830195109244](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830195109.png)

- 缓存使用：

### @Cacheable注解的使用：

1. 开启基于注解的缓存
2. 标注缓存注解即可

![image-20210830195202464](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830195202.png)

注意要EnableCaching开启缓存功能嗷！！！

将方法的运行结果缓存，再要获得相同的数据，直接从缓存中获取，就不用调用方法了嗷！！！

![image-20210830195607264](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830195607.png)

SpEL表达式：

![image-20210830200320141](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830200320.png)

下面是缓存中用到的参数和参数的一些属性嗷！！！

![image-20210830200222978](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830200223.png)



### 原理：

1. 自动配置类：`CacheAutoConfiguration`

2. 加载了一堆缓存的配置类

3. 默认只有SimpleCacheConfiguration生效

4. 给容器中注册了一个CacheManager

5. 可以获取和创建ConcurrentMapCache类型的缓存组件，它的作用将数据保存在ConcurrentMap中

   运行流程：

   1. 方法运行之前，先去查询Cache（缓存组件），按照cacheNames获取缓存。第一次获取缓存如果没有这个Cache组件，会帮助我们自动创建出来。

   2. 去Cache中查找缓存的内容，使用一个key，默认是方法的参数嗷！！！Key是按照某种策略生成的嗷！！！默认是使用simpleKeyGenerator生成的嗷！！！

      SimpleKeyGenerator的生成策略：

      - 没有参数就自己默认new一个对象
      - 如果有一个参数，key=参数的值
      - 如果有多个参数，多个参数的对象嗷！！！

      ![image-20210830202238521](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830202238.png)

   3. 没有查到缓存，就调用目标方法

   4. 将目标方法返回的结果，放进缓存里面

- 一句话：@Cacheable标注的方法执行之前先来检查缓存中有没有这个数据，默认按照值作为key来查询缓存。如果没有，就运行方法，并将结果放入缓存。以后再来调用就可以直接使用缓存中的数据嗷！
- 核心：
  1. 使用CacheManager按照名字获得Cache组件。
  2. key使用keyGenerator生成的，默认是SimpleKeyGenerator。



### 其他属性的使用：

- cacheNames/value：指定缓存组件的名字，将方法的返回结果放在哪个缓存中，是数组的方式，可以指定多个缓存嗷！！！

使用SePL表达式指定Key用的是什么：

![image-20210830203249781](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830203249.png)

- 自定义keyGenerator：

![image-20210830203459321](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830203459.png)

Key和KeyGenerator二选一，所以这里，我们就可以把key的指定去掉。然后KeyGenerator就使用我们自定义的KeyGenerator就行嗷！！！

- Condition：

符合条件的情况下才缓存嗷！！！

![image-20210830203825563](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830203825.png)

![image-20210830203908927](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830203908.png)

- Unless：

符合条件的话就不缓存嗷！！！排除掉某些情况下的缓存嗷！！！

- Sync：

异步情况下，Unless属性就不支持了嗷！！！



### @CachePut：

![image-20210830204905046](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830204905.png)

- 标注在更新方法上面嗷，更新了数据之后，对于缓存中的数据进行缓存嗷！！！
- 运行时机：

1. 先调用目标方法
2. 将目标方法的结果缓存起来

![image-20210830205632301](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830205632.png)

如何解决员工没有在缓存中更新的问题呢？

![image-20210830205755048](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830205755.png)

- 说白了上面这个问题的出现就是因为，缓存中的key，是由keyGenerator生成的，默认的key是参数。而update的参数和select的参数不一样，导致缓存的key不是同一个值，因此可以通过指定key的值来解决嗷！！！
- 注意，@Cacheable的key是不能够使用result的。因为是先读取cache，再去操作数据库的，这个时候result这个变量还不存在呢嗷！！！

### @CacheEvict：

- 缓存清除
- 参数：
  - key：指定要清除的数据
  - value：缓存的名字
  - allEntries：是否删除所有的key，默认为false。可以指定清除这个缓存中所有的数据嗷！！！
  - beforeInvocation默认为false，缓存的清除是否在方法之前执行，默认是在方法之后执行的嗷！！！（如果方法出错了，之后清空缓存就会失败。而之前清空缓存就会成功嗷！！！）



### @Caching：

- 是一个组合注解，可以同时指定多个缓存规则嗷！！！
- ![image-20210831143306709](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210831143315.png)
- ![image-20210831143404248](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210831143404.png)

注意我们这里的CachePut注解嗷，CachePut注解在的话，底下这个方法是一定会执行的嗷！



### @CacheConfig：

- 这个注解是标志在类上的，用于指定公共的缓存规则嗷！！！

![image-20210831143839909](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210831143840.png)







## Redis缓存：

![image-20210831144038303](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210831144038.png)

### 千万记得设置密码！！！

- 这里用Docker配置，也要记得配置密码嗷！！！

### 操作Redis：

![image-20210831152048041](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210831152048.png)

- 如果测试保存对象的话，使用就是redisTemplate了嗷。如果保存的是字符串的话才使用stringRedisTemplate嗷！

![image-20210831152306370](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210831152306.png)

### 改变默认序列化规则：

























