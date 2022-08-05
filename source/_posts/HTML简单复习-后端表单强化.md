---
title: HTML简单复习-后端表单强化
date: 2021-03-17 19:50:54
tags: 前端
categories: 动力结点后端课程
---



# 老杜的HTML讲的十分粗浅，Head First的内容个人觉得比这个全很多。表单强化很到位，期待后面的CSS复习课程



<!--more-->



# 系统结构

## B/S结构：

- Browser / Server
- Server:
  - python
  - C
  - C++
  - Java(我们选择的)
- Browser:
  - HTML   CSS   JavaScript
- 架构优缺点：
  - 缺点：
    - 速度慢
    - 体验不好
    - 界面不炫酷
  - 优点：
    - 升级方便（只升级服务器即可）
    - 维护成本低
- 企业内部的解决方案都是采取B/S架构的系统
- 代表：
  - 京东
  - 百度
  - 天猫
  - ...







## C/S结构：

- Client / Server
- 特点：有App，需要下载
- 优点：
  - 用户机上已经有好多东西了，就可以操作一些更加复杂的东西
  - 体验好，界面炫酷
- 缺点：
  - 升级麻烦
  - 维护成本高
- 代表：
  - 微信
  - 支付宝
  - ...







# HTML学习

## 一些特点：

- 不区分大小写
- 语法松散不严格！

## 基本标签：

- 段落标记：p
- 标题字：h1,h2....,h6
- 换行：br   ->   独目标记
- 横线：hr   ->   独目标记
- 预留格式：pre（标签内的内容格式和你输入的格式是一样的！！！）
- 删除字：del
- 插入字：ins（有下划线）
- 粗体字：b
- 斜体字：i
- 右上角：sup（类似于平方）
- 右下角：sub（类似于脚注）
- 字体标签：font
- 居中信息：center

## 实体符：

- `&lt;`   <
- `&gt; `   >
- `&nbsp;`   空格

## 数据库表格：

- tabel
- tr -> tabel row
- td -> tabel data
- th -> table head（自动加粗居中）
- 一些属性：
  - border
  - width: 150px or 60%(占页面宽度的60%)
  - height
  - align="center" 居中
  - 注意位置，在表格上就是表格居中，在tr中就是列中文字居中
- 单元格行合并：
  - 找到对应的格
  - 下面的格删掉（只能向下rowspan）
  - 然后用rowspan就可以了
- 单元格列合并：
  - 找到对应的格子
  - 删除右边的（向右colspan）
  - 然后用colspan就可以了
- 表格分为thead , tbody , tfoot，这些不是必须的，但是写css用于分组会更加方便。这三部分在table中不是必须的！！！

## 背景色和背景图片：

- `<meta charset='utf-8'>`这个东西是告诉以什么方式来打开html文件，如果你用的是gbk写的，那就告诉浏览器用gbk打开html文件，**而不是设置**页面的编码格式！！！
- gbcolor-背景色
- background-背景图片
- 图片下面是背景色



## 图片：

- 浮在网络上面的一个元素。
- 背景图片是盒子的底色，而图片是在盒子里放的一个照片。
- img标签
- title是属性，悬停时显示的问题
- src是属性，设置图片路径
- width是属性，只设置宽度（高度）的时候，是等比例缩放。但是两个都设置就容易失真。
- alt是属性，图片加载失败的时候显示的信息。



## 超链接：

- `<a href="www.baidu.com">这是一个超链接</a>`
- href : hot references，这个后面是一个资源的地址
- 路径可以是本地和互联网的
- 超链接！！！永远的神，所有用的是a
- 在a标签里面甚至可以再放一个img，可以做的图片超链接。
- F12在浏览器里面调试！！！
- target属性，可取值为:
  - _blank 新窗口中显示
  - _self 当前窗口显示（默认就是这种方式）
  - _top 顶级窗口（辈分最高的窗口，有爷爷窗口的时候，就不会在父窗口显示）
  - _parent 父窗口





### 超链接作用：

- 超链接可以从浏览器向服务器发送请求
- 服务器向浏览器返回数据
- B/S结构的系统，每一个请求会对一个响应
- 输入网址和点击超链接，从操作上来讲，超链接使用更加方便。原理都是一样样的！





## 列表：

- ul套上li（无序列表）
- 可以嵌套，li里面嵌套ul
- type:
  - disc
  - circle
  - square
  - 可以用于指定前面的样式
- ol套上li（有序列表）
- 也可以嵌套
- type也有好多种，可以选a啊，数字啊，罗马字母啊之类的显示方式



## **表单（最重要）:**

- 收集信息并且提交给服务器

- form来绘制表单

- 网页中可以同时有多个表单

- 表单中重要的是action属性，你提交给哪个服务器啊？？？？，这个action就是用来指定服务器地址的。

- action和href属性一样，都可以向服务器发送请求

- ```html
  <form action="localhost:4000/oa/save">
      <input type="submit">
  </form>
  ```

- type为submit的时候，表示该按钮是提交按钮，具有提交表单的能能力。

