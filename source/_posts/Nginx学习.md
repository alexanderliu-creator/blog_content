---
title: Nginx学习
date: 2021-08-12 21:02:20
tags: 后端
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812210435.png
---



# Cloud中Nginx知识补充

- Nginx基本概念：

> 1. 是啥，干啥
> 2. 反向代理
> 3. 负载均衡
> 4. 动静分离
>

- Nginx学习大纲：
> 1. Linux安装
> 2. Nginx常用命令
> 3. 配置文件
>

- Nginx配置实例：

> 1. 反向代理
> 2. Nginx负载均衡
> 3. 动静分离
> 4. 配置高可用集群

- Nginx原理



# Nginx简介：

- Nginx为高性能反向代理服务器，为了性能而开发了，十分注重效率，非常好用嗷！！！
- 可以热更新嗷！！！很快乐！！！高可用嗷！！！



## 正向代理：

- 客户端用的代理服务器来访问Internet的过程就叫做正向代理哦！！！

![image-20210812215138818](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812215139.png)

- 客户端自己配置代理服务器，通过代理服务器进行资源访问。





## 反向代理：

- 客户端不需要经过任何的配置就能够访问，我们只需要将请求发送到反向代理服务器，反向代理服务器帮助我们获取数据之后返回给客户端。

![image-20210812215605850](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210812215606.png)

- 对于用户来说，它甚至不知道反向代理服务器的存在，它把右边当作整体，对外暴露的是9001端口，由反向代理服务器再去拿到资源返回给用户。

- 整体作为一整个服务器，但是实际上暴露的是代理服务器的地址，而隐藏了正式服务器的IP地址。
- 说白了正向代理是客户端的代理，反向代理是服务端的代理呗！！！





## 负载均衡：

- 增加服务器的数量，将请求分发到各个服务器上，将负载分发到不同的服务器上嗷！！！

![image-20210813083546390](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210813083546.png)









## 动静分离：

- 加快网站解析速度，将动态页面和静态资源分开来，加快解析速度。

![image-20210813083927268](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210813083927.png)





# Nginx在Linux中的安装：

1. XShell连接Nginx

2. 安装方式选择：

   1. Windows下载对应的tar.gz包就好，Xshell中cd到对应的文件夹下，将下载的包拖进去嗷！！！
   2. 使用docker进行下载安装嗷！！！
   3. 使用Linux中默认的命令进行下载配置。

   第一种和第三种其实还需要好多其他的依赖的嗷！！！所以需要我们进一步进行配置等。



- 初探配置（这里采用Docker下的Nginx）：

  - ```shell
    [root@VM-12-10-centos usr]# docker exec -it nginx01 "bin/bash"
    root@bf9a21cb4ccd:/# ls
    bin   docker-entrypoint.d   home   media  proc	sbin  tmp
    boot  docker-entrypoint.sh  lib    mnt	  root	srv   usr
    dev   etc		    lib64  opt	  run	sys   var
    ```

  - 然后进入了交互页面之后，在etc文件夹下的nginx目录，就有对应的内容嗷！！！

  - ```shell
    root@bf9a21cb4ccd:/etc/nginx/conf.d# ls
    default.conf
    ```

  - 在如上目录中，即有default.conf，这个是Docker的默认配置文件嗷！！！



# Nginx操作的常用命令：

- 前提条件：进入到Nginx目录中才能够输入nginx的命令。正常情况下应该是nginx文件夹中的sbin目录中，但是docker是阉割版的，没有这个文件夹？？？
- 这里我们进入Nginx中都能够使用nginx的命令嗷！！！

## nginx -v

- 可以查看版本号
- 正常的nginx是 ./nginx -v

## nginx -s stop

- 关闭Nginx
- 在docker中就是直接停止这个Nginx容器的运行嗷！
- 正常的nginx是 ./nginx -s stop

## ./nginx(docker  run / docker restart)：

- 启动nginx服务（正常的nginx）

- 由于在正常的nginx中的sbin文件夹下是有nginx.exe这个东西的，上面这些命令其实都要在sbin文件夹下使用/nginx + 命令才可以使用，只是在docker中直接使用nginx就可以使用罢了。
- docker中没有严格意义上的nginx内部重启nginx服务，要启动nginx服务，只能在docker层面上，通过开启对应的image的方式来实现嗷！

## nginx -s reload：

- 不暂停nginx服务，重新加载配置文件，使新的nginx配置生效。
- 正常的nginx是./nginx -s reload。



# Nginx配置文件：

- Docker中所在位置：`/etc/nginx`
- Linux中所在位置：`/usr/local/nginx/conf`



## 配置文件中的组成部分：

1. 全局块：

![image-20210815191122340](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815191129.png)

![image-20210815202512279](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815202512.png)

2. events块

```shell
events {
    worker_connections 1024;
}
```

