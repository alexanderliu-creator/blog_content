---
title: Docker学习
date: 2021-05-19 16:00:09
tags: 后端
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210819205713.png
---

# B站狂神说docker教程，SpringMVC太干了听不下去，先听会儿这个转换心情。



<!--more-->



# Docker入门

## Docker为啥会出现

- 开发 ---> 上线 ， 两套环境！应用环境，应用配置！

- 问题：我在我都电脑上可以跑哇？？？
- 版本更新，导致服务不可用，运维裂开来。
- 开发即运维。

> - 发布一个项目 jar + (Redis MySQL jdk ES) , 环境和项目打包，项目带上环境安装打包
> - 传统：
>   - 传统：开发jar , 运维来做
>   - 现在：开发打包部署上线，一套流程完成
> - Docker为上面问题提供了解决方案嗷！！！
> - Docker仓库就类似于镜像，环境和内部打包好了就上线到商店里面，有需要下载下来就可以直接跑的嗷！

- 隔离是docker的核心思想，每个箱子是相互隔离的。
- docker的隔离机制可以充分利用服务器资源，端口冲突的问题也可以通过docker来解决。
- 本质：新的技术的出现都是为了解决问题。





## Docker的历史

- 2010年一帮人搞了一个容器化技术，命名为Docker。为了解决类似于VMWare，太重量的东西。
- 2013年Docker开源了，Docker是真的非常好用哦！！！每一个月Docker都会更新一个版本。
- 2014年4月9日，Docker1.0发布。
- 为啥火？容器技术出现之前，使用虚拟机技术。

> 虚拟机：win10上装一个VMWare，子电脑，可以虚拟电脑，但是太大了笨重。
>
> Docker：容器技术，本质上也是虚拟化技术，但是轻量级。镜像（最核心的环境），十分小巧，运行镜像就可以了。小巧。（几兆，几十兆就可以启动，秒级启动，爽啦。）

- Docker是基于go语言开发的开源项目。

