---
title: Redis学习
date: 2021-08-16 19:30:45
tags: 后端
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817091327.png
---



# This is the course of Redis

# NoSQL是啥？

- 技术的分类：
  1. 解决功能性的问题：Java , Jsp , Tomcat , HTML , JDBC等
  2. 解决拓展性的问题：Spring , SpringMVC , Hibernate , Mybatis , Struts
  3. 解决性能的问题：NoSQL , Java线程 ， Nginx , MQ , ElasticSearch , Java线程等
- 分布式遇到的问题：

![image-20210817094038766](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817094039.png)

Session的问题，转发到不同的服务器，那Session咋办？？？ ---> Redis

而且Redis也可以做缓存，提高对于经常访问的数据的访问速度。

## 概述：

- NoSQL , Not only sql .
- 问题：
  - 没有统一的SQL标准
  - 不支持ACID
  - 效率远高于SQL



# Redis环境配置：

## Redis安装

- [Redis官网](https://redis.io/)

- 在Redis中间正下方有一个download，下载的就是.tar.gz
- 不考虑在windows中的使用，直接在Linux下安装即可，把安装包拖到Linux中去嗷。
- 环境配置：
  - 需要C语言的编译环境
  - `yum install gcc`
  - gcc安装完成之后，可以使用`gcc --version`来查看版本，要是能查到说明没啥问题。
- 接下来：
  - ![image-20210817095545797](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817095546.png)
  - make完成之后，使用make install命令把它安装
  - 默认会安装到/usr/local/bin这个目录下嗷！！！
  - [可能遇到的一些其他的问题回去看原视频or百度嗷！！！](https://www.bilibili.com/video/BV1Rv41177Af?p=4&spm_id_from=pageDriver)
- 安装完成之后对应的安装目录下：

```shell
[root@VM-12-10-centos bin]# ls
busybox-x86_64   redis-check-aof  redis-cli       redis-server
redis-benchmark  redis-check-rdb  redis-sentinel
```

最重要的是redis-server和redis-cli



## Redis启动：

- 启动方式：

  - 前台启动：

    - 直接在目录下输入redis-server就可以启动啦！！！
    - 问题：当前窗口动不了了，一旦关掉了，Redis

  - 后台启动：

    - 关窗口还可以继续使用嗷！！！

    - 如何操作呢？

      - 要先回到解压出来的redis的那个解压目录。

      - ![image-20210817102932363](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817102932.png)

        这个目录不一定要在etc下哈！！！

      - 改etc中的redis.conf文件的配置

      - daemonize把no改成yes

      ![image-20210817103208928](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817103209.png)

    - 回到redis的安装目录，`cd /usr/local/bin`

    - `redis-server /etc/redis.conf`

    - 启动完成之后，查看redis进程（这样启动之后，就算断了链接关闭窗口，都能够正常使用redis）：

    ```shell
    [root@VM-12-10-centos bin]# ps -ef | grep redis
    root     27049     1  0 10:35 ?        00:00:00 redis-server 127.0.0.1:6379
    root     27167 15067  0 10:36 pts/0    00:00:00 grep --color=auto redis
    ```

- 在安装目录使用`redis-cli`，就能够成功访问到我们的redis-server嗷：

```shell
[root@VM-12-10-centos bin]# redis-cli 
127.0.0.1:6379> ping
PONG
```

## Redis关闭：

1. 在上述cli中输入shutdown

   另外，cli中输入exit是退出cli

2. 直接kill -9 redis-server的进程号也可以嗷！

3. `redis-cli -p 6379 shutdown`



## Redis相关知识：

- Merz就是刚好对应了6379，这个端口号的由来

- 默认16个数据库，初始默认使用0号库

- ```shell
  127.0.0.1:6379> select 2
  OK
  127.0.0.1:6379[2]> 
  ```

  上面这种就是切换数据库嗷，从0-15

- 统一库管理，所有库的密码相同。

- 单线程 + 多路IO复用：

![image-20210817110551511](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817110551.png)

黄牛去火车站单线程取票的时候，1，2，3号可以去干别的事儿。黄牛取完票之后，会通知1，2，3号过来取嗷！！！

# Redis详细学习：

## Key的基本操作：

![image-20210817112637989](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817112638.png)

```shell
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> set k1 tuzi
OK
127.0.0.1:6379> set k2 lele
OK
127.0.0.1:6379> set k3 QAQ
OK
127.0.0.1:6379> keys *
1) "k3"
2) "k1"
3) "k2"
127.0.0.1:6379> exists k1
(integer) 1
127.0.0.1:6379> exists k4
(integer) 0
127.0.0.1:6379> type k2
string
127.0.0.1:6379> del k3
(integer) 1
127.0.0.1:6379> keys *
1) "k1"
2) "k2"
```

![image-20210817113139205](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817113139.png)

![image-20210817113215574](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817113215.png)

## 数据库基本操作：

![image-20210817113817831](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817113818.png)



## 数据类型：

### String

- 最基本的类型，一个key对一个value
- 二进制安全，只要内容能够使用字符串表示，都可以存进去。意味着Redis的string可以包含任何数据，比如jpg图片或者序列化对象。
- 一个Redis中字符串value最多可以是512M
- 常用命令：
  - set key value
    - 多次set key，以最后一次为准
  - get key
  - append key value
    - 追加到原值的末尾
    - 返回追加后总的字符串长度
  - strlen key
  - setnx key value
    - 只有key不存在的时候，才能够设置key的值
    - 返回值为0，说明设置失败。返回值为1，说明设置成功。
  - incr key
  - decr key
    - 以上两个加一减一的操作，只有对于数字才有效嗷！！！
  - incrby / decrby key step
    - 自定义步长
    - 加上或着减去对应的值
- 原子性：
  - INCR key等
  - 不会被线程调度操作打断的操作，因为redis操作时单线程操作，而不是多线程操作
  - 原理是：单线程 + IO多路复用

![image-20210817115858773](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817115859.png)

不是，因为Java是多线程操作，可以允许多个线程并发操作嗷！！！

取值范围是2到200之间嗷！！！

![image-20210817123901533](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817123901.png)

就是硬生生卡位置呗orz

- 常用命令补充：
  - mset key1 value1 key2 value2...
  - mget key1 key2...
  - msetnx key1 value1 key2 value2...
    - 如果设置的key已经存在，设置是不会成功的嗷！！！
    - 注意，这是一个原子性质的操作，如果有一个key存在的话，所有的赋值操作都会失败嗷！！！
  - getrange key BeginPosition EndPostion
    - 字符串从Begin到End位置的字符串提取出来，包括起始位置和结束位置的值嗷
  - setrange key BeginPosition value
    - ![image-20210817131735074](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817131735.png)
    - 从某个位置开始设置值
  - setex key ttl value
    - 设置value的同时设置过期时间
  - getset name value
    - 以新换旧，设置了新值的同时获取旧值。



- 底层数据结构：
  - 简单**动态字符串**SDS，采用预分配冗余空间的方式减少内存的频繁分配嗷！
  - ![image-20210817132447485](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817132447.png)
- 小于1M和大于!M扩容策略不一样嗷！！！最大不超过512M嗷！！！
- 注意上面说的这个扩容方式是对于value的类型和范围嗷，不是key的嗷！！！





### List

- 单键多值，简单的字符串列表，按照插入顺序排序，可以添加一个元素到列表的头部或者尾部。
- 底层上实际是个双向列表，对于两端的操作性能很高，通过索引下标的操作中间结点性能会比较差。

![image-20210817153655557](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817153655.png)

- 常用命令：

  - lpush key v1 v2 v3

    - 从左边push进去

    ```shell
    127.0.0.1:6379> lpush k1 v1 v2 v3
    (integer) 3
    127.0.0.1:6379> lrange k1 0 -1
    1) "v3"
    2) "v2"
    3) "v1"
    ```

  - rpush key v1 v2 v3

    - 从右边push进去

  - lpop key

  - rpop key

    - 一次只吐出一个值哦！！！
    - 和上面类似，从左边或者右边拿一个值出来。
    - 键在值在，键亡值亡。

  - rpoplpush k1 k2

    - k1列表右边pop出一个值，push到k2列表左边嗷！！！
    - 我咋感觉这个List和Python有点像呢？？？

  - lrange key start end

    - 按照索引下标获得元素
    - 0表示第一个
    - -1表示最后一个
    - 0 -1代表所有

  - lindex key index

    - 取得某个位置上的元素
    - 从左到右来看的嗷！

  - llen key

    - 获得列表的长度

  - linsert key before/after value newvalue

    - 在value的前面或后面插入newvalue

  - lrem key n value

    - 从左边删掉n个value

  - lset key index value

    - 设置某个位置的值为新值

- 底层数据结构：

  - quickList
  - 元素少的时候，连续的内存存储，这一块内存中的元素是压缩列表ziplist
  - 将所有的元素一起存储，分配一块连续内存
  - 数据量多的时候，才会构成quickList

  ![image-20210817155550652](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817155551.png)



### Set

- String类型的无须集合，底层是一个value为null的hash表，所有添加，删除，查找的复杂度都是O(1)
- 常用命令：
  - sadd k1 v1 v2 v3
  - smembers key
    - 取出集合所有值
  - sisnumber k1 v1
    - 查找某个值是否在k1集合中
  - scard key
    - 返回集合的元素个数
  - srem key v1 v2 ...
    - 删除集合中的某个元素
  - spop key
    - 随机从集合中pop出一个值
  - srandmember key n
    - 从key集合中随机取出n个值
    - 注意，是取出，不会删除嗷！！！
  - smove source destination value
    - 把一个值从一个集合移动到另外一个集合
  - sinter k1 k2
    - 取两个集合的交集
  - sunion k1 k2
    - 取两个集合的并集
  - sdiff k1 k2
    - 返回两个集合的差集（key1中有的，不包含key2中的元素所构成的集合）



- 数据结构：
  - dist字典，通过Hash表实现的
  - Java中HashSet内部使用的HashMap，只不过所有value都指向同一个对象。Redis的set结构也是一样，内部使用hash结构，所有的value都指向同一个内部值。



### Hash

- 键值对集合
- 类似于Java中的map结构，field和value的映射关系嗷！！！通过field对应value，Map<String , Object>

![image-20210817162855094](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817162855.png)

- 常用命令：
  - hset key field value
    - hset user:1001 name zhangsan
  - hget key field
    - hget user:1001 name
  - hmset key value key value
    - hmset user:1002 name lisi age 30
  - hexists key field
    - 查看对应的field是否存在
  - hkeys key
    - 查看所有的fields
  - hvals key
    - 查看所有的values
  - hincrby key user:1002 age 2
    - 对应的filed加上增量
  - hsetnx key field value
    - 将哈希表中的key中的filed设置为value，当field不存在的时候才能够添加嗷！！！
- 数据结构：
  - 短的话用ziplist，长度话用hashtable





### Zset

- 和普通集合非常相似，没有重复元素
- 但是每一个成员有一个评分，评分用于按照最低到最高来排序。集合的成员唯一，评分可以重复。
- 常用操作：
  - zadd key score1 value1 score2 value2
    - 将一个或多个元素存进去
  - zrange key begin stop [withscores]
    - 返回有序集key，下标在begin和stop之间
    - withscores可选，让分数一起和值返回到结果集
  - zrangebyscore key start stop [withscores]
    - 取score位于start和stop之间的元素
  - zrevrangebyscore key start stop [withscores]
    - 从大到小的scores进行排序
  - zincrby key score value
    - 为key中的某个value增加权重
  - zrem key value
  - zcount key start stop
    - 统计score位于start和stop之间的value
  - zcount key start stop
    - 返回区间内元素的个数
  - zrank key value
    - 返回某个值在集合中的排名（从0开始）
- 数据结构：
  - 等价于Java的Map<String , Double>，可以给每一个元素value一个score，另一方面又类似于TreeSet，内部的元素会按照score进行排序，得到每个元素的名次，可以通过score的范围来获取元素的列表。
  - 两个数据结构：
    - hash，关联元素和权重score，保证value的唯一性，可以通过元素value来找到对应的score值。
    - 跳跃表，给元素value排序，根据score的范围来获取元素列表嗷！！！

跳跃表：

![image-20210817171058509](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817171058.png)

典型的用空间换时间的做法嗷！！！



# Redis配置文件：

![image-20210817171306164](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817171306.png)

- 如果要远程连接的话可以把这个配置给注释掉

![image-20210817171402017](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817171402.png)

- 如果要远程连接的话可以把这个yes改为no，关闭保护模式

- tcp-backlog：
  - 总和 = 未完成三次握手的队列+已经完成三次握手的队列
  - 本质上就是一个连接队列而已嗷！！！



- port：
  - 服务的端口



- timeout：
  - 默认为0，永不超时
  - 可以设置为对应的值，单位是秒，这么多秒没有操作就会超时嗷！！！

- tcp-keepalive
  - 默认是300s
  - 检测你的心跳，没心跳了就断掉连接嗷！！！



![image-20210817171842816](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817171843.png)

![image-20210817171907334](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817171907.png)



- logfile：
  - 默认为空
  - 日志文件的输出位置



- databases：
  - 默认为16，默认用的是0



- 密码设置：

![image-20210817172051354](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817172051.png)

![image-20210817172116889](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817172117.png)





- LIMITS限制：

  - maxclients：

    - 默认为10000个客户端
    - 设置redis可以和多少客户端连接
    - 达到限制就会拒绝连接请求，并且回应max number of clients reached。

  - ![image-20210817172253896](C:\Users\Alexander\AppData\Roaming\Typora\typora-user-images\image-20210817172253896.png)

    ![image-20210817172324292](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817172324.png)

  - ![image-20210817172346962](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817172347.png)



- 还有一些持久化的操作
- 综上，如果要远程访问：
  - bind给它注释掉
  - 关闭保护模式





# 发布和订阅：

- 消息通信模式，pub发送消息，订阅者接受消息
- Redis客户端可以订阅任意数量的频道

![image-20210817191037613](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817191037.png)



1. 客户端订阅channel：

`SUBSCRIBE channel1`

2. 向channel中发布消息：

`PUBLISH channel2 message`

- tips: 启动两个连接，一个订阅，一个发送，可以通过这种方式来操作嗷！！！

# Redis新的数据类型：

## Bitmaps

- 本身不是一种数据类型，实际上它就是字符串嗷！！！但是它可以对于字符串的位进行操作嗷！！！
- Bitmaps单独提供了一套命令，所以在Redis中使用Bitmaps的方法不一样嗷！！！每个单元只能存放0和1嗷！！！

![image-20210817192222123](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817192222.png)

上面这个实际上存储的是ascii码嗷！！！

- 具体操作：

  - setbit key offset value

    - 设置Bitmaps中某个偏移量的值（0或者1）

      ![image-20210817192428034](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817192428.png)

      ![image-20210817192525353](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817192525.png)

      缺点：偏移量大的时候，会执行慢，可能造成redis阻塞嗷！！！

  - getbit key offset

  - bitcount key start end

    - 统计为1的位数的数量

  - bitop and(or/not/xor) \<destkey> [key...]

    - and表示两个里面都取值
    - or表示或
    - xor表示异或
    - 说白了就是对应位置的bit运算嘛！！！
    - 返回值是符合条件的比特位个数。
    - 可以用于统计是否登录啊之类的...

- 优点！！！

![image-20210817193414148](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817193414.png)

![image-20210817193448348](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817193448.png)

用户比较少的时候，很多位置是0，就不是非常合适！

- 本身并不是一种独立的数据类型，和Set相比，节约空间嗷（在用户量非常大的时候才适用嗷）！！！



## HyperLogLog

- 用于做基数统计的算法

![image-20210817193838603](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817193838.png)

- 主要用于做基数的计算操作



- 常用命令：
  - pfadd key element1 element2 ...
    - 添加元素
  - pfcount key
    - 返回所有元素的基数的数量
  - pfmerge destkey sourcekey1 sourcekey2...
    - 合并多个key，为一个destkey
    - 例如k1 , program如果都存在
    - pfmerge k100 k1 program
    - 这样保证了k1和program不动的情况下，融合成一个全新的k100



## Geospatial

- 元素的二维坐标，用于地理位置的记录和操作
- 常用操作：
  - geoadd key longtitde latitude member [longtitude latitude member...]
    - ![image-20210817194829643](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817194829.png)
    - 可以同时加入多个嗷！！！
    - ![image-20210817194923626](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817194924.png)
  - geopos key member
    - 这样就可以取到经纬度嗷！！！
  - geodist key member1 member2 [m|km|ft|mi]
    - ![image-20210817195128895](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817195129.png)
    - 取出两个经纬度之间的直线距离嗷！！！
  - georadius ley longtitude latitude radius m|km|ft|rt
    - 取出某个经纬度点，以它为中心，半径多哦少以内的所有元素嗷！！！
    - ![image-20210817195332376](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210817195332.png)



# Java操作Redis：

## 准备项目：

1. 新建project
2. POM依赖：

```xml
<dependencies>
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>3.6.0</version>
    </dependency>
</dependencies>
```

3. 新建测试类（com.tuzi.jedis.JedisDemo1）:

```java
import redis.clients.jedis.Jedis;

public class JedisDemo1 {

    public static void main(String[] args) {
        //新建Jedis对象
        Jedis jedis = new Jedis("www.liuyihao.work",6379);

        //测试
        String value = jedis.ping();
        System.out.println(value);
    }

}
```

- 如果连接超时的话，很有可能是redis防火墙设置的问题嗷！！！
- 返回值就是一个PONG，说明连接上了嗷！！！



## 后续测试：

- 每种数据类型都有自己的
- 上网直接查Jedis对应数据类型的操作方法就好了嗷！！！



## 实战演练：

![image-20210818103917199](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818104318.png)

1. 随机生成6位的数字码，2分钟内有效

- **Random**

- 验证码放到Redis里面去，设置过期时间

2. 验证码是否相同？

- 从Redis中拿出来，和你的输入进行比对嗷

3. 每个手机号每天只能输入3次

- incr每次发送之后+1
- 大于2的时候，提交就不能发送啦！！！



```java
public class PhoneCode {
    public static void main(String[] args) {
        verifyCode("123456");
        getRedisCode("123456","463733");
    }

    //1. 生成六位数字验证码
    public static String getCode(){
        Random random = new Random();
        String code = "";
        for (int i=0;i<6;i++){
            int rand = random.nextInt(10);
            code += rand;
        }
        return code;
    }

    //2. 让每个手机每天只能发送三次，设置过期时间
    public static void verifyCode(String phone){
        //连接redis
        Jedis jedis = new Jedis("liuyihao.work",6379);
        jedis.auth("********");

        //拼接key
        //手机发送次数key
        String countKey = "VerifyCode" + phone + ":count";

        //验证码key
        String codeKey = "VerifyCode" + phone + ":code";

        //每个手机每天只能取三次
        String count = jedis.get(countKey);
        if (count == null) {
            //没有发送次数，第一次发送
            //设置发送次数位1
            jedis.setex(countKey,24*60*60,"1");
        }else if(Integer.parseInt(count) <= 2){
            //发送次数+1
            jedis.incr(countKey);
        }else if(Integer.parseInt(count)>2){
            System.out.println("今天发送次数已经超过三次");
            jedis.close();
            return;
        }

        //发送验证码到redis里面
        String vcode = getCode();
        jedis.setex(codeKey,120,vcode);
        jedis.close();
    }

    //3.验证码校验
    public static void getRedisCode(String phone , String code){
        //从redis获取验证码
        //连接redis
        Jedis jedis = new Jedis("liuyihao.work",6379);
        jedis.auth("********");

        String codeKey = "VerifyCode" + phone + ":code";
        String redisCode = jedis.get(codeKey);
        //判断
        if (redisCode.equals(code)){
            System.out.println("成功");
        }else {
            System.out.println("失败");
        }

        jedis.close();
    }
}
```



## Boot整合redis：

1. 新建一个Module
2. POM.xml：

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
            <version>2.6.0</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.12.1</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency><dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
            <version>2.6.0</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.12.1</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.12.3</version>
        </dependency>
```

3. Application.yml：

```yaml
spring:
  redis:
    host: liuyihao.work
    port: 6379
    password: ********
    timeout: 1800000
    lettuce:
      pool:
        max-active: 20
        max-wait: -1
        max-idle: 5
        min-idle: 0
```

4. 主启动
5. 业务类：

```java
@EnableCaching
@Configuration
public class RedisConfig extends CachingConfigurerSupport{

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate();
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.activateDefaultTyping(LaissezFaireSubTypeValidator.instance, ObjectMapper.DefaultTyping.NON_FINAL, JsonTypeInfo.As.WRAPPER_ARRAY);
        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);
        template.setConnectionFactory(factory);
        template.setKeySerializer(redisSerializer);
        template.setValueSerializer(jackson2JsonRedisSerializer);
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        return template;
    }

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory){
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        //解决查询异常转换问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        //配置序列化问题
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(600))
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();
        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }
}
```

- 引入web，要做测试嗷！！！
- Controller：

```java
@RestController
@RequestMapping("/redisTest")
public class RedisTestController {

