---
title: Vue路由学习
date: 2021-03-27 20:55:37
tags:
	前端
---



# 路由学习，下周就要用，赶紧冲一把



<!--more-->





- 下载与安装：[路由官网](https://router.vuejs.org/zh/guide/#html)

- 至于在`Vue3.0`版本中如何使用，这个使用就要参考`element-ui`的引入方式了嗷！！！`Element-UI`中，对于外界包的引入有指导作用，想不起来可以去找那个嗷！！！

![image-20210327210132984](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210327210132984.png)

- 等等我发现这个`vue add router`无敌！！！直接帮你自动干好一切工作，绝了，👍！！！

![image-20210327211047804](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210327211047804.png)

- components里面主要是公有组件，单页面主要放在view里面嗷！！！





- scss的要求比较严格，这个时候在底下的`style`里面如果写对应的样式的话，`.`就只能够用于访问class类型的标签了嗷。





# 路由设置

- router.js
- 由于本身设置好了，你可以照着那个样式来写router规则即可。
- `App.vue`中，可以添加导航栏

```html
<div id="nav">
    <router-link to="/">Home</router-link>
    <router-link to="/about">About</router-link>
</div>
<router-view></router-view> //导航跳转的页面会加载到这里来
```



- 进入页面之后直接由路由接管跳转，他会在你注册的路由里面去寻找响应的子组件并且渲染到合适的位置上返回页面给你，如果

  ![image-20210328101649816](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210328101649816.png)

  这个没有被设置，那么默认返回给你的就是一个空页面`localhost:8080/#/`

- 控制登录的话就需要路由守卫啦！！！

## 跳转：

`this.$router.push(...)`

## 编程式导航：

![image-20210328104837969](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210328104837969.png)

- 这里选中会把name赋值

![image-20210328105018383](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210328105018383.png)

- 代码点击，然后通过对应的方法跳转



## 动态路由：

- `path:'/iview_router/:name'`，后面输入的值会作为name变量传入到组件中去嗷。
- 页面输入的`name1`，从动态路由的名字到组件里面来。
- 编程式导航则是通过点击下面的，将信息传到上面的输入栏来。
- 组件复用：

![image-20210328105513546](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210328105513546.png)





## 嵌套路由：

![image-20210328105926576](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210328105926576.png)



- 渲染位置：

![image-20210328110017192](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210328110017192.png)

- 感觉和那个差不多？？？路由嵌套组件会渲染到父组件的这个位置。
- `$router.params.id`这样式可以获取到上面那个位置填入的id的嗷。



## 路由重定向：

![image-20210328110446405](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210328110446405.png)

- *代表输入的匹配不上嗷
- 重定向关键词：redirect
- `/iview_router/:name`，这个时候不写name的话，也会重定向到这里嗷
- 名字在命名路由中使用嗷



## 路由参数传递：

1. 就上面讲的`/:id`这样可以拿到参数

2. ![image-20210328110814058](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210328110814058.png)

   ![image-20210328110848914](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210328110848914.png)

   和父穿子的方法和做法是一样的嗷

3. 如果还要往子组件传的话，再用父传子，或者bus也可嗷。





## 路由守卫：

### 全局守卫：

`router.beforeEach(to,from,next)=>{...}`全局前置守卫

```javascript
router.beforeEach(to,from,next){
	if(to.path == '/iview_index'){
		if(auth.islogin()){
			next()
		}else{
			next('/iview_login')
		}
	}else{
        next()
    }
}
```

- islogin()实际过程中要后端传数据进行验证的嗷



### 局部守卫：

![image-20210328112225541](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210328112225541.png)

- 新建组件之前，先进行守卫，判断是否应该新建这样一个组件。
- 如果要时时刻刻更新就要用`watch`了，因为组件那边，不重新加载页面时不会重新创建这个组件的，只会重载页面。



## 输入获取：

- 利用特定的钩子函数就可以实现啦！！！

![image-20210328112845060](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210328112845060.png)