像这里就是表示Nginx支持的最大连接数是多少，这部分配置对于Nginx的影响较大。

3. http块

配置最频繁的部分，这里有细分为：

- Http全局块：

![image-20210815191629828](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815191629.png)



- Server块：

与虚拟主机有密切关系，每个Http可以有多个server块，每个server可以当作一台虚拟主机。每个server块分为全局server块，同时包含多个location块。

1. 全局server块：

最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或者ip配置。

2. location块：

一个server又可以有多个location块。基于收到的字符串进行匹配，对于特定的请求进行处理。很多第三方模块配置（数据缓存啊，应答控制之类的），也在这里嗷！

# Nginx配置实例：

## 反向代理：

1. 实现效果：

- 打开浏览器，输入www.123.com，跳转到linux系统中的Tomcat主页面中

2. 准备工作：

- 在Linux系统安装Tomcat，默认端口8080。我们这里启动一个tomcat镜像，使用本机的8080系统。
- 对外开放访问的端口

```shell
firewall-cmd --add-port=8080/tcp --permanent
firewall-cmd -reload
```

这里不用上面这种哈，直接用腾讯云的控制台开放对应的8080端口就好了嗷！！！

3. 访问过程分析：

![image-20210815201129368](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815201129.png)

网络访问原理：

首先浏览器先会去到Host文件中查找我们是否做过相应的配置，如果有相应的解析配置的话就用我们自己的解析配置。如果没有相应的配置的话，才会下一步去DNS服务器解析我们输入的域名嗷！！！

这里实验为了简单我就不配host中的域名和ip对应关系的配置嗷！！！本质上就是访问nginx嘛，多了一层映射而已，没啥区别。

Host文件的默认位置：C:\\Windows\System32\etc\drivers\etc\HOSTS

前面的内容是ip地址，后面那个是域名。

4. 实际操作



### Docker容器之间反向代理

![image-20210815202629435](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815202629.png)

本质上只需要修改nginx的映射规则就行了，这里就是修改我们的这个server就可以啦！

- Docker中的反向代理：
  - 首先docker中的反向代理涉及到了网关的概念。
  - 例如我们使用本机3344映射Nginx，本机8080映射Tomcat，下面是Nginx转发请求到Tomcat的过程。

![image-20210815212916324](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815212916.png)

- 首先找到Tomcat的IP地址，Docker之前相互沟通本质上是通过网关进行的嗷！！！

![image-20210815213048169](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815213048.png)

- 我们查看一下Nginx的Ip地址，发现它和Tomcat使用的本质上是同一个网关嗷，它可以通过172.17.0.2感知到Tomcat的Docker进程的存在。

![image-20210815213316906](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815213317.png)

- 然后我们在Nginx容器内部相应的转发代理的位置，添加对应的地址（注意，这里是容器与容器的沟通，没有到Linux服务器层面，所以这里的端口号就是容器本身的端口号，ip地址也是Tomcat容器的IP地址）。

