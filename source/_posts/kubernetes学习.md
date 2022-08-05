---
title: Kubernetes学习
date: 2021-08-26 15:23:44
tags: 后端
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826174644.png
---

# 终于来到了K8S这个部分相应知识的讲解嗷！！！



- 课程介绍：

![image-20210826152922417](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826152922.png)



# K8S基本概念：

## K8S概述和特性

- 概述：
  - 容器化集群管理系统，是Google开源的一个容器编排引擎，支持自动化的部署。
  - 使用k8s进行容器化应用部署
  - 使用k8s利于应用拓展
  - 目标：让容器化应用更加简洁和高效



- 特性：
  - 自动装箱：
    - 基于容器对应用运行环境的资源配置要求自动部署应用容器。
  - 自我修复：
    - 容器失败会自动重启
    - 部署的结点有问题的时候，会自动重新部署和重新调度
    - 未通过监控检查的时候，会关闭此容器。直到正常运行，才会对外提供服务。
  - 水平拓展：
    - 流量来了之后动态拓展
  - 服务发现：
    - 对外提供统一入口
    - 对内负载均衡，动态分配嗷！！！
  - 滚动更新：
    - 根据应用的变化，对于应用容器运行的应用，进行一次性或者批量式的更新。
  - 版本回退：
    - 可以根据应用部署情况，对于应用容器进行的应用，进行历史版本的即时回退。
  - 密钥和配置管理：
    - 类似于热部署
  - 自动编排：
    - 自动实现存储系统的挂载以及应用，对于数据持久化非常重要。
  - 批处理：
    - 一次性任务，定时任务，满足批量数据处理和分析的场景嗷！！！





## K8S架构组件

![image-20210826155221671](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826155416.png)



1. Master和Worker：

- 和Nginx里面那个类似哦，Master是主控结点，Worker是工作结点。
- Master组件：
  - API Server：集群统一入口，以restful方式，交给etcd存储
  - Scheduler：选择Worker Node结点应用部署
  - Controller-Manager：处理集群中常规后台任务，一个资源对应一个控制器
  - etcd：存储系统，保存集群相关数据
- Worker组件：
  - kubelet: Master派到Worker节点的代表，管理工作节点容器
  - kube-proxy: 提供网络代理，实现负载均衡等功能。





## K8S核心概念

### Pod

- 最小部署单元

> - 一个Pod中可以有很多个容器，它是一组单元的集合
> - 最小单元不是一个容器，而是一个Pod嗷！
> - 一个Pod中的容器是共享网络的
> - 生命周期是短暂的

### Controller

- 确保预期的pod副本的数量

> - 无状态应用部署
>   - 没有任何约定
>   - 直接拿容器来使用
> - 有状态应用部署
>   - 有一定的条件
>   - 依赖于存储或需要特定网络IP

- 确保所有的node都运行同一个Pod，一次性任务和定时任务

### Service

- 定义一组pod的访问规则
- Service统一部署，Controller来创建pod，pod是最小部署单元。



# 搭建K8S集群：

## K8S环境平台规划

- 单Master集群：

![image-20210826165740642](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826165740.png)



- 多Master集群：

![image-20210826170053372](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826170053.png)



## 服务器硬件配置要求

master：2核 , 4G , 20G

worker：4核 , 8G , 40G

生产环境：要求更高嗷！！！

## 搭建K8S集群部署方式

1. kubeadm

> K8S部署工具，提供kubeadm init和kubeadm join，用于快速部署K8S集群
>
> 非常简单嗷！！！有点像Swarm嗷！！！

2. 二进制包

> 手动部署每个组件，组成K8S集群嗷！！！
>
> 可以清晰看到每一步是怎么做到的嗷！！！

## 实操：

### Kubeadm集群搭建：

- 基本要求：

![image-20210826173026783](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826173042.png)

1. 安装三台虚拟机，安装CentOS7。

下面的操作可以发送到所有会话，直接在所有会话中操作嗷！！！

![image-20210826173437046](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826173437.png)

![image-20210826173501669](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826173502.png)

![image-20210826173657885](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826173658.png)

2. 所有结点安装Docker / Kubeadm / Kubelet

- docker：

![image-20210826173911308](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826173911.png)

- 阿里云yum软件源：

![image-20210826174031806](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826174032.png)

- 安装对应的东西：

![image-20210826174157323](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826174157.png)



- Master部署：

![image-20210826174237619](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826174237.png)

- 加入Node：

![image-20210826174933351](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826174933.png)

![image-20210826175034332](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826175034.png)

