---
title: vue-cli脚手架使用
date: 2021-03-24 21:16:26
tags:
	前端
---



# 关于vue-cli脚手架的使用



<!--more-->

# 安装的坑多的很

[github安装教程](https://github.com/oinsd/Vue.js-Learning-Example/blob/master/NO.12_vue-cli%E5%AE%89%E8%A3%85%EF%BC%88%E5%86%85%E5%90%AB%E6%96%87%E5%AD%97%E7%89%88%EF%BC%89/vue-cli%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E5%8D%B8%E8%BD%BD.pdf)

下面给出一些安装注意的重要的地方

- npm的安装：
  - 全局目录设置（下载的内容所保存的位置）

- nodejs的下载：
  - 安装的目录放到除c盘外的别的盘
  - 创建node_cache和node_global，设置全局变量
- cnpm的下载
- Vue-CLI的下载
  - [官网下载安装指导，最新版的Vue-CLI安装](https://cli.vuejs.org/zh/guide/installation.html)
- 指令可以用cnpm来代替npm嗷，cnpm是国内的镜像源会快一点，但我个人使用npm的时候感觉也没啥区别！！！~~可能是因为我科学上网？？？~~



# 新建脚手架

- `vue create hello-world`，按照选项选
- `npm run serve`会打包来启动服务，可以用于本地调试
- node_modules非常大嗷！！！计算机文件多，计算机处理起来很费劲啊，几万个文件很难找。





# 结构分析

- 单一文件组件：
  - script
  - style
  - template
- node_modules包含了我们需要的很多依赖文件
- package.json显示了很多文件的版本和配置
- README.md主要介绍了一些对应的模式和模式对应的启动方法
- 教程`P5`有个坑嗷！！！
- GitHub上的文件都没有node_modules，具体怎么跑起来去康康readme.md，里面会有相应的介绍嗷！！！





# 其他注意事项

- 很多默认文件，比如`index.html`不要去修改
- main.js是Vue项目的入口，负责把相应的组件解析完之后挂载到index.html中去。
- App.vue是单文件组件的根结点，它就是最底下的根！！！
- 默认的目录是在src下，`import HW from "./components/HelloWorld.vue"`可以引入别的组件

![image-20210324214819634](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210324214819634.png)

![image-20210324215022312](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210324215022312.png)

- `ctrl+mouseclick`可以跳转到子组件
- 本质上现在的Vue脚手架变成了层式结构，所有的子组件变成了单独的文件，思路变得特别特别特别清晰嗷呜嗷呜嗷呜！！！
- 脚手架的建立我们可以用手动创建嘛：
  - 类似于Linter实际上语法检查器，我们是可以不需要选择的，因为我们的VSCode里面实际上自带了嗷！！！



## Vue UI

- `vue ui`为可视化页面里面也可以管理项目：

  - 初始化git仓库这个可以不勾选
  - ![image-20210325090811013](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210325090811013.png)

  - ![image-20210325090837745](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210325090837745.png)

    简单的页面甚至用不到路由，不过要康康，那个`CSS Pre-process`要勾选嗷！

  - ![image-20210325091010361](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210325091010361.png)

  - ![image-20210325091043109](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210325091043109.png)还可以保留预设嗷QAQ！下回i据不用这么复杂去配置啦！！！



# Vue示例文件

- `export default`中的数据要写成`data(){ return {bShow:true}}`。
- 全局组件要注意写法哈：

![image-20210325091915288](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210325091915288.png)

- return的括号也是不要卸载第二行：

![image-20210325091945852](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210325091945852.png)

- 如果不指定某个class或者id的组件部分来进行CSS设置的话，设置就会默认成为全局设置。或者：

  `<style scoped lang="less">`，这个也可以设置作用域嗷！！！

- 监听某个事件：`@click`





# 打包发布模拟

- `npm run build`
- 打包好之后生成了一个dist目录
- 把dist目录放置在nginx其中一个目录下，然后改一下conf文件中对应的设置就可以啦！！
- 修改后保存，停止nginx，在nginx目录下输入`nginx.exe -s stop`。然后再开启nginx，对应的内容即设置成功啦！！！