[Nginx反向代理实例](https://www.cnblogs.com/dotnet261010/p/12596185.html)



### Docker为宿主机提供反向代理

- 同理，上面的所谓的gateway是由Linux宿主机提供的，要想代理宿主机的某个端口，就可以使用172.17.0.1来操作。
- ![image-20210815214426914](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815214427.png)
- 可以看到，我们这里反向代理的宿主机的80端口，而我早就在宿主机的80端口部署了我的博客，成功访问。

![image-20210815214523441](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815214524.png)



### Docker为外网提供反向代理

- ![image-20210815215012078](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815215012.png)
- 直接代理到外网网址，完全不慌嗷！！！我来试试我自己的！！！

![image-20210815215148595](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815215148.png)

也没有任何问题，成功访问到了我的博客首页，非常棒！！！

### Docker使用ip地址提供反向代理

![image-20210815215440789](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210815215440.png)

- 这里也没有任何问题嗷！！！注意一下一个坑，这里的话，一定要加上前面的Http！！！
- 综上，提供了代理不同情况下的端口的方法，后面学习会更加容易嗷！！！
- 每次更新了nginx配置文件之后，都要`nginx -s reload`才能够刷新nginx，重新读取配置文件嗷！！！



## 反向代理2：

1. 根据访问的路径不同，转到不同的页面去嗷！！！例如`http://127.0.0.1:9001/edu/`和`http://127.0.0.1/vod/`这两个要跳转到不同的端口去。

- 课程使用的是部署了两个Tomcat，然后改了不同的Tomcat的端口号，并且在webapps下加入了对应的文件夹和定制化页面。
- 这里我们不那么复杂，我们就拿上次部署在8080端口的Tomcat和80端口我们的网页做一个测试就行嗷！

2. 实操：

![image-20210816092259367](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816092307.png)

本质上就是使用正则表达式多加了两个映射功能，当路径含有blog和edu的话，就访问8080端口嗷！！！注意哈，有些东西是有问题的：

![image-20210816092422414](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816092422.png)

![image-20210816092450064](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816092450.png)

![image-20210816092624459](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816092624.png)

- 前面表示路径，最后一个表示资源。所以上面的第一个里面，本质上是请求3344端口映射下的blog资源，那当然找不到哇。后面两个都是在访问/blog路径下的资源，这才符合正则匹配的规则，成功引到了正确的页面。
- 注意！！！

![image-20210816092915828](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816092916.png)



- 并且还有一个大坑！！！

反向代理的原理还待深入研究！！！

```shell
root@8ff1a1258fb6:/usr/local/tomcat/webapps/edu# pwd
/usr/local/tomcat/webapps/edu
root@8ff1a1258fb6:/usr/local/tomcat/webapps/edu# cat a.html
<h1>This is a test of the /edu/a.html path!!!</h1>
```

我在tomcat的webapps目录下，新建了一个edu文件夹，然后新建了a.html文件。

![image-20210816100105792](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816100105.png)

在网页中我访问到了这个文件，这引发了我的思考，这个原理是什么呢？？？

**我得出的结论，这里的反向代理转发，本质上只是把前面的liuyihao.work:3344代理成了我的服务器对应的地址。然后把/edu/a.html转交给了Nginx所代理的服务器，就是说，这里的转发不是直接到那个代理的服务器。而是在规则匹配了之后，将我访问的路径，转交给了代理的服务器，由代理的服务器进行进一步的处理嗷！！！**

- 并且我发现，如果是docker层之间的反向代理，是不受防火墙的约束的，例如我新启动了一个tomcat，服务监听8081，但是我的8081端口没有对外开放。我通过docker的网关，172那个，而不是从宿主机的端口进行访问，是可以成功的嗷！！！

![image-20210816101914750](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816101914.png)



## 负载均衡：

1. 通过浏览器地址栏输入地址，负载均衡效果，平均到8080和8081端口中。现在就是访问3344端口，实现8080/8081，实际上就是轮询两台Tomcat服务器嗷！！！
2. 环境准备：

- 和上面一样的，完成Nginx对于两台Tomcat的访问（通过内部网关），/edu/a.html都要由嗷！！！

3. 实操：

http块儿中加入upstream name {不同的server}

![image-20210816104144668](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816104144.png)

![image-20210816104220690](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816104220.png)

- 这个东西和我们学springcloud的时候，那个注册到nacos，然后用服务名实现负载均衡一个样样的嗷！！！
- 注意，上面那个include嗷，本质上就是把下面这个server引入到了总文件中，没啥区别。
- 还有就是，上面那个记得前面不用http嗷，因为下面的proxy_pass里面已经有http了嗷！！！
- 效果：

![image-20210816104419220](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816104419.png)

![image-20210816104431430](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816104431.png)

刷新的时候，两个交互出现嗷！！！

此外，upstream那个地方，还可以配置每个server的权重哦，在后面可以加上一个weight参数来实现赋予权重嗷！！！

4. 负载均衡策略：

- 轮询：默认的策略，逐一分配，如果后端服务器down了，可以自动剔除嗷！！！
- weight：权重策略，默认为1。权重越高，被分配到的客户端越高（权重为大于等于一的整数嗷，小数没试过，不晓得，可以试试。）
- Ip_Hash：每个请求按照访问ip的Hash结果分配，每个访客固定访问一个后端服务器，可以解决session的问题。

![image-20210816105849268](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816105849.png)

注意哈，这里的ip_hash后面是有;的嗷！！！

可以解决Session共享的问题嗷！！！

- fair（第三方）：按照后端服务器的响应时间来分配，响应时间短的有效分配。

![image-20210816110224146](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816110224.png)

fair后面是有个分号的嗷！！！



## 动静分离：

1. 什么是动静分离？

Nginx把动态和静态请求分开，Nginx处理静态页面，Tomcat处理动态页面。

![image-20210816110757114](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816110757.png)

- 纯粹把静态资源单独提取出来，并且独立成单独的域名，放在独立的服务器上，是目前主流推崇的方案嗷！！！
- 还可以动态和静态混在一起，通过Nginx来分开，不过这种比较少。
- expires参数设置可以设置浏览器缓存过期时间，减少与服务器之前的请求和流量。（和计算机网络学的一样，回去Last Modified，如果没有变化会返回304，如果由变化会返回200，并重新访问服务器获得新的缓存嗷！！！）



2. 准备工作：

- /文件夹下创建data文件夹，文件夹中创建www文件夹，并在www文件夹里面创建test.html文件。
- 想要我们的Nginx帮助我们访问到html文件嗷！！！



3. 实操：

- 配置文件中进行相应的配置嗷！！！！

![image-20210816114105497](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816114105.png)

- 这里配置的意思是，如果我们访问/www/，就会自动帮助我们加上前面的一个root，也就是根目录下的/data文件夹。

![image-20210816114206901](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816114207.png)

- 实际上这里就是location中的配置，实际上帮助我们获得了/data/www/test.html这个静态文件嗷！！！
- 我们访问的是liuyihao.work:3344下的www/test.html资源，然后发现，/www/匹配，于是转到了Nginx中的/data/，加上www/test.html，刚好构成了/data/www/test.html路径，成功访问到了资源嗷！！！
- 此外，如果想显示出文件夹那种效果：

![image-20210816114335077](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816114335.png)

autoindex on，就能够帮助我们列出文件夹目录嗷！！！



## 高可用：

- 为了解决Nginx宕机的效果

![image-20210816115456765](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816115456.png)

- 解决方案：

![image-20210816163314471](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816163314.png)

设置备份服务器嗷！！！

需要额外的软件keepalived，keepalived提供虚拟ip，将其绑定到不同的Nginx服务器，主服务器挂了，就将其绑定到从服务器，通过这种方式来实现高可用嗷！！！！这是一种主从模式嗷！！！

![image-20210816163619355](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816163619.png)



1. 需求：

- 两台Nginx
- 需要keepalived
- 需要虚拟ip

2. 实际准备：

- 两个带有Nginx的container
- Linux宿主机安装keepalived
- 目前为止的端口号：
  - 3344: Nginx01
  - 8080: Tomcat01
  - 8081: Tomcat02
  - 其中前两个是对外暴露了的嗷！！！

3. 安装keepalived：

- yum install keepalived -y
- docker pull keepalived
- 由于端口号的原因，懒得再去开放了，直接使用Linux环境安装keepalived。配置文件处理`/etc/keepalived/keepalived.conf`位置嗷！！！

4. 实操：

- 本质就是修改conf这个配置文件嗷！！！
- 这里我不再演示，需要**两台服务器**，两个Nginx和两个keepalived，分别绑定各自的网卡到同一个ip地址上。然后通过设置的优先级不一样，提供服务嗷！！！
- 这里演示的话，就要新启动两个centos容器，然后配置各自的nginx和keepalived，最后进行实验。
- [高可用配置博客](https://www.cnblogs.com/jinjiangongzuoshi/p/9313438.html)
- 采用的技术：Docker + Nginx + Keepalived

5. 配置解析：

- global_dfs   全局配置
- script   脚本配置（检测脚本配置）

![image-20210816173425888](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816173426.png)

-20表示当脚本中的条件成立，就把此台服务器对应的权重降低20

- 虚拟ip的配置：

![image-20210816173654679](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816173654.png)

[Keepalived配置文件解析](https://www.bilibili.com/video/BV1zJ411w7SV?p=16&spm_id_from=pageDriver)



# Nginx原理

![image-20210816173852490](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816173853.png)

- 原理：

  - 线程分类：

    - master
    - worker

  - 解析：

    - master：

    1. 一个master，多个worker
    2. Master管理，Worker实际工作。

    - worker如何工作：

    1. ![image-20210816174048362](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210816174048.png)
    2. 使用的是争抢的机制嗷！！！

- 好处？

  1. nginx -s reload命令？热加载/热部署方式。

  有任务的worker不参与争抢，其他的worker重新加载嗷！！！利于Nginx进行热部署操作。

  2. 每个worker都是一个独立的进程，不需要加锁。同时，就算有worker挂了，其他的worker还可以进行正常的服务。不会造成服务的中断嗷！！！

- 设置多少个worker合适？

  - IO多路复用（Linux中才能够将服务器的性能发挥到极致嗷！！！）
  - 让worker的数量和CPU数量相同是最合适的嗷！！！

- 连接数work_connection

  - 发送一个请求，占用了worker的几个连接数？
    - 要么是两个，要么是四个
      - 两个：client -> nginx , nginx -> client（静态资源）
      - 四个：client -> nginx , nginx -> tomcat , tomcat -> nginx , nginx -> client（动态资源）

- Nginx有一个master，有四个worker，每一个worker的最大连接数是1024，支持最大的并发数是多少？

  - 就是最多能承受多少个请求？
  - 所有worker支持的最大连接数：4 * 1024
  - 最大并发：4 * 1024 / 2
  - 最小并发：4 * 1024 / 4
  - 结论：

  ![image-20210816190551708](C:\Users\Alexander\AppData\Roaming\Typora\typora-user-images\image-20210816190551708.png)



## 补充：

- 对于Docker中的配置文件，可以采用挂载的形式进行修改嗷！！！
- [Docker挂载实例](https://blog.csdn.net/qq_34556414/article/details/108588203)
- 注意，所谓的挂载

