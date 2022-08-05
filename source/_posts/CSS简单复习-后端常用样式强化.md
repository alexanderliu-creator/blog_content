---
title: CSS简单复习-后端常用样式强化
date: 2021-03-18 09:47:48
tags: 前端
categories: 动力结点后端课程
---





# 老杜的CSS课程非常非常粗浅，仅供入门参考，找时间要把前端的html,css,js,vue框架都给再来一遍，忘了好多呜呜呜，ElementUI，Vue-router，Vuex，还有Vue-cli，兔兔要记得在b站的收藏夹里有相关的课程嗷！本课程链接就不放了，不建议大家学这个orz，不成体系。



<!--more-->





# CSS的作用

- 层叠样式表(Cascading style sheet)
- 修饰html页面，设置某些元素的样式，是html的化妆品。
- css的存在就是修饰HTML
- 程度：
  - 常见的会写
  - 别人的CSS样式能够看懂





# HTML嵌套使用CSS

1. 三种方式：

- 内联定义方式：`<p style="margin-left: 0.5px; border=1px">Tutu</p>`

- 定义内部的style对象

  - ```html
    <html>
        <head>
            <style type="text/css">
                选择器{
                    样式名：样式值;
                    样式名字：样式值；
                }
                选择器{
                    样式名：样式值;
                    样式名字：样式值；
                }
            </style>
        </head>
        <body>
            
        </body>
    </html>
    ```

- 独立写到一个CSS文件中，在HTML中引入（这种最常用！！！）

  - `<link type="text/css" rel="stylesheet" href="css文件路径">`





# 一些常用CSS属性

- border-color
- border-width
- border-style(solid...等)
- border(可以直接转递上面三个属性的参数   宽度   样式   颜色，用空格隔开)





- width
- height
- display(none,block...等)
- background-color
- background
- text-decoration（有none，underline之类的取值）
- cursor（有pointer之类的，设置鼠标在某个元素上时的样式和行为）





- 列表样式：
  - list-style-type（有none，cricle，square之类的值）
- 定位：
  - 绝对定位：
    - position（absolute）
    - left
    - top
  - 相对定位：
    - potion（relative）











# 选择器

- id选择器：`#id`
- 类选择器：`.class`（这个东西可以跨标签）
- 标签选择器：`div`（这个东西作用范围广）



- id只有一个，class可以有多个，中间用" "来隔开。