    @Autowired
    private RedisTemplate redisTemplate;


    @GetMapping
    public String testRedis(){
        redisTemplate.opsForValue().set("name","Tuzi");
        //从redis中获取值
        String name = (String)redisTemplate.opsForValue().get("name");
        return name;
    }
}
```

6. 测试：

![image-20210818180016559](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818180016.png)







# 事务操作：

- 单独的隔离操作，所有命令都会序列化，按照顺序执行。事务执行过程中不会被打断嗷。
- **串联多个命令**（序列化）防止别的命令插队！！！

## 命令：

- Multi：类似于mysql中的start transaction，开启事务。
- Exec：类似于mysql中的提交事务
- Discard：类似于mysql中的回滚事务

![image-20210818191123464](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818191123.png)

![image-20210818191319320](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818191319.png)

- 演示：

multi + exec

![image-20210818191506662](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818191506.png)

multi + discard

![image-20210818191550730](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818191550.png)



## 事务出错处理：

- 组队出错：
  - ![image-20210818191706634](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818191706.png)
  - ![image-20210818191752429](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818191752.png)
  - 组队有错，整个命令都不会执行嗷！！！
- 执行出错：
  - ![image-20210818191842002](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818191842.png)
  - ![image-20210818191927555](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818191927.png)
  - 组队成功，运行的时候哪个出错就错了，正确的正常执行。



## 事务和锁机制：

- 悲观锁：加锁操作

  - 行锁，读锁，写锁，表锁等都是嗷！

- 乐观锁：

  - 数据加上版本号
  - 不锁死嗷！！！
  - 所有人都能拿到数据，但是更新就不一定了嗷
  - 写的少，读的多
  - 抢票就是用乐观锁的嗷

- 命令：

  - 使用watch key这种方式来实现嗷！！！
  - watch是加上了乐观锁嗷！！！
  - 实验使用两个终端，watch同一个key，来进行实验嗷！
  - 实验结果：

  ![image-20210818195716428](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818195716.png)

  ![image-20210818195805139](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818195805.png)

  前面的先操作了，后面的就执行不了了嗷！！！

- 事务三特性：

  - 单独的隔离操作（序列化顺序执行）
  - 没有隔离级别的概念
  - 不保证原子性（上面的执行失败了，其他成功的是不会回滚的嗷！！！）



## 秒杀案例：



![image-20210818200121552](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210818200121.png)

- 并发+多请求操作：
  - ab测试工具（模拟高并发嗷！！！）
  - [并发实例学习！！！](https://www.bilibili.com/video/BV1Rv41177Af?p=25&spm_id_from=pageDriver)
- 连接问题的解决：连接池
- 超卖问题的解决：乐观锁+multi组队的事务操作
- 乐观锁造成的库存遗留问题：
  - 改用悲观锁（Redis内部默认不支持悲观锁）
  - LUA脚本的问题

# Redis持久化

## RDB：

- 指定的时间间隔中将内存中的数据集快照写入磁盘
- 默认生成dump.rdb文件，在当前文件夹下生成嗷！！！
- 配置文件中可以配置嗷，从理论上来说，key变得越快，更新dump.rdb的时间间隔就越短嗷！！！
- 配置文件中也有对应的配置嗷

![image-20210819083319828](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819083320.png)

注意哈，要把注释打开，然后需要做适当的时间调整哈！！！

- 配置文件中还有save的配置：

![image-20210819083611916](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819083612.png)

第一个效率很低，提供不了服务（持久化的时候）。

![image-20210819083659018](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819083659.png)

- stop-writes-onbgsave-error：

![image-20210819083823678](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819083823.png)

- rdbcompression：

![image-20210819084044780](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819084045.png)

- rdbchechsum：

![image-20210819084120378](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819084120.png)

- 备份如何执行：
  - 单独创建一个fork子进程来进行持久化，将数据写入一个临时文件，临时文件写完了之后，再用临时文件去替换持久化文件就行了嗷！！！
  - ![image-20210819084325262](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819084325.png)
  - 缺点：最后一次持久化后的数据可能丢失嗷！（说白了就是还没满足条件，持久化还未激活，导致后面来的数据没有被持久化，自然就丢失了orz）
  - 如果数据非常敏感不建议使用RDB，使用AOF就可以了嗷！
- 原理：

![image-20210819084903138](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819084903.png)

然后子进程就操作自己的那一份内容，操作完成再替换掉父进程中的内容。

- 恢复：

![image-20210819085028908](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819085029.png)

- 优势：
  - 节约磁盘空间
  - 更快一些
  - 适合大规模的数据恢复
  - 对于数据完整性和一致性要求不高更加适合使用



## AOF：

- Append Only File
- 以日志的形式来记录每个写操作（不保存读记录）
- 记录所有指令，只允许追加文件不可以改写文件。
- AOF默认不开启的嗷！！！配置文件中有一个appendonly

![image-20210819100050855](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819100051.png)

改成yes才算开启了AOF

![image-20210819100141767](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819100142.png)

生成的路径是一样的哦，和RDB是一样的，都是在当前目录下生成嗷！！！

- 如果AOF和RDB都开启了的话，系统默认从AOF中去读取恢复数据嗷！！！（说白了就是RDB就像不存在了，使用AOF的机制来备份和恢复嗷！！！）



- 异常恢复：

![image-20210819100612713](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819100612.png)

如果文件出现异常的话，会报以下的错误啊：

![image-20210819100950126](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819100950.png)

这个时候就需要进行aof文件的恢复嗷！！！



- 配置：

  - 同步频率设置：

    - appendfsync always/everysec/no
    - ![image-20210819101142247](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819101142.png)

  - Rewrite压缩：

    - ![image-20210819101334886](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819101335.png)

    - 这个也是通过fork子进程来实现的嗷！！！

    - ![image-20210819101510700](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819101511.png)

      ![image-20210819101536615](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819101537.png)



- 持久化流程：
  - 请求写命令会被Append到AOF缓冲区内。
  - 根据AOF持久化策略，将操作异步同步到磁盘的AOF文件中。
  - 如果触发重写策略或者手动重写的时候，会对AOF文件rewrite重写，压缩AOF文件容量。
  - 重启的时候，会加载AOF中的写操作达到数据恢复的目的。
- 优点：
  - 备份机制文件，丢失数据概率低
  - 可读的日志文本，通过操作AOF文件，处理错误的备份文件。
- 劣势：
  - 记录操作，占用更多的空间
  - 速度慢
  - 每次同步，性能压力
  - 恢复记录文件可能会造成问题



## 总结：

- 官方推荐两个都启用
- 数据不敏感，RDB
- 不建议单独使用AOF，可能会有bug
- 只是做纯内存缓存，可以都不用







# 主从复制：

![image-20210819103645608](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819103646.png)

- 好处：
  - 读写分离
  - 容灾的快速恢复
- 一主多从！！！那主机挂了咋办？？？主机集群嗷！！！有备用的嗷！！！



## 一主两从示例：

![image-20210819105649499](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819105656.png)

- 上面这里就是，每一台服务器都有自己的配置内容，配置到自己的配置文件中，然后引入公共的配置部分嗷！！！
- 然后启动三个Redis服务：

![image-20210819110739173](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819110739.png)

没有主从的效果，因为我们没有进行过任何的主从配置嗷！！！

- 如何配置主从关系呢？

注意：配置从服务器，默认开启就是主服务器嗷！！！主服务器不用进行额外配置的嗷！！！

![image-20210819111243976](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819111244.png)

以上命令是在命令行中执行的嗷！！！

- info replication：

  - 也是在命令行中执行的嗷！！！
  - 可以查看本机的状态：

  ![image-20210819111540893](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819111541.png)

  ![image-20210819111630381](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819111630.png)

- 测试：

  - 主机写入的内容，从机可以读！！！
  - 从机不可以进行写操作！！！



## 几个大问题：

### 一主两从：

- 从机挂了：
  - 重启某个从服务器之后，它默认又是主服务器了orz
  - 还需要重新设置为从服务器，然后会重新从头复制一遍主服务器的内容，保证数据的一致性嗷！！！
- 主机挂了：
  - 知道大哥挂了，但是大哥就是大哥，我还认你这个大哥嗷！！！
  - 大哥复活了，小弟们都还在嗷！！！（从服务器仍然是从服务器嗷！！！）



### 薪火相传：

![image-20210819115218796](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819115219.png)

层级模式，主服务器同步从服务器，服务器再帮忙给更多的从服务器扩散。

问题：如果第二层的服务器裂开了，第三层就失联了orz

- 从服务器下面还能再挂从服务器嗷！！！

![image-20210819120142267](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819120142.png)

### 反客为主：

- 小弟当大哥嗷！！！
- 命令：slaveof no one
- 上面这个命令在cli里面输入嗷！！！就是相当于从slave模式切换回master模式嗷！！！

![image-20210819125929530](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819125937.png)

缺点：手动完成，不能够自动完成嗷！！！

- 自动完成：
  - 哨兵模式---凡客为主自动版



### 复制原理：

![image-20210819112651758](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819112651.png)

- 前面的框中是从服务器主动做的，连接主服务器，要求同步的消息。
- 后面主服务器再收到写的消息的时候，就会主动把数据同步到从服务器嗷！！！

![image-20210819112827263](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819112827.png)



# 哨兵模式：

- 从机盯着主机，主机挂了，就立马通知从机变成主机。一主二仆模式。

1. ![image-20210819154123733](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819154132.png)

这个名字不能错哈！！！

2. ![image-20210819154508808](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819154509.png)

3. ![image-20210819154830327](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819154830.png)

上述就成功完成了哨兵对于主机的监控嗷！！！

![image-20210819155138820](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819155139.png)

6380就是新的主机，6379变味了从机，就算它好了，它也是从机嗷！！！

- 小缺点：
  - 复制延迟
  - 从主机到从机的过程中，会有延迟嗷！！！

![image-20210819155450521](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819155450.png)

- 选举规则：
  - 值越小，优先级越高
  - 在redis.conf中配置嗷！！！
  - 偏移量指的是和原主机数据的同步率，同步率越高，偏移量越大。
  - ![image-20210819155707977](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819155708.png)
- 配置文件中的位置：

![image-20210819155733606](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819155733.png)

- 主从复制的Java实现：

![image-20210819160850945](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819160851.png)

![image-20210819160918720](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819160918.png)

# Redis集群：

- 集群解决的问题：
  - Redis容量不够？
  - 并发写操作，如何分摊？





![image-20210819161153243](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819161153.png)

- 代理主机：

![image-20210819161336607](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819161336.png)

紫色的都是集群后备机，预防出现的问题嗷！！！

- 无中心化集群：

![image-20210819161428919](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819161429.png)

任何一台Redis都可以作为集群的入口，相互联通，可以转交服务请求嗷！！！

- 特点：
  - 实现了Redis集群的水平扩容，数据平均存储，每个节点存储总数据的1/N
  - Redis通过分区提供一定程度的可用性，一部分出问题，其他部分还可以提供服务嗷！！！



## 操作实例：



### 集群搭建：

- 模仿上面的用户，订单，商品模块来搭建集群：

![image-20210819161847703](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819161847.png)

![image-20210819162211072](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819162211.png)

- 替换小技巧：

![image-20210819162429964](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819162430.png)

- 将启动的六个结点编程一个集群嗷！！！

![image-20210819162608821](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819162609.png)

replicas后面加一个1，表示以最简单的方式创建集群嗷！！！

![image-20210819162824723](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819162824.png)

- 创建集群完成之后，有如下效果：

![image-20210819163125289](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819163125.png)

- 连接集群：

![image-20210819163346320](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819163346.png)

-c 表示以集群的方式连接，连接这六台机器里面的任何一台都没问题嗷！！！都是连接到了这个集群中嗷！！！





### 集群操作：

![image-20210819163631485](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819163631.png)

最后一句话是因为，尽量分开啊，如果主和从在同一台机器上，一起挂了，不久了吗orz

- 插槽：

![image-20210819164348513](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819164349.png)

插槽就是帮助我们，把压力分担到各台服务器上去嗷！！！

- 插槽实例：

![image-20210819164450726](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819164451.png)

set先会计算插槽，然后根据插槽所在的主机，切换集群中的主机，然后再执行set操作嗷！！！

- mset：

![image-20210819164633913](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819164634.png)

失败了，那我如何实现多个值同时赋予呢？

![image-20210819164723622](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819164723.png)

设置一个组名，这个时候，计算插槽是用组名去计算的，就能够正常完成多个值的添加嗷！！！

- 查询集群中的值：

![image-20210819164826701](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819164826.png)

当你处于某一台服务器，你只能看自己这台服务器插槽的值，相应的插槽要到对应的服务器上去看嗷！！！

- 故障恢复：

![image-20210819165302904](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819165303.png)

和上面是一样的，之前的从机变成了现在的主机，挂掉的主机变成了从机嗷！！！

如果主从都挂掉了可以吗？？？这个要看配置嗷！！！

![image-20210819165447225](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819165447.png)

- Java操作Redis：

![image-20210819165605117](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819165605.png)



## Java操作集群：

- 入口很好写，任何一台机器都可以嗷！！！

![image-20210819172304793](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819172305.png)

- 上面和上上面都可以的哦，可以写Set嗷！！！但是没必要，集群一个就可以嗷！！！
- 好处：
  - 扩容
  - 分摊压力
  - 无中心配置相对简单
- 不足：
  - 多键操作不被支持
  - 多键的Redis事务不被支持，lua脚本不被支持
  - Redis集群方案出现较晚



# Redis应用问题（面试高危）：

## 缓存穿透：

- 应用服务器压力变大了

![image-20210819174252937](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819174253.png)

缓存帮SQL分担压力，但是当大量流量涌入的时候，缓存无能为力了，差不到了，流量全去找数据库了orz。把服务器搞崩了orz。

- redis平稳运行，但是缓存没有起到作用，继续去访问

- 出现的问题：
  - redis查询不到数据库
  - 出现很多非正常url服务（DDos攻击）
- 解决：
  1. 对空值做缓存，让过期时间变短
  2. 设置可访问的名单（白名单）：BitMaps
  3. 布隆过滤器：效率高的BitMaps
  4. 进行实时监控（例如黑名单）



## 缓存击穿：

- 现象：
  - 数据库访问压力突然增加
  - redis里面并没有出现大量key过期
  - redis正常运行
- 原因：
  - 某个被大量访问的key（**某个热门key**）过期了
  - 但是没有被黑客攻击哦！！！
- 流程图：

![image-20210819180056436](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819180056.png)



- 解决方案：
  - 预先设置热门数据，加大时长
  - 实时调整：根据热门数据调整key的时长
  - 加锁：效率低，但是能够解决嗷！！！



## 雪崩问题：

- 数据库压力变大，服务器崩溃！！！
- 问题：
  1. 极少的时间段内，查询**大量key**的集中过期情况
- 解决方案：
  1. 构建多级缓存架构：nginx + redis + 其他缓存等
  2. 使用锁或者队列
  3. 设置过期标志，更新缓存
  4. 将缓存失效时间分散开来

![image-20210819192339879](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819192340.png)



# 分布式锁：

![image-20210819192454434](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819192454.png)

![image-20210819192527835](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819192528.png)

- 例如setnx就相当于加锁，del就相当于释放锁
- 你要拿了锁不释放呢？设置过期时间！！！

![image-20210819193049684](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819193049.png)

- 解决分布式锁问题：

![image-20210819193108433](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819193108.png)

使用：set key value nx ex TTL来实现，nx表示上锁嗷！！！



## Java测试：

![image-20210819193737491](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819193737.png)

- 没有问题，就是怕锁没办法释放就裂开来！！！比如运行中间出现了问题，锁加上了但是没释放，再次执行就执行不了了嗷！！！
- else后面就是等待，上锁失败就要sleep一会儿，再次进行获取嗷！！！另外，上锁和解锁本身都是原子操作嗷！！！
- 那如何设置过期时间呢？

![image-20210819194022400](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819194022.png)

- 还存在什么问题？？？

  - 锁乱啦！！！

  ![image-20210819194242039](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819194242.png)

  - 如何解决？

    1. 使用UUID防止误删。使用UUID来进行判断。

    释放锁的时候，先判断当前要释放锁的UUID和锁的UUID是否一样嗷！！！

  - 上面仍然有问题嗷！！！删除缺乏原子性！！！

    - 即使有上面的判断，依然可能造成A删除了B的锁嗷！！！
    - ![image-20210819201656561](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819201656.png)

  - 如何解决？

    - LUA脚本嗷！！！具有原子性，所有的指令一次性完成嗷！！！
    - 所以比较UUID和删除在脚本中一气呵成，别人是不能够干预的，A就没有释放B的锁的机会了嗷！！！释放锁成了一个整体嗷！！！

- 分布式锁至少要满足以下四个条件：
  - 互斥性，任何时刻，只有一个客户端能持有锁
  - 不会发生死锁，即使有一个客户端在持有锁的期间没有主动解锁，也能保证其他客户端能够加锁嗷！！！
  - 客户端不能够把别人加的锁给解了，解铃还须系铃人。
  - 加锁和解锁的整个过程都要是原子操作嗷！！！



# Redis6.0开始的新功能

## ACL:

- acl list
- acl cat
- acl cat string/......
- acl setuser username

![image-20210819203311312](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819203311.png)

\> 后面的内容就是密码哈！！！

- acl whoami
- auth username password

## IO多线程：

- 本质执行命令还是单线程，单线程+多路IO复用。这个多路IO是针对于网络命令解析啊之类的开启的嗷！！！
- ![image-20210819203616177](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819203616.png)



## 工具支持Cluster：

![image-20210819203720387](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819203720.png)



## 新功能关注：

![image-20210819203751249](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819203751.png)