- input的type有好多：

  - submit
  - password
  - text
  - radio
  - checkbox
  - button
  - ...

- 只有submit才能提交表单！！！

- 要设置input的名字的话在value属性里面输入嗷！！！，默认值为submit

- 只有处于表单里面的输入数据类似于input数据，都会被submit所发送走。

- ```html
  <form>
      <table border="1px">
          <tr>
          	<td>用户名</td>
              <td>
                  <input type="text" name="username">
              </td>
          </tr>
          
          <tr>
          	<td>密码</td>
              <td>
                  <input type="password" name="userpwd">
              </td>
          </tr>
          
          <tr>
          	<td colspan="2" align="center">
                  <input type="submit" value="登录">
                  <input type="reset" value="清空">
              </td>
          </tr>
      </table>
  </form>
  ```

- reset和submit这两个input属性只有在form内部才有用。

- 记得，要提交的内容一定要有name属性，不然后端是接收不到的，发送都发送不出去。

- input用于输入的，里面的value是不用我们写的，是用户写的，我们写了就是默认值了。

- 提交的格式是：`action?name=value1&name=value2...`，提交的本质上就是name和value。

- mention:

  - 表单写了name属性的会一律提交给服务器，不想提交这一项就不要写name属性。
  - value不需要程序员指定，是由用户提交的。value本质上是默认值，不写value的话，value默认的是空字符串，java代码得到的是""。



## 用户注册的表单：

- 用户注册：

  - 用户名
  - 密码
  - 确认密码
  - 性别
  - 兴趣爱好
  - 学历
  - 简介

- ```html
  <form action="http://localhost:8080/register">
    用户名
    <input type="text" name="username">
    <br>
    密码
    <input type="password" name="userpwd">
    <br>
    确认密码
    <input type="text"> <!--这个可以不用发送，js在浏览器就可以完成判断-->
    <br>
    性别
    <input type="radio" name="gender" value="1" checked>男
    <input type="radio" name="gender" value="0">女
    <!-- name一样代表一组，name不一样就是两组，就都能选了，有bug!!!这里value必须手动写出来，因为用户选择的紧紧用于提交对应的，你需要选定对应的值传到后端 -->
    <br>
    兴趣爱好
    <input type="checkbox" name="interest" value="tutu" checked>tutu
    <input type="checkbox" name="interest" value="lele" checked>lele
    <input type="checkbox" name="interest" value="兔兔">兔兔
    <input type="checkbox" name="interest" value="乐乐">乐乐
    <br>
    学历
    <select name="grade">
        <option value="gz">高中</option>
        <option value="cz" selected>初中</option>
        <option value="dx">大学</option>
    </select>
    <br>
    简介<!--文本域-->
    <textarea rows="10" cols="60"></textarea>
    <br>
    <input type="submit" value="提交">
    <input type="reset" value="清空">
  </form>
  ```

- 单选按钮的name相同，value必须手动设定，checked表示选中的默认的值。

- 多选和单选是一样的。

- 那种可以拉动的框框用的是select，用法和上面相似，看看就知道了嗷！！！

- 文本域没有value属性，用户填写的内容就是value

- 最后submit和clean都不用name，他们不用被提交给服务器。

- 总结一下，表单提交的实际上是各个Input的value，因此这种选项框都应该指定自己的value。

- method属性

  - post -> 提交的信息不会被显示在地址栏上，当有敏感信息比如说密码的时候，就用post。
  - get -> 提交的信息会显示在地址栏上
  - 默认是get方式嗷！！！

- 超链接也能够提交数据，但是提交的数据是固定不变的。

- 超链接是get请求。



## 和表单相关的属性（主要是对于Input的属性）：

1. 下拉列表的多选：

```html
<select multiple="multiple" size="2">
      <option value="gz">高中</option>
      <option value="cz" selected>初中</option>
      <option value="dx">大学</option>
</select>
```

- 按住control是可以多选的

- size设置显示的条目数量

2. file控件，文件上传专用

`<input type="file">`

3. hidden

```html
<form action="...">
    <input type="hidden" name="userid" value="111">
</form>
```

- 数据不想被用户看见，但是又想提交给服务器的时候，就用这个。
- hidden的话，一般和 method= "post" 搭配使用。

4. readonly和disabled

- disabled - 数据改不了，有name属性也提交不了
- readonly - 数据改不了的，但是readonly属性是可以提交的。

5. maxlength

- 设置文本框中的最大输入字符数量

6. 注意提交的数据中间是以&来连接的



## 元素的ID属性：

- 任何元素都有id属性。
- id是为一个，不能重复。
- 表单提交数据的时候只和name有关，与id无关。
- id方便我们去获取元素。
- js可以对任意结点进行增删改。
- 树模型！！！DOM结构。





# Div和Span

- div和span是用于布局的

- div和span都可以被称为图层，保证页面可以灵活的布局
- 盒模型！！！div的嵌套！！！
- 图层就是一个一个的盒子。
- div和span中摆放部件是可以定位的。
- 默认情况下，div会独占一行。
- span是内联元素





