- 上面那一段是初始化主结点！！！
- 下面这一段是在Worker中运行，将Worker添加注册到主节点中用到的嗷！！！
- 后面还有网络配置等一大堆的，建议看视频加课件嗷！！！

![image-20210827085447881](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827085448.png)



### 二进制集群搭建：

[二进制集群搭建相关知识嗷！！！](https://www.bilibili.com/video/BV1GT4y1A756?p=10&spm_id_from=pageDriver)

- 基本要求：

![image-20210826173026783](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210826173042.png)

1. 创建多台虚拟机，安装Linux操作系统
2. 操作系统初始化
3. 为etcd和apiserver自签证书



- 自签证书：
  - ![image-20210827090245778](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827090246.png)



自签证书准备：

- cfssl证书生成工具

![image-20210827090344670](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827090344.png)



4. 部署etcd集群



5.  部署master组件

![image-20210827090521462](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827090530.png)

6. 部署Node组件

![image-20210827090522956](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827090530.png)



7. 部署集群网络

- 老师讲的挺糊的，二进制这里先跳过去，后面再进行补充嗷！！！



# Kubectl：

- 集群管理的命令行工具嗷！！！
- 语法格式：`kubectl [command] [type] [name] [flags]`

![image-20210827095917745](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827095917.png)

- `kubectl + --help`可以查看所有的命令嗷！！！

![image-20210827102816349](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827102816.png)

![image-20210827102843999](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827102844.png)

![image-20210827102908370](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827102908.png)

![image-20210827102922364](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827102922.png)



## 目前用到的命令：

1. `kubectl create deployment nginx --image=nginx`
2. `kubectl expose deployment nginx --port=80 --type=NodePort`
3. `kubectl get port,svc`

port是段都的信息，svc里面就包括了上面指定的开放端口的映射信息嗷！！！

4. `kubectl get nodes`

获取所有结点的信息嗷！

5. `kubectl get cs`

显示基本的状态

6. `kubectl apply -f + yaml文件`

刚刚用于部署网络嗷！！！

# 资源编排：

- 实际开发的过程中，会把相应的配置记录在配置文件yaml中。叫做资源清单文件或资源编排文件。
- Yaml文件：

![image-20210827103832789](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827103832.png)

- Yaml文件详解：

![image-20210827104018767](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827104019.png)

Yaml文件的组成部分：

- 控制器定义
- 被控制对象

常用字段含义：

- ![image-20210827104619431](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827104619.png)
- 如何快速编写yaml文件：
  1. 使用kubectl create生成yaml文件
  2. 使用kubectl get命令导出yaml文件

1. kubectl create：

![image-20210827174309484](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827174309.png)

直接输出到yaml文件中：

![image-20210827174351326](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827174351.png)

2. kubectl get：

- 适用于已经部署好的项目，将配置文件导出出来嗷！！！

![image-20210827174620867](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827174621.png)

系版本的--export已经弃用了嗷！！！

`-o yaml > 1.yaml`就可以，--export可以不用了嗷！！！



# Pod：

## 概述和存在意义：

- 基本概念：
  - K8s中创建和管理的最小单元，部署的最小单元
  - K8s不会直接处理容器，而是直接处理Pod。
  - Pod由一个或者多个container构成嗷！！！
  - 一个Pod容器共享网络
  - Pod是短暂存在的嗷！

![image-20210827185620421](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827185620.png)

![image-20210827185641059](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827185641.png)

- 存在意义：

1. 创建容器使用Docker，一个docker对应一个容器，一个容器有进程，一个容器运行一个应用程序。
2. Pod是多进程设计，运行多个应用程序。

- 一个pod有多个容器，一个容器里面运行一个应用程序。

3. Pod存在为了亲密性应用

- 两个应用之间要进行交互
- 网络之间的调用
- 两个应用需要频繁进行调用





## 实现机制：

1. 共享网络
2. 共享存储

- 容器本身之间是相互隔离的嗷！！！通过Linux中的Namespace和group进行隔离的嗷！！！
- 共享网络的前提条件：容器在同一个Namespace下面嗷！！！
- Pod实现共享网络的机制：

1. 创建一个默认的根容器（Pause容器），又被称为info容器
2. 后面创建的docker容器会逐个加入到info容器中嗷！！！

![image-20210827194459456](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827194459.png)

- 通过Pause容器，把其他业务容器加入到Pause容器中，让所有容器业务在同一个名称空间中，可以实现网络共享。

![image-20210827194829852](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827194830.png)

- 共享存储：

  - 数据持久化：
    - 日志数据
    - 业务数据
  - ![image-20210827200341170](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827200341.png)

  Volume数据卷的概念，使用数据卷进行持久化存储。

  - ![image-20210827202855285](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827202855.png)





## 镜像拉取策略：

![image-20210827203258176](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827203258.png)

## 资源限制：

![image-20210827204211428](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827204211.png)

![image-20210827204320259](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827204320.png)

![image-20210827204506927](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827204507.png)



## Pod重启机制：

- ![image-20210827204648137](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827204648.png)
- ![image-20210827204824567](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827204824.png)

批量任务就可以用Never等，有不同的用法嗷！！！



## 健康检查：

- 通过容器检查 ---> 应用层健康检查

![image-20210827205152089](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827205152.png)

- 健康检查的策略：

![image-20210827205301480](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827205301.png)

![image-20210827205354611](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827205354.png)

## 调度策略：

- 时序图：

![image-20210827210104034](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827210104.png)

- 每一步都要经过API Server，每一步的数据都写入到etcd，通过监控的方式，完成一步接一步的执行嗷！！！

- 影响调用的属性：

  1. Pod资源限制对于Pod调用产生影响

  ![image-20210827211222380](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827211222.png)

  2. 结点选择器标签影响Pod调度

  ![image-20210827211310247](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827211310.png)

  ![image-20210827211417955](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827211418.png)

  要采用上面这种措施，首先要对于结点创建标签`kubectl label node node1 env_role=dev`

  ![image-20210827211718172](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827211718.png)

  3. 结点的亲和性影响Pod调度

  nodeAffinity和之前nodeSelector基本一样的，根据节点上标签约束来决定将Pod调度到某些节点上嗷！！！

  ![image-20210827212055187](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827212055.png)

  支持的常用的操作符号：

  - In NotIn Exists Gt Lt DoesNotExists
  - 反亲和性：和亲和性刚好相反嗷！！！

  4. 污点和污点容忍：

  - nodeSelector和nodeAffinity：Pod调度到某些节点上，Pod属性，调度的时候实现
  - Taint污点：节点不做普通分配调度，是节点属性嗷！
  - 应用场景：专用节点，配置特点硬件节点，基于Taint驱逐

  ![image-20210827214725188](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210827214725.png)

  这里的Key的名字没有特殊的要求嗷，可以随便取嗷！！！
  
  - 删除污点：
  
  ![image-20210829091432792](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829091637.png)
  
  - 污点容忍：
  
  ![image-20210829091655646](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829091655.png)
  
  容忍污点，使得某个节点即使有污点也有可能被调度嗷！！！

# Controller：

## 什么是Controller

- 实际存在的，管理和运行实际的对象。
- Controller是集群上管理和运行容器的对象

## Pod和Controller之间的关系

- Pod是通过Controller来实际上实现应用的运维的，比如伸缩，滚动升级等等。
- Pod和Controller之间通过Label建立关系嗷！！！
- 控制器也可被称为工作负载（WorkLoad）嗷。
- 两者之间的运维管理：

![image-20210829092242059](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829092242.png)

Selector嗷！！！

![image-20210829102134436](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829102134.png)

## Deployment控制器应用场景

- 部署无状态应用嗷！！！
- 管理Pod和RepliccaSet
- 部署，滚动升级等功能
- 应用场景：
  - Web服务
  - 微服务

## yaml文件字段说明

- 命令行不可复现 -> 命令行变为Yaml
- ![image-20210829102200400](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829102200.png)
- 使用yaml文件进行部署：

![image-20210829102240319](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829102240.png)



## Deployment控制器部署应用

- 步骤：

  1. 导出Yaml文件：

  ![image-20210829102340153](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829102340.png)

  2. 使用Yaml进行部署：

  ![image-20210829102405236](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829102405.png)

  3. 对外暴露端口（发布）：

  ![image-20210829102530630](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829102530.png)

  ![image-20210829102611292](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829102611.png)

  本质上这里还是通过Yaml来部署的，熟悉了之后，可以直接编写Yaml中的参数，对其进行操作和部署嗷！！！执行第二个步骤，就可以实现我们的部署工作了嗷！！！

  ![image-20210829102841918](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829102842.png)

  80是容器内暴露的端口，30048是所有的结点，对外暴露的端口。就是相当于Docker中的端口映射的概念嘛！！！

## 升级回滚

- 指定版本的配置：
- ![image-20210829103956299](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829103956.png)

![image-20210829103902899](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829103903.png)

- 如果我要升级Nginx的版本呢？

![image-20210829104149578](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829104149.png)

- 升级的原理和过程：

![image-20210829104332318](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829104332.png)

1.14正常运行，同时去下载1.15，下载完成并且成功运行之后，再去把1.14给替换掉。这就是升级的整个流程嗷！！！保证升级过程中，五福不会终端嗷！！！

- 查看升级是否成功：

![image-20210829104511926](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829104512.png)

- 回滚版本如何操作呢？

1. 查看历史版本：

![image-20210829104605072](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829104605.png)

第一个是1.14，第二个就是1.15

2. 还原（回滚）版本：

- 回滚到上一个版本：

![image-20210829104712466](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829104712.png)

- 回滚到指定版本：

![image-20210829104830670](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829104831.png)

## 弹性伸缩

![image-20210829104919152](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829104919.png)





# Service：

## 概述：

- 定义一组Pod的访问规则嗷！！！

1. Service存在的意义：

- 防止Pod失联（服务发现）

![image-20210829105124253](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829105124.png)

Pod每次重启，IP都会变化的，所以Pod的生命周期是短暂的嗷！！！因此虽然通过Ip可以相互调用，由于Ip不可靠，不予以采用嗷！！！

- 说白了就是Nacos！！！服务注册中心嘛！！！

2. 定义一组Pod的访问策略：

- 负载均衡！！！

![image-20210829105541773](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829105542.png)



## Pod和Service的关系：

- 如何建立关系？

![image-20210829105810639](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829105811.png)



## Service的三种类型：

- 类型：
  1. ClusterIP：集群内部访问
  2. NodePort：对外访问应用使用
  3. LoadBalancer：对外访问应用使用，公有云也可以使用嗷！

- `kubectl get svc`：svc是对外暴露端口，这里就是查询每个端口的暴露情况orz。

![image-20210829110142408](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829110142.png)

![image-20210829110247676](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829110248.png)

- 跑起来之后的结果：

![image-20210829110332924](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829110333.png)

注意字母的大小写哈！！！、

- Node一般内网部署应用，外网一般不能够访问到。

![image-20210829111120898](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829111121.png)



# Controller补充：

## 部署无状态应用：

- 无状态特点：
  - 其实认为Pod都是一样的，因为没有状态嘛
  - 没有顺序要求嗷！！！
  - 不用考虑我在哪个Node上运行
  - 随意进行伸缩和拓展
- 有状态：
  - 上面因素都需要考虑到
  - 让每个Pod都是独立的，保持Pod的启动顺序和唯一性
  - 唯一的标识符进行区分，持久存储
  - 有序的，比如Mysql主从等



## 部署有状态应用：

- 无头service

![image-20210829112954170](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829112954.png)

- ClusterIP：None

![image-20210829113123442](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829113123.png)

下面这个Kind，里面是StatefulSet嗷，注意一下嗷！

- ![image-20210829113024879](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829113025.png)
- 查看Pod：

![image-20210829113538783](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829113539.png)

每个都是唯一的名称嗷！！！

![image-20210829113626795](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829113627.png)

None就代表这是一个无头的服务嗷！！！

- summary：

![image-20210829113841918](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829113842.png)

![image-20210829113857427](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829113858.png)



## 所有Node运行同一个Pod：

- 部署守护进程DaemonSet

每个node上面运行一个pod，新加入的node也同样运行在一个pod里面嗷！！！

例子：

![image-20210830164403320](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830164410.png)

Yaml文件：

![image-20210830164928922](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830164929.png)



## Job（一次性任务）：

![image-20210830165205933](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830165206.png)

这里的Kind为Job嗷，是Job才为一次性任务。

![image-20210830165641957](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830165642.png)



## CronJob（定时任务）：

![image-20210830165735467](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830165735.png)

- 每隔一分钟重启一次嗷！！！
- 启动定时任务：`kubectl apply -f cronjob.yaml`

![image-20210830170012503](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830170012.png)

# 配置管理：

## Secret

- 作用：加密数据加载到etcd里面，让Pod容器以挂载Volume方式进行访问。

- 场景：凭证

- 场景：

  1. 创建secret加密数据（以Base64编码为例子）：

  ![image-20210830170303052](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830170303.png)

  2. 挂载数据卷：

     - 以变量形式挂载：

     ![image-20210830170523737](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830170523.png)

     ![image-20210830170623197](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830170623.png)

     ![image-20210830170754577](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830170754.png)

     - 以Volume的形式挂载到Pod中：

     ![image-20210830171206721](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210830171206.png)



## ConfigMap：

- 作用：存储不加密的数据到etcd中，让Pod以变量或者Volume的形式挂载到容器中。

- 场景：

  - 配置文件

- 步骤：

  1. 创建配置文件：

  



















