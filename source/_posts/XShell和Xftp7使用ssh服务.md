---
title: XShell和Xftp7使用ssh服务
date: 2021-03-22 10:33:28
tags: [云服务器与项目部署,计算机网络]
---











# 这是关于XShell和Xftp使用的教学，用ssh连接的例子是我的VMWare中的Ubuntu虚拟机。其实在阿里云上连接是一样的！！！



<!--more-->



# 工具使用：

- Xshell或者Xftp
- ![image-20210312212642320](https://i.loli.net/2021/03/12/hBIwvURmtXqunxT.png)

- 下载与安装：
  1. 首先进入XSHELL官网，选中XSHELL点击Download
  2. ![image-20210312213049546](C:/Users/Alexander Liu/AppData/Roaming/Typora/typora-user-images/image-20210312213049546.png)
  3. 选中Free License，点击底下那个Free Licensing Page，然后就可以申请免费使用的
  4. ![image-20210312213355178](C:/Users/Alexander Liu/AppData/Roaming/Typora/typora-user-images/image-20210312213355178.png)
  5. 填入用户名和email，就可以点击both，会发送相应的下载链接到你的邮箱，点开就可以进行对应软件的下载与使用。

# 配置要点:

- 无论是Xftp还是Xshell本质上都是建立本机与虚拟机的通讯，既然是通讯，那就要保证你的虚拟机是开！机！状！态！
- 这两者的目的主要是可以保证本机与虚拟机文件的共享，我个人觉得Xftp更加好用嗷！！！



## 网络配置：

- 进行网络配置，首先你要在linux中有相应的相关软件：[Linux使用ssh链接相关的教程](![img](file:///C:\Users\Alexander Liu\AppData\Roaming\Tencent\QQTempSys\%W@GJ$ACOF(TYDYECOKVDYB.png)https://blog.csdn.net/meihuasheng/article/details/98473918?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=1328627.8341.16153536665230129&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)
- ![image-20210312215037129](C:/Users/Alexander Liu/AppData/Roaming/Typora/typora-user-images/image-20210312215037129.png)
- 网络连接设置为桥接模式，注意！！！这个选中了就不要随便改了，每次改的话IP地址都不一样，意味着要在Xftp和Xshell中要反复重连！！！
- 获得本机的ip地址，在linux命令行中输入ifconfig -a就可以找到对应的ip
- ![image-20210312215414778](https://i.loli.net/2021/03/12/ryvBVR29lqGnTPA.png)
- 名称可以随便取一个，主机填上刚刚你的ip地址即可，端口号不用动，勾选使用身份验证代理，输入你linux中的用户名和密码，即可以实现快速连接。连接成功可以看到两台电脑的所有文件，连接非常方便！！！
- Xshell中登陆可以直接输入`ssh + ip地址`，然后在弹出来的页面中填入Linux机的用户名和密码，也可以实现连接。
- 配置好了之后，理论上以后可以直接选择之前配置好的那些行，然后再虚拟机保证开机的情况下，进行选择连接！！！
- VSCode也可以连接Linux，将VScode的终端变为Linux的终端，非常方便，以后再配置。