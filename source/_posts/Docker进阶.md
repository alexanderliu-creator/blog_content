---
title: Docker进阶
date: 2021-08-19 20:48:48
tags: 后端
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819205713.png
---

# B站狂神说docker教程，这是Docker进阶课程，衔接于K8S之前嗷！！！



<!--more-->

- 前置只是补充：

1. [Docker集群搭建实例](https://blog.csdn.net/weixin_41896265/article/details/108245264)
2. [SpringBoot在Docker中的部署实例](https://www.bilibili.com/video/BV1og4y1q7M4?p=39)
3. [Dockerfile编写实例](https://zhuanlan.zhihu.com/p/143109114)



# Docker Compose

## 简介：

- Dockerfile   --->   build   --->   run，手动操作，单个容器嗷！！！
- Compose轻松高效的管理容器，定义运行多个容器。

![image-20210820211207168](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210820211215.png)

- 狂神的理解：
  - Compose是Docker官方的开源项目，需要安装。
  - Dockerfile让程序在任何地方运行。多个容器就要多次run来安装。现在通过配置文件，可以配置它们之间的关系。
  - 重要的概念：
    - 服务services：容器，应用，web , redis , mysql
    - 项目project：一组关联的容器，wp等



## 安装：

- 首先官网上有相应的地址和安装方式嗷，但是从github上拉下来，太慢了orz，可以上网查（评论区有），下载对应的github嗷！！！
- 安装和使用的示例建议遵循官网的例子嗷！！！
- [Compose官网嗷！！！](https://docs.docker.com/compose/)
- Compose帮助我们自动生成了一个网络并将容器全部纳入到了这个网络之下。同时完成了网络打通的功能，各个网络之前能够通过服务名进行访问嗷！！！

```shell
"Containers": {
            "0182740eadaf4282f6356a8728a3ecafef4dfced4ae379f071b6017efb7c3e81": {
                "Name": "composetest_redis_1",
                "EndpointID": "15e44f973a098e0dfef0ca25790a6e953fb18ac6d9b4c666c6ffe9f44d9bc661",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "241be1776b1a1a7a1d7da384e66bc24f5d5676cb30a9175b5421d31769a34b6d": {
                "Name": "composetest_web_1",
                "EndpointID": "cbd7e7e3ceac089f8b69c609ea7c60341f4f70e50100038f019e0620cec1b020",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
```



## Yaml规则：

- docker-compose.yaml的核心：

```yaml
# 三层

version: '' #版本

services: #服务
	服务一: web
	#服务配置
	images:
	build:
	network:
	服务二: redis
	#服务配置
	...
	服务三: 
	...
	
#其他配置，网路/卷，全局规则
volumes:

```

- 能配置什么？怎么配置？参考文档：

[Yaml配置文件具体内容](https://docs.docker.com/compose/compose-file/compose-file-v3/)





## 开源项目（博客）：

- [WordPress项目官方文档](https://docs.docker.com/samples/wordpress/)
- 步骤：
  1. 下载项目（docker-compose.yaml）
  2. 如果需要文件，Dockerfile
  3. 文件准备齐全，一键启动项目



- 前台启动 -> 后台启动：
  - docker -d
  - docker-compose -d

```shell
[root@VM-12-10-centos my_wordpress]# docker-compose up -d
Starting my_wordpress_db_1 ... done
Starting my_wordpress_wordpress_1 ... done
[root@VM-12-10-centos my_wordpress]# 
```

- 把用户密码啊，之类的，都改好之后，一键启动就可以访问到WordPress主页面了。进行注册之后就能够正常使用嗷！！！

![image-20210821101712070](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821101712.png)



- Compose相当于一个集合体，将整个项目需要的依赖和依赖之间的关系都以配置文件的方式配置好，然后一键启动就可以完成对应的整个项目的成功运行嗷！！！



## 自己编写的项目上线：

- 本质上解答了我心中，如何在Java项目中表示项目中的某个依赖是Docker中的某个依赖的问题！！！

- 创建一个Java项目版本的计数器：
  1. 创建项目
  2. POM.xml
  3. applicaton.yaml
  4. 主启动
  5. 业务类：

![image-20210821103410286](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821103410.png)

![image-20210821103459775](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821103459.png)

- 这个项目在本机是跑不起来的，因为我的本机（或者服务器上根本就没有这个host），所以不是常规的写服务器名称和端口号。这里的redis作为host，指的就是docker中提供的redis服务嗷！！！



- Dockerfile的书写：

![image-20210821105855778](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821105855.png)

- docker-compose.yml：

![image-20210821110235199](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821110235.png)

这里就会发现我们定义的redis服务就叫做redis，当java -jar命令启动的时候，docker就能够根据host里面的host : redis去找到docker容器中的redis服务嗷！！！

- 步骤：
  - 编写项目微服务
  - dockerfile构建镜像
  - docker-compose.yaml编排项目！
  - 丢到服务器启动嗷！！！



- 我们需要放入到Linux中的东西：
  - 通过Maven的package我们打的微服务的jar包
  - Dockerfile
  - docker-compose.yml
- 如果上面的三个东西齐全，直接docker-compose up 就能够启动了嗷，可以使用Ctrl + C或者docker-compose down这种形式来关闭服务嗷！！
- 小结：
  - 未来项目只要有docker-compose文件，按照这个规则，启动编排容器

- 狂神的课：

[里面是存在一些大问题的嗷！！！Debug过程很有学习价值嗷！！！](https://www.bilibili.com/video/BV1kv411q7Qc?p=7&spm_id_from=pageDriver)

![image-20210821112209938](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821112210.png)



# Docker Swarm：

[购买阿里云教程嗷！！！](https://www.bilibili.com/video/BV1kv411q7Qc?p=8&spm_id_from=pageDriver)

- 四台机器同步安装docker：

![image-20210821113030822](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821113031.png)

以后在一个会话输入的命令，会自动同步到打开的所有会话嗷！！！

- [配置所在网站](https://docs.docker.com/develop/)，在Run you app in production下面的Configure containers里面的Scale your app就有Swarm相关的内容嗷！！！



## 工作原理：

![image-20210821155248337](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821155249.png)

- 上面用于操作的结点，至少要有3台嗷！！！



## 搭建集群实例：

- swarm相关命令：

```shell
[root@VM-12-10-centos ~]# docker swarm --help

Usage:  docker swarm COMMAND

Manage Swarm

Commands:
  ca          Display and rotate the root CA
  init        Initialize a swarm
  join        Join a swarm as a node and/or manager
  join-token  Manage join tokens
  leave       Leave the swarm
  unlock      Unlock swarm
  unlock-key  Manage the unlock key
  update      Update the swarm

Run 'docker swarm COMMAND --help' for more information on a command.
```

- swarm init相关用法：

```shell
[root@VM-12-10-centos ~]# docker swarm --help

Usage:  docker swarm COMMAND

Manage Swarm

Commands:
  ca          Display and rotate the root CA
  init        Initialize a swarm
  join        Join a swarm as a node and/or manager
  join-token  Manage join tokens
  leave       Leave the swarm
  unlock      Unlock swarm
  unlock-key  Manage the unlock key
  update      Update the swarm

Run 'docker swarm COMMAND --help' for more information on a command.
[root@VM-12-10-centos ~]# docker swarm init --help

Usage:  docker swarm init [OPTIONS]

Initialize a swarm

Options:
      --advertise-addr string                  Advertised address (format: <ip|interface>[:port])
      --autolock                               Enable manager autolocking (requiring an unlock key to start a stopped manager)
      --availability string                    Availability of the node ("active"|"pause"|"drain") (default "active")
      --cert-expiry duration                   Validity period for node certificates (ns|us|ms|s|m|h) (default 2160h0m0s)
      --data-path-addr string                  Address or interface to use for data path traffic (format: <ip|interface>)
      --data-path-port uint32                  Port number to use for data path traffic (1024 - 49151). If no value is set or is set to 0, the default port (4789) is used.
      --default-addr-pool ipNetSlice           default address pool in CIDR format (default [])
      --default-addr-pool-mask-length uint32   default address pool subnet mask length (default 24)
      --dispatcher-heartbeat duration          Dispatcher heartbeat period (ns|us|ms|s|m|h) (default 5s)
      --external-ca external-ca                Specifications of one or more certificate signing endpoints
      --force-new-cluster                      Force create a new cluster from current state
      --listen-addr node-addr                  Listen address (format: <ip|interface>[:port]) (default 0.0.0.0:2377)
      --max-snapshots uint                     Number of additional Raft snapshots to retain
      --snapshot-interval uint                 Number of log entries between Raft snapshots (default 10000)
      --task-history-limit int                 Task history retention limit (default 5)
```

- 具体操作：

  - docker swarm init 初始化结点
  - docker swarm join加入一个结点：

  ![image-20210821161225306](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821161225.png)

  - 获取令牌：

  ```shell
  docker swarm join-token manager
  docker swarm join-token worker
  ```

  上面这两个命令是生成加入某个结点的指令的命令嗷，就是说，如果你想以Worker加入结点A，你就进入到结点A中，执行`docker swarm join-token worker`，然后复制A中的指令到B中执行，即可获得结果嗷！！！

  - docker node ls：

  这个可以显示出所有的结点嗷！！！

  - docker swarm leave：

  不用参数，这个就可以直接离开集群。





## Raft协议：

- 协议内容：保证**大多数结点存活**才可以使用，只要>1，集群至少要大于3台嗷！！！

- 双主双从：假设一个主节点挂了，另外一个主节点是否可以用？另外一个主节点就不可以用了嗷！！！

并且，不可以用的结点复活之后，虽然还是管理结点，但是已经不是最大的老大了嗷！！！

- Worker节点就是工作的，管理节点才能执行操作的命令嗷！！！
- worker就是工作的，管理节点操作！
- 三个管理结点，一台worker，一个管理结点报废了，其他还是可以使用的嗷！！！
- 十分简单：集群，可用！！！三个主节点，>1台管理者存活才能使用嗷！！！Raft保证大多数结点存活才能使用哈！！！



## 集群创建服务（Docker Service）：

- docker-compose up！启动一个单机的项目哈！！！
- 容器 ===> 服务
- redis = 3份！容器！集群：高可用，web -> redis（3台，分布在不同的机器上嗷！）
- 容器的概念逐渐变为了服务，服务主逐渐又衍生出了副本。
- Redis服务   --->   10个服务，从dcoker-compose逐步演变为docker service嗷！！！

```shell
[root@VM-12-10-centos ~]# docker service --help

Usage:  docker service COMMAND

Manage services

Commands:
  create      Create a new service
  inspect     Display detailed information on one or more services
  logs        Fetch the logs of a service or task
  ls          List services
  ps          List the tasks of one or more services
  rm          Remove one or more services
  rollback    Revert changes to a service's configuration
  scale       Scale one or multiple replicated services
  update      Update a service

Run 'docker service COMMAND --help' for more information on a command.
```

- 拥有创建服务，动态拓展服务，动态更新服务等功能。
- 概念：
  - 灰度发布
  - 金丝雀发布
- 创建一个服务：

`docker service create -p 8080:80 --name my-nginx nginx`

![image-20210821165002947](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821165003.png)

> docker run   容器启动！不具有扩缩容功能
>
> docker service   具有扩缩容功能，滚动更新！！！

- 查看服务：

> docker ps
>
> docker service ps

![image-20210821165348665](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821165349.png)

- 动态扩容：

> docker service update --replicas 3 my-nginx

![image-20210821165639725](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821165639.png)

启动的微服务可以在集群中的结点中分布嗷！！！

**在集群的任何一台机子，都能够访问到这个服务，就算它上面没有跑服务。集群就是一个大的整体嗷！！！**

集群中的任意结点都可以访问，服务中有多个副本动态扩缩容来实现我们的高可用。例如上面，我们通过第一个服务名，成功复制出了若干个微服务副本跑在不同机器上，整体提供服务嗷！！！

- 动态回滚到一个微服务：

> docker service update --replicas 1 my-nginx

- 弹性和扩缩容的概念！服务端高可用和服务器的高可用。服务的高可用，任何企业，云！！！k8s！服务云原生运用嗷！！！
- 工作结点才能执行命令哈！！！
- 另外的扩缩容命令：

> docker service scale my-nginx=5

和我们更新副本数是一模一样的嗷！！！

- 移除服务：

> docker service rm my-nginx

![image-20210821170529865](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821170530.png)





## 概念总结：

**swarm**

集群的管理和编号，docker可以初始化一个swarm，其他节点可以加入。（Worker和Manager）

**Node**

就是一个docker节点，多个节点组成了网络集群（管理，工作者）

**Service**

任务，可以在管理节点或工作节点来运行，核心。

**Task**

容器内的命令，细节任务！

![image-20210821170959132](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821170959.png)

- Service：

![image-20210821171036165](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821171036.png)

- 内部原理：

![image-20210821171105631](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821171106.png)

- 副本跑在哪儿？

![image-20210821173816139](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821173816.png)

如果工作结点不跑东西，可以让它在副本上跑嗷！！！

![image-20210821173909342](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821173909.png)

核心就是参数而已嘛！！！



## 拓展（网络）：

![image-20210821174731644](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821174731.png)

- 虽然Docker在四台机器上，实际网络是同一个嗷！！！

![image-20210821174850013](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821174850.png)

- 虚拟网络技术：

![image-20210821175136707](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210821175136.png)

分开的变成了一个集群整体嗷！！！

# 其他技术：

## Docker Stack

- docker-compose用于单机部署项目
- Docker Stack用于集群部署

> docker-compose up -d wordpress.yml
>
> docker-stack up -d wordpress.yml

```shell
[root@VM-12-10-centos ~]# docker stack

Usage:  docker stack [OPTIONS] COMMAND

Manage Docker stacks

Options:
      --orchestrator string   Orchestrator to use
                              (swarm|kubernetes|all)

Commands:
  deploy      Deploy a new stack or update an existing stack
  ls          List stacks
  ps          List the tasks in the stack
  rm          Remove one or more stacks
  services    List the services in the stack

Run 'docker stack COMMAND --help' for more information on a command.
[root@VM-12-10-centos ~]# docker stack

Usage:  docker stack [OPTIONS] COMMAND

Manage Docker stacks

Options:
      --orchestrator string   Orchestrator to use (swarm|kubernetes|all)

Commands:
  deploy      Deploy a new stack or update an existing stack
  ls          List stacks
  ps          List the tasks in the stack
  rm          Remove one or more stacks
  services    List the services in the stack

Run 'docker stack COMMAND --help' for more information on a command.
```

## Docker Secret

- 密码啊，证书的之类的

```shell
[root@VM-12-10-centos ~]# docker secret

Usage:  docker secret COMMAND

Manage Docker secrets

Commands:
  create      Create a secret from a file or STDIN as content
  inspect     Display detailed information on one or more secrets
  ls          List secrets
  rm          Remove one or more secrets

Run 'docker secret COMMAND --help' for more information on a command.
```



## Docker  Config

- Docker对于容器的统一的一些配置嗷！！！

```shell
[root@VM-12-10-centos ~]# docker config

Usage:  docker config COMMAND

Manage Docker configs

Commands:
  create      Create a config from a file or STDIN
  inspect     Display detailed information on one or more configs
  ls          List configs
  rm          Remove one or more configs

Run 'docker config COMMAND --help' for more information on a command.
[root@VM-12-10-centos ~]# 
```

- 查什么docker config案例啊，例子啊，网上一大堆，看看示例就晓得了嗷！！！
- 学习方式：网上找案例试试嗷！！！然后查看帮助文档--help , 官网等



# 拓展到K8S

## 云原生时代（大趋势）

- k8s，10台以上的集群嗷！！！
- Go语言！必须掌握！！！
  - Docker是Go开发的
  - K8S也是Go开发的
  - Etcd还是Go开发的



- 本质上这里学习的就是从单机到集群的指令的迁移嗷，集群有自己单独的一套指令嗷！！！
  - Docker   --->   Docker Compose
  - Docker   --->   Docker Swarm   ,   Docker Compose   --->   Docker Stack等