[docker的官方文档](https://docs.docker.com/get-docker/)，这个文档超级详细，力赞。

[dockerhub官网](https://hub.docker.com/)，和github是一样的，和github很像，push pull都有。



## Docker能干啥

- 虚拟机技术的缺点：
  - 资源占用多
  - 冗余步骤多
  - 启动很慢

![image-20210521172228149](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210521172228.png)

- 容器化技术：
  - 不是模拟一个完整的容器：

![image-20210521172330732](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210521172330.png)

  - 容器没有自己的内核，也没有虚拟我们的硬件，所以就轻便了。
  - 每个容器相互隔离的，不影响的。
  - 应用更加快速的交付和部署。
  - 更加便捷的升级和扩缩容量。
  - 更加简单的系统运维。
  - 更高效的计算资源利用，Docker是内核级别的虚拟化嗷，可以将服务器的性能压榨到极致。



## Docker的架构

![image-20210521173517895](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210521173517.png)

- 从左到右为服务端，客户端和仓库。

### 镜像( image )：

- docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，tomcat -> run -> 容器。一个镜像可以创建多个容器。

### 容器( container )：

- Docker利用容器技术，独立运行一个或者一个组运用，通过镜像来创建。
- 启动，停止，删除，基本命令
- 目前就可以把这个容器理解为一个简易的linux系统

### 仓库(  repository )：

- 仓库就是存放镜像的地方。
- 仓库分为公有仓库和私有仓库。
- Docker Hub 默认是国外的，要配置镜像加速的嗷！



## 安装Docker：

> 环境准备：
>
> - Linux基础
> - CentOS 7.6
> - 使用Xshell连接远程服务器进行操作

[Docker安装教程](https://www.bilibili.com/video/BV1og4y1q7M4?p=6)

- 可以在腾讯云上实操嗷！！！





### 安装步骤：

> 查看现在发行的版本：

```shell
cat /etc/os-release
# 上面这个可以查看内核版本
# 下面是显示出来的系统版本，要7以上
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

> 帮助文档中选择对应的系统进行安装：

![image-20210522160140340](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210522160147.png)

- 接着这里的步骤一步步往下走就好了嗷！！！

```shell
# 1. 卸载旧的版本
 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
# 2.需要的安装包
 sudo yum install -y yum-utils
 
 
 # 3.设置镜像仓库，这一步不建议用国外的网站，这里找国内的来用嗷！！！！（不推荐，看下一条嗷！！！）
 sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 3.改用国内的镜像（阿里云镜像）来设置仓库地址：
 sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 更新yum软件包索引
yum makecache fast

# 4.安装docker相关的好东西
 sudo yum install docker-ce docker-ce-cli containerd.io
# docker-ce 是社区版的，ee是企业版的，推荐使用ce。到这里安装就结束了

# 5. 启动docker
sudo systemctl start docker

# 6. 运行hello-world 映像来验证是否正确安装了Docker Engine 。
sudo docker run hello-world
```

- 如果要安装指定版本的话，官网有教程哦：

![image-20210522161646888](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210522161646.png)

- 当显示下面的内容的时候，docker安装成功：
  - 执行docker run hello-world：
    - ![image-20210522161855830](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210522161855.png)
  - 使用docker version查看docker版本：
    - ![image-20210522161921240](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210522161921.png)



### Docker run hello-world：

- 首先Unable to find image ， 就是没有找到这个镜像
- 其次就去网上pull hello-world这个东西了。
- 下载完成之后再执行这个镜像，显示执行结果：

![image-20210522162121571](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210522162121.png)



```shell
# 查看下载的hello-image镜像
docker images
```

- 结果如下：

![image-20210522162332833](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210522162332.png)







### 卸载docker：

1. 卸载Docker Engine，CLI和Containerd软件包：

   ```shell
   $ sudo yum remove docker-ce docker-ce-cli containerd.io
   ```

2. 主机上的映像，容器，卷或自定义配置文件不会自动删除。要删除所有图像，容器和卷：

   ```shell
   $ sudo rm -rf /var/lib/docker
   $ sudo rm -rf /var/lib/containerd
   ```

您必须手动删除所有已编辑的配置文件。



- 官方文档真香啊！！！



### 配置腾讯云加速：

[腾讯云官方文档](https://cloud.tencent.com/document/product/1207/45596)



- 适用于 Linux 操作系统实例：

  1. 执行以下命令，打开配置文件。

     ```shell
vim /etc/docker/daemon.json
     ```
     
     切换至编辑模式，添加以下内容，并保存。

     ```shell
{
     "registry-mirrors": [
 "https://mirror.ccs.tencentyun.com"
     ]
     }
     ```
     
  2. 执行以下命令，重启 Docker 即可。示例命令以 CentOS 7 为例。
  
     ```shell
   sudo systemctl restart docker
     ```



### Hello-World流程：

- 流程分析图：

![image-20210522164949462](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210522164949.png)

## 底层原理：

- 怎么工作的？
  - 本质上是一个C/S接口系统，Docker守护进程运行在主机上，通过socket从客户端访问。
  - DockerServer接收到Docker-Client的指令，就会执行这个命令。
- 内部结构：

![image-20210522171235487](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210522171235.png)

- 后台是一个大的Linux服务器，通过客户端去连接后台的守护进程，Docker容器在我们的范围之内。每一个小的docker容器好比是一个小的Linux虚拟机。容器内有自己的端口号，和外界隔离，外界无法访问，只有守护进程能访问。大小Linux相互隔离，因此外界大Linux要访问小的Linux，需要一些技术将其连通。
- Docker为啥比VM快？
  - Docker有更少的抽象层
  - Docker利用的是宿主机的内核，vm需要的是Guest OS。
  - 新建一个容器的时候，不需要加载虚拟机一样的操作系统内核，避免引导，因此很快。省略了复杂的引导过程。
- 之后学命令就会很快乐，很清晰。



## Docker常用命令：

#### docker version / info / help：

```shell
docker version		#显示版本信息
docker info			#显示docker详细的系统信息
docker 命令 --help   #docker的帮助文档
```

- 官网也可以在帮助文档中的reference找到所有的命令和列表。[Docker帮助文档](https://docs.docker.com/reference/)
- 但是官网在国外会比较慢，所以我们这里直接使用命令进行查找用法即可。



### 常用镜像命令：

#### docker images：

- ```shell
  [root@VM-12-10-centos ~]# docker images
  REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
  hello-world   latest    d1165f221234   2 months ago   13.3kB
  ```

- Repository：镜像的仓库源

- Tag：镜像的标签

- Image ID：镜像的id

- Created：镜像的创建时间

- Size：镜像的大小

- 可选项：

  - -a , 列出所有镜像
  - -q , 只显示id

#### docker search:

- shell中所搜镜像源头

- ```shell
  [root@VM-12-10-centos ~]# docker search mysql
  NAME                              DESCRIPTION                                  STARS     OFFICIAL  AUTOMATED
  mysql                             MySQL is a widely used, open-source relation…   10903     [OK]       
  mariadb                           MariaDB Server is a high performing open sou…   4115      [OK]       
  mysql/mysql-server                Optimized MySQL Server Docker images. Create…   809                  [OK]
  ```

- 可选项：

  - --filter , 过滤器，过滤出符合条件的内容。例如`--filter=STARS=3000`，就只会显示STARS中3000以上的内容。

  - ```shell
    [root@VM-12-10-centos ~]# docker search mysql --filter=STARS=3000
    NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
    mysql     MySQL is a widely used, open-source relation…   10903     [OK]       
    mariadb   MariaDB Server is a high performing open sou…   4115      [OK]  
    ```



#### docker pull：

![image-20210522180106175](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210522180106.png)

- 下载镜像采用分层的方式
- Digest是一个签名
- 最后是一个下载的真实地址

下面两条是等价的，你这里pull mysql实际上就是从下面这个地址去pull



- 指定版本：
  - 需要是系统支持的版本，在docker hub中可以看到有哪些版本的。
  - `docker pull mysql:5.7`
- 分层下载：
  - 多个文件有共享的内容就不用分层进行下载了，直接可以共用哦。



#### docker rmi：

- 可以通过id或者是name来删除。
- 删除过程中也是按照层来删！！！
- `docker rmi -f name/id`，删除指定容器
- `docker rmi -f 容器id 容器id 容器id`，删除多个容器
- 这里-f表示的是强制删除的参数嗷！！！

```shell
[root@VM-12-10-centos ~]# docker rmi -f d1165f221234
Untagged: hello-world:latest
Untagged: hello-world@sha256:5122f6204b6a3596e048758cabba3c46b1c937a46b5be6225b835d091b90e46c
Deleted: sha256:d1165f2212346b2bab48cb01c1e39ee8ad1be46b87873d9ca7a4e434980a7726
```

- 批量删除（用到了linux中的技巧哦）：
  - `docker rmi -f $(docker images -aq)`，这样可以删除所有镜像。



### 常用容器命令：

- 有了镜像才可以创建容器，linux，下载一个centos镜像来测试学习:

```shell
docker pull centos
```



#### docker run：

- docker run [可选参数] image
- 参数说明：
  - --name="Name"   容器名字用来区分名字
  - -d   后台方式运行
  - -it   使用交互方式运行，进入容器查看内容
  - -p   指定容器端口：
    - 主机端口映射到容器端口（最常使用）
    - 直接容器端口
    - ip：主机端口:容器端口
  - -P   随机指定端口

- 实际进入容器的例子：

```shell
[root@VM-12-10-centos ~]# docker run -it centos /bin/bash
[root@0c9bb9c70481 /]

[root@0c9bb9c70481 /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
```

- 启动并进入容器，主机名称变了兄弟，现在进入到我们的容器里面去了，容器里面和外面是两个世界。里面是一个非常非常轻量的服务器，甚至指令都不完善的。
- 从容器中退回主机：exit可以退出进入的容器。



#### docker  ps：

- 显示正在运行的程序
- 参数大全： 
- -a , 显示历史运过的容器：

```shell
[root@VM-12-10-centos ~]# docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS                     PORTS     NAMES
0c9bb9c70481   centos         "/bin/bash"   5 minutes ago   Exited (0) 2 minutes ago             laughing_shamir
0e312a95099c   d1165f221234   "/hello"      3 hours ago     Exited (0) 3 hours ago               crazy_cray
```

- -n=？ 显示最近创建的容器，可以给个数。
- -q  只显示容器的编号

```shell
[root@VM-12-10-centos ~]# docker ps -aq
2ab97137c167
5650592ad646
e6778cc94fee
```

- 上面这个例子可以显示全部容器的编号



#### 退出容器：

- exit，停止并退出容器
- Ctrl + P + Q ， 容器不停止同时退出容器



#### docker rm：

- 删除容器
- `docker rm 容器id`
- `docker rm -f $(docker ps -aq)`，删除所有的容器
- 运行中的容器不能够删除，如果要强制删除的话要`rm -f`
- `docker ps -a -q|xargs docker rm`，这个也可以删除所有容器。



#### 启动和停止容器的操作：

```shell
docker start 容器id 	#启动
docker restart 容器id #重启
docker stop 容器id	#停止正在运行的容器
docker kill 容器id	#强制停止当前容器
```



### 常用其他命令：

#### docker run -d 容器名

- 通过`docker run -d 镜像名`来后台跑起来一个容器
- 但是你发现启动完成之后，docker ps -a看不到？？？它自动停掉了。
- reason：
  - docker是容器使用后台运行，必须要有一个前台进程，docker发现没有前台提供服务的应用，就会自动停止。
  - 容器启动后，发现自己没有提供服务，就会立即停止，没有程序了。



#### docker logs

- 参数：
  - -f：format，标准化输出。
  - -t：显示时间戳。
  - 还有好多...

```shell
docker logs -tf --tail 要显示的日志条数 容器id
# -tf 显示日志
# --tail number 显示日志条数
dockers logs -tf 容器id
#上面这个就可以显示全部的日志
#上面两个都可以不断更新最新的日志
```

- 上面这个可以显示容器内的日志，但有可能容器啥都没干，就无日志



#### 查看进程信息

- docker top 容器id
- 会显示出容器中所有进程的id



#### 查看镜像元数据

- docker inspect 容器id

```shell
[root@VM-12-10-centos ~]# docker inspect hello-world
[
    {
        "Id": "sha256:d1165f2212346b2bab48cb01c1e39ee8ad1be46b87873d9ca7a4e434980a7726",
        "RepoTags": [
            "hello-world:latest"
        ],
        "RepoDigests": [
            "hello-world@sha256:5122f6204b6a3596e048758cabba3c46b1c937a46b5be6225b835d091b90e46c"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2021-03-05T23:25:25.230064203Z",
        "Container": "f5a78ef54769bb8490754e9e063a89f90cc8eee6a6c5a0a72655826e99df116e",
        "ContainerConfig": {
            "Hostname": "f5a78ef54769",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"/hello\"]"
            ],
            "Image": "sha256:77fe0a37fa6ce641a004815f2761a9042618557d253f312cd3da61780e372c8f",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "19.03.12",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/hello"
            ],
            "Image": "sha256:77fe0a37fa6ce641a004815f2761a9042618557d253f312cd3da61780e372c8f",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 13336,
        "VirtualSize": 13336,
        "GraphDriver": {
            "Data": {
                "MergedDir": "/var/lib/docker/overlay2/780de7dbba67a06f81bbd4e90bf01dbb79656623b604472bcf9d0edaba8687cd/merged",
                "UpperDir": "/var/lib/docker/overlay2/780de7dbba67a06f81bbd4e90bf01dbb79656623b604472bcf9d0edaba8687cd/diff",
                "WorkDir": "/var/lib/docker/overlay2/780de7dbba67a06f81bbd4e90bf01dbb79656623b604472bcf9d0edaba8687cd/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:f22b99068db93900abe17f7f5e09ec775c2826ecfe9db961fea68293744144bd"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]

```

- docker inspect 容器id/镜像id都可以看嗷！！！



#### 进入正在运行的容器

```shell
docker exec -it 容器id /bin/bash
docker attach 容器id
```

- 区别：
  - exec，进入容器后开启一个新的终端。
  - attach，不会启动新的进程，直接进入正在执行的终端。



#### 从容器内拷贝文件到主机

- 要进入容器内再拷贝出来
- `docker cp 容器id:/文件夹/文件 /宿主机内文件夹/文件`
- 拷贝是一个手动过程，未来使用 -v 卷的技术，可以实现自动拷贝。





## 小结：

![image-20210523094900703](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523094907.png)



## 小作业1（部署Nginx）:

> Docker安装nginx



```shell
#1. docker search 搜索镜像，去dockerhub官网搜索可以看到详细的帮助文档
#2. docker pull 下载镜像
```

```shell
[root@VM-12-10-centos ~]# docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
69692152171a: Pull complete 
49f7d34d62c1: Pull complete 
5f97dc5d71ab: Pull complete 
cfcd0711b93a: Pull complete 
be6172d7651b: Pull complete 
de9813870342: Pull complete 
Digest: sha256:df13abe416e37eb3db4722840dd479b00ba193ac6606e7902331dcea50f4f1f2
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
[root@VM-12-10-centos ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
nginx         latest    f0b8a9a54136   10 days ago    133MB
hello-world   latest    d1165f221234   2 months ago   13.3kB
centos        latest    300e315adb2f   5 months ago   209MB
[root@VM-12-10-centos ~]# docker run -d --name nginx01 -p 3344:80 nginx
# 这里的话对于设置nginx后台运行，宿主机的3344对应容器内的80端口，同时给这个容器命名为nginx01
68b3ef85158ed8d58fd92a6af097fb03c2cbed3a1b4118c27918444639b9de95
```

- 上面完成了nginx的下载与部署，下面进行访问测试：

```shell
[root@VM-12-10-centos ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
68b3ef85158e   nginx     "/docker-entrypoint.…"   22 minutes ago   Up 22 minutes   0.0.0.0:3344->80/tcp, :::3344->80/tcp   nginx01
[root@VM-12-10-centos ~]# curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

- 端口暴露的概念图：

![image-20210523110242571](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523110242.png)

- 可以进入nginx容器中查看nginx相关的信息：

```shell
[root@VM-12-10-centos ~]# docker exec -it nginx01 /bin/bash
root@68b3ef85158e:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@68b3ef85158e:/# cd /etc/nginx
root@68b3ef85158e:/etc/nginx# ls
conf.d		koi-utf  mime.types  nginx.conf   uwsgi_params
fastcgi_params	koi-win  modules     scgi_params  win-utf
```

- 由于你将本机端口和容器内部端口进行了映射，访问本机端口就可以获得服务，这个时候在服务器配置中开放端口就可以访问到你在对应端口开放的服务。
- 思考问题：每次改动配置文件都需要进入容器内部十分麻烦。我们要是可以在容器外部提供映射路径，达到在容器外的修改可以同步到容器内，就十分的方便嗷！！！-v 数据卷的技术。



## 小作业2（部署Tomcat）:

- 官方文档中提供的安装Tomcat的命令

![image-20210523114616832](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523114616.png)

```shell
[root@VM-12-10-centos ~]# docker run -it --rm tomcat:9.0
Unable to find image 'tomcat:9.0' locally
9.0: Pulling from library/tomcat
d960726af2be: Pull complete 
e8d62473a22d: Pull complete 
8962bc0fad55: Pull complete 
65d943ee54c1: Pull complete 
da20b77f10ac: Pull complete 
8669a096f083: Pull complete 
e0c0a5e9ce88: Pull complete 
f7f46169d747: Pull complete 
42d8171e56e6: Pull complete 
774078a3f8bb: Pull complete 
Digest: sha256:10842dab06b5e52233ad977d4689522d4fbaa9c21e6df387d7a530e02316fb45
Status: Downloaded newer image for tomcat:9.0
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/openjdk-11
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
NOTE: Picked up JDK_JAVA_OPTIONS:  --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
23-May-2021 03:47:09.425 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version name:   Apache Tomcat/9.0.46
23-May-2021 03:47:09.427 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server built:          May 8 2021 17:35:52 UTC
23-May-2021 03:47:09.427 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version number: 9.0.46.0
23-May-2021 03:47:09.427 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Name:               Linux
23-May-2021 03:47:09.427 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Version:            3.10.0-1127.19.1.el7.x86_64
23-May-2021 03:47:09.427 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Architecture:          amd64
23-May-2021 03:47:09.428 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Java Home:             /usr/local/openjdk-11
23-May-2021 03:47:09.428 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Version:           11.0.11+9
23-May-2021 03:47:09.428 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Vendor:            Oracle Corporation
23-May-2021 03:47:09.430 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_BASE:         /usr/local/tomcat
23-May-2021 03:47:09.430 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_HOME:         /usr/local/tomcat
23-May-2021 03:47:09.457 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.lang=ALL-UNNAMED
23-May-2021 03:47:09.457 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.io=ALL-UNNAMED
23-May-2021 03:47:09.457 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.util=ALL-UNNAMED
23-May-2021 03:47:09.457 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.util.concurrent=ALL-UNNAMED
23-May-2021 03:47:09.457 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
23-May-2021 03:47:09.457 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties
23-May-2021 03:47:09.457 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
23-May-2021 03:47:09.457 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djdk.tls.ephemeralDHKeySize=2048
23-May-2021 03:47:09.458 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.protocol.handler.pkgs=org.apache.catalina.webresources
23-May-2021 03:47:09.458 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dorg.apache.catalina.security.SecurityListener.UMASK=0027
23-May-2021 03:47:09.458 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dignore.endorsed.dirs=
23-May-2021 03:47:09.458 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.base=/usr/local/tomcat
23-May-2021 03:47:09.458 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.home=/usr/local/tomcat
23-May-2021 03:47:09.458 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.io.tmpdir=/usr/local/tomcat/temp
23-May-2021 03:47:09.468 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent Loaded Apache Tomcat Native library [1.2.28] using APR version [1.6.5].
23-May-2021 03:47:09.468 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent APR capabilities: IPv6 [true], sendfile [true], accept filters [false], random [true].
23-May-2021 03:47:09.468 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent APR/OpenSSL configuration: useAprConnector [false], useOpenSSL [true]
23-May-2021 03:47:09.478 INFO [main] org.apache.catalina.core.AprLifecycleListener.initializeSSL OpenSSL successfully initialized [OpenSSL 1.1.1d  10 Sep 2019]
23-May-2021 03:47:10.220 INFO [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["http-nio-8080"]
23-May-2021 03:47:10.283 INFO [main] org.apache.catalina.startup.Catalina.load Server initialization in [1275] milliseconds
23-May-2021 03:47:10.390 INFO [main] org.apache.catalina.core.StandardService.startInternal Starting service [Catalina]
23-May-2021 03:47:10.390 INFO [main] org.apache.catalina.core.StandardEngine.startInternal Starting Servlet engine: [Apache Tomcat/9.0.46]
23-May-2021 03:47:10.420 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["http-nio-8080"]
23-May-2021 03:47:10.511 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [228] milliseconds
^C23-May-2021 03:47:26.727 INFO [Thread-3] org.apache.coyote.AbstractProtocol.pause Pausing ProtocolHandler ["http-nio-8080"]
23-May-2021 03:47:26.734 INFO [Thread-3] org.apache.catalina.core.StandardService.stopInternal Stopping service [Catalina]
23-May-2021 03:47:26.737 INFO [Thread-3] org.apache.coyote.AbstractProtocol.stop Stopping ProtocolHandler ["http-nio-8080"]
23-May-2021 03:47:26.783 INFO [Thread-3] org.apache.coyote.AbstractProtocol.destroy Destroying ProtocolHandler ["http-nio-8080"]
[root@VM-12-10-centos ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

- 我们发现容器跑起来和在Tomcat部署上的一摸一样，这个时候`ctrl+c`停掉Tomcat服务，`docker ps -a`显示一下容器我们发现它就没有了！！！
- 我们这儿不这么跑哈，不太好嗷！！！直接pull , run 起来好一点嗷！！！
- `docker pull tomcat`
- `docker run -d -p 3344:8080 --name tomcat01 tomcat`

- 本地的curl命令和远程访问3344端口都没问题：

![image-20210523122418187](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523122418.png)

可以看到Apache服务器已经部署成功了！！！

- 进入容器进行查看：

```shell
[root@VM-12-10-centos ~]# docker exec -it tomcat01 /bin/bash
root@57104ebe62bb:/usr/local/tomcat# ls
BUILDING.txt	 NOTICE		RUNNING.txt  lib	     temp	   work
CONTRIBUTING.md  README.md	bin	     logs	     webapps
LICENSE		 RELEASE-NOTES	conf	     native-jni-lib  webapps.dist
root@57104ebe62bb:/usr/local/tomcat# cd webapps
root@57104ebe62bb:/usr/local/tomcat/webapps# ls
root@57104ebe62bb:/usr/local/tomcat/webapps# 
```

- 发现问题：
  - 命令被阉割了，类似于ll这样的命令都没有了
  - webapps底下啥都没有，原因是默认是最小的镜像，所有不必要的就都剔除掉，保证最小可运行的环境。
- 实际文件藏在webapps.dist文件里面，就是基本的页面，把其复制到webapps中就可以正常跑起来啦！！！

```shell
root@57104ebe62bb:/usr/local/tomcat# ls webapps
root@57104ebe62bb:/usr/local/tomcat# ls webapps.dist/
ROOT  docs  examples  host-manager  manager
root@57104ebe62bb:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@57104ebe62bb:/usr/local/tomcat# cd webapps
root@57104ebe62bb:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
```

- 这个时候无论是本地curl还是远程:3344，都能够访问到apache的初始页面了嗷！！！

- 思考问题：我们以后部署项目，每次进入容器是不是很麻烦？我要是可以提供一个映射路径：webapps，我们在外部放置项目，就自动同步到内部就好了。





## 小作业3（部署ES+Kibana）：

[ES+Kibana平台](https://www.bilibili.com/video/BV1og4y1q7M4?p=16&spm_id_from=pageDriverhttps://www.bilibili.com/video/BV1og4y1q7M4?p=16&spm_id_from=pageDriver)

- 比较耗费资源和内存，我这儿不用自己的虚拟机做演示了嗷！！！老师部署的都裂开来！！！
- elasticsearch就是我们要下载部署的东西嗷！！！
- 部署了服务器都顶不住，直接卡住了，一核2G根本顶不住。解决方法：
  - 停整个docker
  - 服务器直接重启orz
  - 等服务器缓过来
- 查看状态：`docker stats`,这是一个比较耗资源的操作嗷：

![image-20210523165518965](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523165526.png)

- 增加内存的限制才可以嗷！！！
  - ![image-20210523165856964](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523165857.png)
  - 这里的是-Xms , -Xmx限制的是最大和最小内存。
  - 它的前面的这部分代码上dockerhub去看官网文档的用法嗷！！！
- 作业：如何使用Kibana去连接ES呢？两者是相互隔离的，就非常顶？？？解决：通过Linux的内网地址，以Linux为中介，去连接ES。

![image-20210523170234877](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523170235.png)

# 可视化工具：

## portainer(先用这个)：

- docker的图形化管理工具，提供一个后台面板供我们操作
- 操作指令：

```shell
docker run -d -p 3344:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

- 然后访问3344端口就能够访问到：

![image-20210523171252987](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523171253.png)

- 创建用户之后使用本地的就好啦，很方便嗷！

![image-20210523171425800](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523171425.png)

进入后，可以看到我们的主面板：

![image-20210523171614813](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523171614.png)

- 可视化面板，挺好用的，就是一般不会用orz。





## Rancher(CI/CD再用)















# Docker镜像

## 镜像是什么

- 所有的引用直接打包为docker镜像，就可以直接跑起来
- 得到镜像：
  - 远程仓库下载
  - 朋友拷贝给你
  - 自己制作一个镜像dockerfile



## 镜像加载原理（层级的概念）

- `Union File System`，联合文件系统，是一种分层，轻量级并且高性能的文件系统，它支持对于文件系统的修改作为一次提交来一层层叠加。同时可以把不同目录挂载到同一个虚拟文件系统下。镜像可以通过分层来进行集成，基于基础镜像，可以制作各种具体的应用镜像。
- 精简的OS，所以可以很小，底层使用的是Host的kernel，因此为秒级启动。
- 有点像windows里面的更新补丁，盖中盖。

![image-20210523190339127](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523190339.png)

- 分层下载可以复用相同的层。这里注意一下，下面这个很重要，我们不可以写只读的镜像，我们只可以操作启动的容器，相当于在镜像上面多加了一层容器层进行操作，新的这一整个整体可以被打包为一个全新的镜像嗷！！！

![image-20210523190514390](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523190514.png)

![image-20210523190752317](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523190752.png)



## commit镜像

- `docker commit -m="提交的信息" -a="作者" 容器id 目标镜像名: [TAG]`

- 命令和git类似

- 实战测试：

  - 1. 启动一个默认的Tomcat

  - 2. 没有webapps应用，官方默认webapps里面是没有东西的

  - 3. 我自己向webapps里面拷贝了webapps.disk里面的全部文件。

  - 4. 这个时候提交一下我们修改的内容打包成一个镜像嗷！！！

  - ```shell
    docker commit -a="tuzi"  -m="add webapps app" 7e119b82cff6 tomcat02:1.0
    ```

- 这个时候就从容器创建了一个镜像，docker images是可以看到你这里生成的镜像的嗷！！！我们以后就可以使用我们自己修改过的镜像。这个就相当于我们学习过的概念snapshot。

- 先理解概念，然后概念和实践相互集合一把拿下。以后项目开发就可以通过commit不断向已有的镜像中添加新的容器层再打包为新的镜像。





# Docker精通



## Docker数据卷

### 什么是容器数据卷

- 将应用和环境打包为一个镜像。
- 数据？如果数据都在容器中，我们删除容器数据就会丢失。**需求：数据持久化**。
- MySQL之类的数据在容器中是不安全的，需要数据共享，将容器中的数据可以导出来保存嗷！！！
- 容器之间可以有一个数据共享的技术！Docker容器中产生的数据同步到本地！！！
- 卷技术，本质上是目录的挂载，将容器内的目录挂载在Linux上。

![image-20210524095038844](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210524095046.png)

容器中的数据自动同步到Linux文件夹上。

- 总结一句话：
  - 容器的持久化和同步操作
  - 容器间也是可以数据共享的



### 使用数据卷

> 方式一：使用命令来挂载

- 命令格式：

`docker run -it -v 主机目录:容器内目录`

这个和上面学的-p也很类似哦，-p是端口映射，-v是目录映射。挂在完成之后，以后容器内目录中的内容都会自动和主机目录的内容同步嗷！！！

- 为了查看挂载是否成功，可以使用docker inspect 容器id来查看容器状态，里面有个属性说明了挂载的内容的状态：

![image-20210524095842507](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210524095842.png)

- 可以理解为双向绑定？？？？ --->   对，就是这么理解的
- 好处：
  - 修改只需要在本地修改即可，不需要进入容器中修改



### 实战：安装MySQL

- MySQL数据持久化的问题
- 注意：MySQL配置的时候需要账号和密码嗷！！！具体怎么设置的话，可以上官网去康康嗷！！！

![image-20210524102556491](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210524102603.png)

- 启动成功之后，可以通过你云服务器主机的ip和密码访问到远程的mysql，就和你本地的Database建立连接没啥区别。IDEA里面的Database就提供了这样的连接功能，VSCode也有嗷！！！
- 本地测试一个新的数据库，映射路径都是ok的嗷！！！
- 就算删掉MySQL容器的话，挂载到本地的数据卷中的数据还在哈！！！



### 具名挂载和匿名挂载

- docker volume:

```shell
[root@VM-12-10-centos ~]# docker volume --help

Usage:  docker volume COMMAND

Manage volumes

Commands:
  create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes

Run 'docker volume COMMAND --help' for more information on a command.

```

- 这个volume就是我们所说的卷 -v 的来源

- 查看所有卷的情况：
  - docker volume ls



#### 匿名挂载：

- -v的时候，只指定了指定容器里内目录，不指定容器外目录。
- `docker run -d -P --name nginx02 -v /etc/nginx nginx`





#### 具名挂载：

- -v的时候，没有给路径，只给了个名字

- `docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx`
- 卷名:容器内路径
- docker volume ls，是可以看到这个名字的相关信息的嗷！！！
- ![image-20210524104347307](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210524104347.png)



#### Summary：

- 所有docker内的卷，没有指定目录的情况下，都是在`/var/lib/docker/volumes/xxxx/_data`下的。
- 这个xxxx如果是匿名挂载就是一个id，如果是具名挂载就是你的命名。
- 具名挂载是最常用的，可以方便找到我们的一个卷，大多情况在使用具名挂载。
- 如何确定是具名挂载还是匿名挂载，还是指定路径挂载？
  - -v   容器内路径
  - -v   卷名:容器内路径
  - -v   宿主机路径:容器内路径
- 同时甚至可以设置权限：
  - ![image-20210524104914282](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210524104914.png)
  - 注意哈，权限是对于容器中的内容来说的，例如ro说明这个路径只能通过宿主机来操作，容器内是无法操作的嗷！！！



## 初识DockerFile

### 采用Dockerfile进行挂载：

- commit是通过容器生成镜像，如果我想自己从头生成一个镜像呢？？？
- DockerFile就是用来构造docker镜像的构建文件，命令脚本。镜像是一层一层的，脚本就是一个一个的命令。

```shell
FROM centos

VOLUMN ["volume01","volume02"]

CMD echo "......end......"
CMD /bin/bash
```

- 上面这里是个例子，第一行指定环境，第二行指定挂载的卷，就是其实就是匿名指定嗷！！！这里只说明了容器内的名字嘛！！！
- `docker build -f /home/tuzi/dockerfile1 -t tuzi/centos:1.0 .`
- 上面这里-f是指文件的位置，-t指的是target，也就是生成的目标位置，最后的.表示当前目录下。
- 注意这里的名字前面不能有/哈，dockerfile文件名字最好为Dockerfile。文件中的内容，指令大写嗷。
- 这里的每个命令是镜像的一层！！！
- 这种方式使用的非常多！！！一般都构建自己的镜像，如果没有的话就需要我们手动镜像挂载！！！



### 数据卷容器：

#### 多个MySQL数据同步：

![image-20210524150655723](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210524150702.png)

- 下面这个就是一个例子：

![image-20210524150900230](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210524150900.png)

- 甚至可以多个容器之间采用同样的方式进行挂载。只要通过这种方式就能够实现容器之间的数据共享。
- 删除了其中一个容器，其他的容器还使用着卷，这其他卷中的数据是不会丢失的嗷！！！
- 结论：容器之间，可以配置信息的传递。容器卷的生命周期一致持续到没有容器使用为止。一旦持久化到了本地，本地的数据就是持久化的，不会随着容器被删除完而消失嗷！！！



### Dockerfile介绍：

- 构建步骤：
  - 构建dockerfile 文件
  - docker build 构建一个镜像
  - docker run 运行镜像
  - docker push 发布镜像(DockerHub , 阿里云镜像仓库)
- 很多官方镜像都是基础包，很多功能没有，我们通常需要自己来配置我们的环境！！！

- 基础知识：
  - 每个保留关键字必须是大写字母
  - 执行从上往下执行
  - \# 表示注释
  - 每一个指令都会提交一个新的镜像层，并且提交。
- 整体结构：

![image-20210524152140558](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210524152140.png)

- 面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件，这个文件十分简单。镜像就是我们要最终发布和运行的产品。
- Docker容器就是镜像运行起来提供服务的嗷！！！





### Dockerfile指令说明：

![image-20210524152518215](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210524152518.png)

#### FROM

- 基础镜像，一切从这里开始

#### MAINTAINER

- 镜像是谁写的，姓名+邮箱

#### RUN

- 镜像构建的时候运行的命令

#### ADD

- 步骤：添加Tomcat镜像：
  - Centos有了,Tomcat呢？
  - 加入Tomcat包才能用哇！！！

#### WORKDIR

- 镜像的工作目录，指定工作的目录

#### VOLUME

- 挂载的目录

#### EXPOSE

- 暴露端口，端口配置

#### CMD

- 指定容器启动的时候要运行的命令

- 只有最后一个会生效，可被替代

#### ENTRYPOINT

- 指定容器启动的时候要运行的命令
- 可以追加命令

#### ONBUILD

- 构建一个被继承的时候，Dockerfile会运行ONBUILD指令，触发指令。

#### COPY

- 类似于add,将我们的文件拷贝到镜像中

#### ENV

- 构建的时候设置环境变量



### 实战测试：

- 非常基础的镜像：scratch , 连CentOS系统都是由它构建的嗷！！！Docker Hub中99%的镜像都是由这个镜像过来的。

> 创建一个自己的CentOS

- 由于原来的镜像中很多指令类似于pwd , vim都没有，我们创建一个有这些东西的镜像。

1. 编写配置文件：

```shell
[root@VM-12-10-centos dockerfile]# cat mydockerfile 
FROM centos
MAINTAINER tuzi<2632746476@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 3344

CMD echo $MYPATH
CMD echo "-----end-----"
CMD /bin/bash
```

2. 构建镜像：

```shell
[root@VM-12-10-centos dockerfile]# docker build -f mydockerfile -t mycentos:0.1 .
Sending build context to Docker daemon  2.048kB
Step 1/10 : FROM centos
 ---> 300e315adb2f
Step 2/10 : MAINTAINER tuzi<2632746476@qq.com>
 ---> Using cache
 ---> b7b887b8d1d9
Step 3/10 : ENV MYPATH /usr/local
 ---> Using cache
 ---> a93da34898df
Step 4/10 : WORKDIR $MYPATH
 ---> Using cache
 ---> aeb1cad8b76e
Step 5/10 : RUN yum -y install vim
 ---> Running in e77104a1f8bf
CentOS Linux 8 - AppStream                      1.5 MB/s | 6.3 MB     00:04    
CentOS Linux 8 - BaseOS                         419 kB/s | 2.3 MB     00:05    
CentOS Linux 8 - Extras                          20 kB/s | 9.6 kB     00:00    
Dependencies resolved.
================================================================================
 Package             Arch        Version                   Repository      Size
================================================================================
Installing:
 vim-enhanced        x86_64      2:8.0.1763-15.el8         appstream      1.4 M
Installing dependencies:
 gpm-libs            x86_64      1.20.7-15.el8             appstream       39 k
 vim-common          x86_64      2:8.0.1763-15.el8         appstream      6.3 M
 vim-filesystem      noarch      2:8.0.1763-15.el8         appstream       48 k
 which               x86_64      2.21-12.el8               baseos          49 k

Transaction Summary
================================================================================
Install  5 Packages

Total download size: 7.8 M
Installed size: 30 M
Downloading Packages:
(1/5): gpm-libs-1.20.7-15.el8.x86_64.rpm        680 kB/s |  39 kB     00:00    
(2/5): vim-filesystem-8.0.1763-15.el8.noarch.rp 360 kB/s |  48 kB     00:00    
(3/5): vim-enhanced-8.0.1763-15.el8.x86_64.rpm  1.3 MB/s | 1.4 MB     00:01    
(4/5): which-2.21-12.el8.x86_64.rpm              15 kB/s |  49 kB     00:03    
(5/5): vim-common-8.0.1763-15.el8.x86_64.rpm    1.3 MB/s | 6.3 MB     00:05    
--------------------------------------------------------------------------------
Total                                           1.3 MB/s | 7.8 MB     00:05     
warning: /var/cache/dnf/appstream-02e86d1c976ab532/packages/gpm-libs-1.20.7-15.el8.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID 8483c65d: NOKEY
CentOS Linux 8 - AppStream                      1.6 MB/s | 1.6 kB     00:00    
Importing GPG key 0x8483C65D:
 Userid     : "CentOS (CentOS Official Signing Key) <security@centos.org>"
 Fingerprint: 99DB 70FA E1D7 CE22 7FB6 4882 05B5 55B3 8483 C65D
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1 
  Installing       : which-2.21-12.el8.x86_64                               1/5 
  Installing       : vim-filesystem-2:8.0.1763-15.el8.noarch                2/5 
  Installing       : vim-common-2:8.0.1763-15.el8.x86_64                    3/5 
  Installing       : gpm-libs-1.20.7-15.el8.x86_64                          4/5 
  Running scriptlet: gpm-libs-1.20.7-15.el8.x86_64                          4/5 
  Installing       : vim-enhanced-2:8.0.1763-15.el8.x86_64                  5/5 
  Running scriptlet: vim-enhanced-2:8.0.1763-15.el8.x86_64                  5/5 
  Running scriptlet: vim-common-2:8.0.1763-15.el8.x86_64                    5/5 
  Verifying        : gpm-libs-1.20.7-15.el8.x86_64                          1/5 
  Verifying        : vim-common-2:8.0.1763-15.el8.x86_64                    2/5 
  Verifying        : vim-enhanced-2:8.0.1763-15.el8.x86_64                  3/5 
  Verifying        : vim-filesystem-2:8.0.1763-15.el8.noarch                4/5 
  Verifying        : which-2.21-12.el8.x86_64                               5/5 

Installed:
  gpm-libs-1.20.7-15.el8.x86_64         vim-common-2:8.0.1763-15.el8.x86_64    
  vim-enhanced-2:8.0.1763-15.el8.x86_64 vim-filesystem-2:8.0.1763-15.el8.noarch
  which-2.21-12.el8.x86_64             

Complete!
Removing intermediate container e77104a1f8bf
 ---> 77e4b03443f6
Step 6/10 : RUN yum -y install net-tools
 ---> Running in 9c36b34f901a
Last metadata expiration check: 0:00:12 ago on Mon May 24 07:44:53 2021.
Dependencies resolved.
================================================================================
 Package         Architecture Version                        Repository    Size
================================================================================
Installing:
 net-tools       x86_64       2.0-0.52.20160912git.el8       baseos       322 k

Transaction Summary
================================================================================
Install  1 Package

Total download size: 322 k
Installed size: 942 k
Downloading Packages:
net-tools-2.0-0.52.20160912git.el8.x86_64.rpm    56 kB/s | 322 kB     00:05    
--------------------------------------------------------------------------------
Total                                            46 kB/s | 322 kB     00:07     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1 
  Installing       : net-tools-2.0-0.52.20160912git.el8.x86_64              1/1 
  Running scriptlet: net-tools-2.0-0.52.20160912git.el8.x86_64              1/1 
  Verifying        : net-tools-2.0-0.52.20160912git.el8.x86_64              1/1 

Installed:
  net-tools-2.0-0.52.20160912git.el8.x86_64                                     

Complete!
Removing intermediate container 9c36b34f901a
 ---> 5aa957293560
Step 7/10 : EXPOSE 3344
 ---> Running in 5337e93b278e
Removing intermediate container 5337e93b278e
 ---> 24a3cf5703dd
Step 8/10 : CMD echo $MYPATH
 ---> Running in 0b0ee9826aed
Removing intermediate container 0b0ee9826aed
 ---> e990f552ebff
Step 9/10 : CMD echo "-----end-----"
 ---> Running in 06b75cec4a71
Removing intermediate container 06b75cec4a71
 ---> 095dbd389dcc
Step 10/10 : CMD /bin/bash
 ---> Running in 84b5d1acc86b
Removing intermediate container 84b5d1acc86b
 ---> 8eb39247efa4
Successfully built 8eb39247efa4
Successfully tagged mycentos:0.1
```

- 注意build最后有个点哈！！！有的它就直接使用，没有的它就去下载。

- ![image-20210524154755515](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210524154755.png)

- 验证过后发现，vim ,  ifconfig , pwd都是设置正确的嗷！

- 解锁新指令：docker history imageID：

  ```shell
  [root@VM-12-10-centos dockerfile]# docker history 8eb39247efa4
  IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
  8eb39247efa4   6 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "/bin…   0B        
  095dbd389dcc   6 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B        
  e990f552ebff   6 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B        
  24a3cf5703dd   6 minutes ago   /bin/sh -c #(nop)  EXPOSE 3344                  0B        
  5aa957293560   6 minutes ago   /bin/sh -c yum -y install net-tools             23.3MB    
  77e4b03443f6   6 minutes ago   /bin/sh -c yum -y install vim                   58MB      
  aeb1cad8b76e   7 minutes ago   /bin/sh -c #(nop) WORKDIR /usr/local            0B        
  a93da34898df   7 minutes ago   /bin/sh -c #(nop)  ENV MYPATH=/usr/local        0B        
  b7b887b8d1d9   7 minutes ago   /bin/sh -c #(nop)  MAINTAINER tuzi<263274647…   0B        
  300e315adb2f   5 months ago    /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
  <missing>      5 months ago    /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B        
  <missing>      5 months ago    /bin/sh -c #(nop) ADD file:bd7a2aed6ede423b7…   209MB     
  ```

  - 通过history可以看看镜像是怎么做出来的嗷！！！

## 



### CMD和ENTRYPOINT的区别：

- CMD可以执行多个的嗷：

`CMD ["ls","-a"]`

- 在你run的时候，只有最后一个cmd指令生效



- CMD中，如果你想用`docker run test -l`这种形式的话，-l会替换掉上面的ls -a，指令出错，无法拼接在指令后面orz。
- ENTRYPOINT则可以让它在我们编写的内容后面添加。

`ENTRYPOINT ["ls","-a"]`

- 这个时候如果你要和上面一样跑起来，这个指令则添加到我们的ENTRYPOINT后面，变为ls -al，这就是这两者的区别。



- DOCKERFILE中很多相似的命令，最好的方法就是去实践并且对比结果。



### 实例：Tomcat镜像

- 这个我没有实操嗷，因为我这儿没有Tomcat和JDK的压缩包，这儿就没有再去下载完成这个任务！！！

- 步骤：
  1. 准备镜像文件tomcat压缩包，jdk压缩包
  2. 编写dockerfile压缩包（官方命名为Dockerfile，如果是这个名字的话-f就可以不需要这个参数，会默认去寻找这个名字的文件来进行build）。
- 注意哈，在dockerfile里面使用add添加文件后，如果它的压缩包，docker会自动帮你解压嗷！！！
- 注意最后那个.哈，比如你要add导入一些文件，你可以先切到对应的文件目录之下，然后直接用文件名导入即可。这样做非常简单，最后的 . 其实就指定了在当前目录下去寻找文件。

```shell
FROM centos
MAINTAINER tuzi<2632746476@qq.com>

#复制本机的文件进入容器中
COPY readme.txt /usr/1oca1/readme.txt
ADD jdk-8u11-1inux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.22.tar.gz /usr/local/

#安装vim
RUN yum -y install vim

#设置docker镜像中的环境
#指定工作目录
ENV MYPATH /usr/1oca1
WORKDIR $MYPATH

#指定JAVA环境
ENV JAVA_HOME /usr/1oca1/jdk1.8.0.11
ENV CLASSPATH $JAVA_HOME/1ib/dt. jar:$JAVA_HOME/lib/too1s.jar
#指定Tomcat环境
ENV CATALINA_HOME /usr/1oca1/apache- tomcat-9.0.22
ENV CATALINA_BASH /usr/1oca1/apache-tomcat-9.0.22
#指定默认路径，和windows里面一样的，这样方便在命令行中直接使用命令！！！
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
#暴露端口
EXPOSE 8080
#启动一些文件来进行操作，例如日志的开启啊
CMD /usr/1oca1/apache-tomcat-9.0.22/bin/startup.sh && tail -F /ur1/1oca1/apache-tomcat-9.0.22/bin/1ogs/catalina.out
```

3. 创建镜像：

`docker build -t diytomcat .`

- dockerfile文件的名字为标准的Dockerfile的话，-f就不用自己设置了，系统会导入的嗷！！！
- docker run的时候如果没有指定版本的话，用的就是默认版本，也就是最新版，因此这里要注意一下嗷！！！

4. 启动镜像：

进行一些挂载，例如发布的时候的目录挂载到Linux虚拟机中嗷！！！

![image-20210524191902624](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210524191909.png)

- 例如上面这个，将这个容器的webapps挂载到了本地的test目录，将日志目录也挂载到了本地。这里我们。

5. 测试访问

6. 发布项目（由于我们做了挂载，可以直接对于本地的文件进行修改就可以实现发布）

例如webapps/test挂载到了本地的目录，那么我们就可以去test里面新建WEB-INF啊，新建web.xml啊，新建index.jsp啊之类的，都可以新建的。然后访问对应端口



- 以后的项目都是这个流程嗷！！！





## 发布镜像：



### 发布到docker hub：

- 先去DockerHub的注册一个账号

- 服务器上提交自己的镜像

- `docker push 账户名(liututu)/镜像名(diytomcat:1.0)`





#### docker login：

- 登录远程的dockerhub
- 登录完毕之后就可以提交镜像了。
- 登录之后就可以docker push了。
- `docker login -u liututu`
- 然后输入密码即可以登录



#### docker tag：

- 用于添加标签
- `docker tag 镜像id tag的名字和版本号`
- e.g. `docker tag xxx tuzi/tomcat:1.0`



### 发布到腾讯云：

- 有点麻烦，暂不考虑



## Docker全流程小结：

- 流程图如下：

![image-20210525164716547](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525164723.png)



### docker save:

- 可以使用--help去看使用方法很简单的嗷
- -o 参数，用于指定输出路径



### docker load：

- 可以使用--help去看使用方法很简单的嗷
- \- i参数，用于指定输入读取的路径



# Docker网络

## 理解网络

### ip addr:

- docker中的网络是docker0
- docker中先清空，比较方便，rmi and rm

![image-20210525165958822](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525165959.png)

- 三个网络，有点乱嗷！！！

- 查看容器内部网络地址，就是上面那幅图：ip addr。
- `docker exec -it tomcat01 ip addr`可以使用命令连接直接查看ip addr。
- 下面这幅图你可以看到，Linux宿主机是可以Ping通docker容器的。

![image-20210525170515929](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525170516.png)

> 原理：
>
> 1. 每启动一个docker容器，docker就会为容器分配一个ip。
> 2. 只要安装了docker，就会有一个docker网卡。这个网卡使用的是evth-pair桥接模式。
> 3. 多启动了一个容器之后，在宿主机内使用ip addr可以看到多了一个网卡！！！
> 4. 容器带来的网卡是一对儿的，容器里面有一个，宿主机外面有一个，这两个网卡是相互绑定可以通信的。这样一对儿一对儿网卡存在的技术就叫做evth-pair技术。一端连着协议，一端彼此相连，他们之间就可以通信。
> 5. 因此我们经常使用evth-pair来充当一个桥梁，连接各种网络设备的。
> 6. OpenStac , Docker容器之间的连接，OVS的连接，都是使用 evth-pair技术。

![image-20210525171852546](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525171852.png)

- 容器和容器之间也是可以相互ping通的嗷！！！
- 网络通信流程图：

![image-20210525172305633](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525172510.png)

结论：Tomcat01和Tomcat02共用一个路由器 ， docker0

- 在所有容器不指定网络的情况下，默认使用的都是docker0，docker会给我们容器分配一个默认的可用ip



> 小结:
>
> ​	Docker使用的Linux中的桥接模式嗷！！！
>
> 概念图，宿主机中式一个Docker容器的网桥：
>
> ![image-20210525172743908](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525172743.png)

- Docker中的所有的网络接口都是虚拟的，虚拟的转发效率高！！！
- 只要容器删除了，容器的一堆网卡就都没了。
- 思考场景

> 思考一个场景，我们编写了一个微服务，database url = ip; 项目不重启，数据库ip换掉了，我们希望可以处理这个问题，可以使用名字来访问容器。



## docker link：

- 用名字取ping ， 容器之间ping不通：

![image-20210525173214861](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525173214.png)

- 解决问题的样式：

![image-20210525173351915](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525173352.png)

- 3 ping 2可以ping通，2 ping 3通不了，因为你只对于3做了配置

### docker  network ls:

![image-20210525173746192](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525173746.png)



### docker network inspect 容器id:

- 查看容器的网络连接情况。

![image-20210525174735252](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525174735.png)

![image-20210525174826630](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525174826.png)

- 其实tomcat03是配置了tomcat02

看看配置：

![image-20210525175127130](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525175127.png)

- 这里进入了tomcat03中的host配置文件，查看到了tomcat03关于tomcat02的网络配置。
- 原理：
  - 在启动容器的时候，对于tomcat02进行了一个绑定，其实就算不绑定，用容器id也能够ping通。
- 未进行配置的tomcat02配置文件中的内容：

![image-20210525185042340](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210525185042.png)

- 真实玩docker的时候，使用自定义的网络，就很方便嗷！！！不适用docker0嗷！！！

- docker0的问题：
  - 不支持容器名访问嗷！！！
- 自定义yyds！！！



## 自定义网络

- 容器互联作用

### 移除网络：

- docker network ls可以显示所有的网络
- docker network rm 网络id ， 可以移除一个或多个网络
- docker network rm $(docker network ls -q)这样就可以递归删除所有网络嗷！！！



### 网络模式：

- bridge：在docker上面搭桥。（默认，自己创建也是使用桥接模式的嗷！！！）
- none：不配置网络
- host：和宿主机共享网络
- container：容器内网络联通（用的少，局限很大）

```shell
[root@VM-12-10-centos ~]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
96547b049a6f   bridge    bridge    local
cbf21525bdaa   host      host      local
1f4bf7dc20df   none      null      local
```



- 上面这三个是docker为我们默认配置好的三个嗷！！！

### 使用create创建网络：

- 我们直接启动镜像的命令，实际上是默认有个：`--net bridge`参数的，这里实际上就指定了使用docker默认为我们提供的docker0桥接模式也就是我们上面这一段代码中的第一个也就是名字为bridge的这个网络。
- docker0默认名字不能访问 ，可以使用link进行打通，但是比较麻烦嗷！！！
- 可以调用帮助文档：`docker network create --help`
- 创建一个网络：
  - `docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet `
  - 上面这个相当于新建了一个router
- 我们创建了网络之后进行查看：

```shell
[root@VM-12-10-centos ~]# docker inspect mynet
[
    {
        "Name": "mynet",
        "Id": "4fdb6ebd5847ecda8ef9aa755cb1fe24a0c792f089492aaa273e528517c56a45",
        "Created": "2021-05-25T21:49:13.115937992+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

- 这个时候启动容器的时候，就可以加上参数：`--net mynet`，这样就完成了`router`的指定。
- 当创建了新的容器之后，我们再执行这个inspect mynet的命令，就可以看到我们上面这一坨里面的多了容器！！！为了验证容器间可以进行交互，我们这里创建了tomcat01和tomcat02，作为示例：

```shell
[root@VM-12-10-centos ~]# docker inspect mynet
[
    {
        "Name": "mynet",
        "Id": "4fdb6ebd5847ecda8ef9aa755cb1fe24a0c792f089492aaa273e528517c56a45",
        "Created": "2021-05-25T21:49:13.115937992+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "0940afc5e87c21a391d076dbc2929f4ff6e2b86e753c9f608deeb4de32b92421": {
                "Name": "tomcat01",
                "EndpointID": "0be1b6042a092ae4fdbd744559a781cccd865b6836c30c3cb25b92359d500214",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            },
            "def80af639d3f697e76de23f50fcdf209612fb70996289b1b812b07f547f55f6": {
                "Name": "tomcat02",
                "EndpointID": "c827e50e6ec0c3c9692b3c4090ac9a9b38e38eb1b2cc70fe5baa7ef40856d14a",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

- 上面就可以看到我们的两个容器嗷！！！自创建的网络非常完善，带了很多的功能嗷！！！不管是ping id还是ping name都能够ping通嗷！！！

```shell
[root@VM-12-10-centos ~]# docker exec -it tomcat01 ping 192.168.0.3
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_seq=1 ttl=64 time=0.060 ms
64 bytes from 192.168.0.3: icmp_seq=2 ttl=64 time=0.049 ms
64 bytes from 192.168.0.3: icmp_seq=3 ttl=64 time=0.058 ms
64 bytes from 192.168.0.3: icmp_seq=4 ttl=64 time=0.064 ms
^C
--- 192.168.0.3 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.049/0.057/0.064/0.010 ms


[root@VM-12-10-centos ~]# docker exec -it tomcat01 ping tomcat02
PING tomcat02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=1 ttl=64 time=0.059 ms
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=2 ttl=64 time=0.061 ms
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=3 ttl=64 time=0.050 ms
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=4 ttl=64 time=0.059 ms
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=5 ttl=64 time=0.063 ms
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=6 ttl=64 time=0.055 ms
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=7 ttl=64 time=0.066 ms
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=8 ttl=64 time=0.063 ms
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=9 ttl=64 time=0.053 ms
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=10 ttl=64 time=0.049 ms
^C
--- tomcat02 ping statistics ---
10 packets transmitted, 10 received, 0% packet loss, time 9ms
rtt min/avg/max/mdev = 0.049/0.057/0.066/0.011 ms
```

- 不适用--link也可以ping名字，我们自定义的网络，docker都已经帮我们维护好了对应的关系。



### 网络联通：

- 不同的网段是ping不通的嗷！！！

![image-20210526115321157](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526115758.png)

![image-20210526115347834](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526115806.png)

- `docker network connect NETWORK CONTAINER`
- 上面用于不同的网段之间的连接，联通就是将tomcat容器放到了mynet网络下。官方叫做：一个容器，两个ip地址。（一个公网ip , 一个私网ip）。

![image-20210526120923916](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526120924.png)

- 网卡和网卡之间正常是不能打通的，但是容器和网卡之间可以嗷！！！
- 只有01和mynet打通了，可以和它交互，但是02不可以嗷！！！！



## 实战（部署redis集群）：

- 分片+高可用+负载均衡

![image-20210526121252216](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210526121252.png)

- [这一块儿可以后面再看嗷！！！](https://www.bilibili.com/video/BV1og4y1q7M4?p=38&spm_id_from=pageDriver)
- 目前还没学redis，以及集群还没学，这个狂神的课程还有进阶版本的docker，目前这个教程就到这里嗷！！！





































# 小技巧

## Docker开机后跑不了的问题

- 由于docker默认没有开启开机自动重启，这个时候你的虚拟机重新开机就会导致docker用不了，如何解决？那就再开启一次docker服务就好了哇！！！
- `systemctl restart docker.service`

## Docker映射端口在服务器打开

- 此外，比如你想开放3344端口，你需要去腾讯云服务器配置的那个位置配置新的安全组：

![image-20210523113152632](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/20210523113152.png)

- 配置完成之后可能需要重启实例才能使得服务生效嗷！！！



## 博客的nginx重启之后记得开

- `systemctl start nginx.service`
- [Nginx服务器解决方案](https://zhuanlan.zhihu.com/p/56349043)



## 远程访问的问题：

- 比如我开放了3344端口，直接通过域名:3344是能够访问到我监听3344端口的程序的。
- 突然让我想起了blog的搭建。我搭建blog的时候，使用nginx监听的是80端口。浏览器访问我的域名的话，默认访问的就是80端口，因此我的nginx能够为其提供服务。
- 域名:端口号就可以根据你自己的喜好，访问到部署在不同端口的应用程序。



## /bin/bash

- 一般和-it这个选项用来一起使用的嗷，这里是指定交互的终端为bash，不然你怎么和内部容器进行交互嘛！！！
- 