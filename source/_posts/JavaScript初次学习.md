---
title: JavaScript初次学习
date: 2021-03-19 20:00:25
tags: 前端
categories: 动力结点后端课程
---



# 老杜的JS教程讲的还不错，学到了很多东西，收获满满！！！[课程连接](https://www.bilibili.com/video/av62653534/?spm_id_from=333.788.b_765f64657363.1)，概念十分清晰。因为我先对于Vue进行了学习，JS这一块儿几乎没碰，导致这一块儿基础薄弱，这下补上了，~~感觉十分舒适~~。对于前后端的帮助感觉都很大！！！

<!--more-->







# JavaScript是啥

- 运行在浏览器中的脚本语言。
- 交互性增强了！！！
- JS的**目标程序**以普通文本形式保存，Java的目标程序实际上是class文件，是不能够以普通文本保存打开的，因此Java不是脚本语言，JS是脚本语言。JS目标程序是由浏览器解释执行的。
- JS能对于DOM树进行增删改查。





# 嵌入HTML页面的方式

1. 使用事件句柄：

   `onclick="JS代码"`

   - 打开页面的时候，js代码不会执行，只是将代码注册到click事件上，当发生事件的时候，js代码会被浏览器自动调用

   `<input type="button" value="hello" onclick="window.alert('hello jscode!!!');`

   `window.alert('hello liututu!!!');">`

   - JS语言写不写分号都可以
   - 可以用双引号也可以用单引号，但是引号里面再使用引号要反过来
   - window可以省略不写，只写alert即可。
   - alert之前见过的啦

2. 使用脚本块儿

   ```html
   <script type="text/javascript">
   	window.alert("Hello World!!!");
   </script>
   ```

   - 这个页面打开就执行了！！！
   - 暴露在脚本块儿中的程序，在页面打开过程中执行，并且自上而下执行，这个代码的执行是不需要事件的！！！
   - 所有的HTML程序，都是从上往下载入浏览器执行的！！！
   - 脚本块儿可以由多个，可以出现在页面的任何位置，不需要任何事件，按照写的顺序执行。
   - JS代码的单行注释是//，多行注释和Java是一样的。
   - alert事件会阻塞当前页面代码的加载，直到用户点击确定按钮为止。因为当前代码直到用户点击才算执行完成。

3. 将JS代码写到对应的JS文件中，引入JS脚本

   ```html
   <script type="text/javascript" src="JS文件路径"></script>
   ```

   - 引入JS代码的时候，也会自上而下自动执行。执行完成之后，才算此条语句执行完成。
   - 同一个JS文件可以被引入多次。
   - 引入JS文件的script标签中写JS代码是没有用的嗷！写的代码是不会执行的，一个脚本块儿，要么引入，要么自己写代码。
   - 引入文件的script标签和写的script标签这个是可以并存的，没啥问题。

- 第一种方法有注册事件，后面两种貌似举的例子没有嗷！







# 事件与事件句柄

- 事件句柄是在事件前面加上一个on
- 事件句柄是以HTML标签的属性存在的！！！



# 标识符和关键字和变量

1. Java中的变量：

- Java规则：只能由几种类型的字符构成，可以有数字，不能以数字开始，不能使用关键字作标识符，严格区分大小写，理论上没有字数限制。（语法的规定）
- Java规则：类名和接口名，首字母大写，后面每个单词首字母大写。方法名和变量名首字母小写，后面每个单词首字母大写。所有的常量要求全部大写，单词与单词之间用下划线连接。见名知意！！！（默认的规范）
- `byte short int long float double boolean char 12484812 `后面的那个是所占的字节数量。
- Java是强类型语言，编译器强行固定了变量的数据类型，为强类型语言。

2. JS中的变量：

- 声明变量：
  - var variable_name
- 复制：variable = value
- 弱类型编程语言，没有编译阶段，一个变量可以随意赋值。JS中变量的类型是由右边说了算嗷！赋什么类型的值都可以嗷！！！
- `var i`这个时候i的值是undefined，js中变量没有赋值的时候默认就是undefined。`var k = undefined`, undefined在js语言中是一个值！！！
- 没有声名直接访问一个变量，实际上是会出现语法错误的，是不能这么干的，语法不对嗷！！！
- `var a,b,c=200`，a,b都是undefined，c是200。







# JS函数

- JS函数等同于java中的方法，可以完成特定功能，但是JS是弱类型语言，其函数的方式更加类似于Python。

- JS中的函数不用返回值的

- 语法格式：

  - `function 函数名(形式参数列表){函数体};`
  - `函数名 = function(形式参数列表){函数体};`

- 两个例子:

  - ```javascript
    function sum(a,b){
    	return a+b;
    }
    ```

  - ```javascript
    sayHello = function(username){
        alert("hello "+username);
    }
    ```

- 函数必须调用才能执行，所以放在脚本块儿中没用嗷，不会自动执行嗷！！！

- 函数调用：`<input type="button" value="hello" onclick="sayHello('tutu!')">`



- 如果只传一个参数，没有赋值的参数，就会自动赋予undefined
- NaN是一个具体存在的值，这个值不是数字，Not a number
- 如果传的参数多于形式参数，多给的值就失效了。
- JS中不需要重载啊兄弟，它本身弱类型猛的亚匹。
- 如果定义同名函数的话，下面的说的算，从上向下执行，下面的会覆盖上面的。
- JS中，函数是不能够重名的！！！





# 局部变量和全局变量

1. 全局变量：在函数体之外声明变量，生命周期是打开时声明，浏览器关闭时销毁，尽量少用，占浏览器的内存。全局变量会一直在浏览器中，耗费内存空间。如果能够使用局部变量，尽量使用局部变量。局部变量使用过后就被释放了！！！‘

2. 一样的，局部变量符合就近原则。

3. 一下语法很奇怪

   ```javascript
   function myfun(){
   	myname = "tutu";
   }
   
   myfun();
   alert("myname = "+myname);
   ```

   - 一个变量声明的时候如果没有用var声明，那不管在哪儿申明，它都是全局变量。**这个特性只有JavaScript才有**





# JS的数据类型

- 赋值的时候，等号右边每一个数据都是由类型的。
- JS中数据类型有原始类型时引用类型：
  - EcmaScript(ES)，ES6之前和之后的数据类型不同
  - ES之前：
    - Undifined
    - Number
    - String
    - Null
    - Boolean
    - Object
  - ES6之后：
    - Undifined
    - Number
    - String
    - Null
    - Boolean
    - Object
    - **Symbol**
- 原始类型：
  - Undifined
  - Number
  - String
  - Boolean
  - Null
- 引用类型：
  - Object和其子类
- ES6之后添加了Symbol
- JS中有一个叫做typeof的运算符，可以在程序运行阶段获取变量的数据类型。由于JS是弱类型语言，无法确定传入参数的类型，这个时候typeof的作用即体现出来了：
  - 语法：typeof + 变量名
  - 返回结果为下列六个字符串之一：
    - undifined
    - number
    - string
    - boolean
    - object
    - fuction
  - 注意，上面没有null！！！没有Symbol。null属于null类型，null的typeof的结果是object嗷。
  - JS中 比较字符串相同用""来进行比较， `if(type a == "number"){...}`
  - 函数没有返回值的话，默认值为undefined，如果调用变量等于函数，这个变量就为undefined。
  - typeof + 变量名这种方式也可以用于判断函数的嗷！！！



## Undefined

- Undefined类型只有一个值，这个值就是undefined
- 当一个变量没有手动赋值，默认undefined，undefined可以用于赋值的。
- `var k = undefined; var j = "undefined";`，第一个才是undefined嗷，第二个是字符串。



## Number

- 包含的值：

  - 数字
  - 浮点数
  - 所有的数字
  - Infinity
  - NaN（不是数字，但是属于Number类型）

- NaN：运算结果应该是一个数字，但是结果不是数字！！！

  ```javascript
  var a = 100;
  var b = "刘兔兔";
  alert(a/b);
  //这个结果就是NaN
  ```

  - 注意哈，如果是相加的话，返回就是字符串哦，字符串还有自动转换拼接的属性。
  - 当除数为0的时候，结果是Infinity嗷。

- 除法不是地板除法嗷，结果和python一样是真实的除法

- 相关函数：

  1. isNaN(data) -> true表示不是一个数字，false表示是一个数字

  - ```javascript
    function sum(a,b){
        if(isNaN(a) || isNaN(b)){
            alert("参与运算的要是数字！！！");
            return;
        }
        return a+b;
    }
    ```

  - 上面是一个例子嗷！！！

  2. parseInt()函数   `parseInt("3.999");` -> 将字符串转换为整数，只取整数位。也可以把float只取整数位
  3. parseFloat()函数   `parseFloat("3.2")+1`   -> 将字符串转换为Float
  4. Math.ceil()函数 -> 用于向上取整！！！   `alert(Math.ceil("2.1"))`  ，返回值为3！！！

## Boolean

1. JS中的bool类型永远都只有true和false

2. if后面跟着的一定要是boolean类型, 如果不是布尔类型，**JS中会自动调用布尔函数转换为布尔类型**。

3. Boolean()函数，作用如上！！！

   ```javascript
   var username=""
   
   if(username){
       alert("Welcome!!!");
   }else{
       alert("用户名不可以为空！！！");
   }
   ```

4. 会被Boolean自动转换为false的东西：

   - 0
   - 空字符串
   - null
   - NaN
   - undefined

5. “有”就转换为false，"真"就转换为true。

## Null

- 这个类型只有一个值叫null
- typeof null = object

## String

1. JS中单引号，双引号都可以用嗷
2. JS当中，创建字符串对象：

   - var s = "abc";
     - typeof s = "string"
   - var s2 = new String("abc");
     - typeof s2 = "object"

   - String的父类是Object
   - 上面是小string，下面是大string，属于object类型
   - 无论大小string，函数和方法都是通用的
3. String类型常用属性和方法

   - length属性！！！`s.length`
   - indexOf(substring) - > 和python一样，存在>0，不存在 = -1
   - lastindexOf(substring) -> python中最后一次出现的字符串的下表
   - replace -> string.replace("原","新")，只替换了第一个？？？-> 多次调用可以的，全部替换的话正则表达式嗷！
   - split
   - substr
   - substring
   - toLowerCase
   - tuUpperCase
4. 三目运算符依旧成立`"string".indexOf("s") >= 0 ? "包含" : "不包含"`
5. substr和substring的区别，substr -> start index , length , substring -> [start , end)





## Object(重点)

- 所有类型的超类，自定义的任何类型默认继承Object

- Object类包含的属性有：

  - prototype（常用）
  - constructor

- Object类包含的方法有：

  - toLocaleString
  - toString
  - valueOf

- 自己定义的类继承自Object，自然有上述属性和方法

- 定义类已知，new对象？

  - 定义类同样是调用函数的方法

  - 但是如果调用“函数”的时候，使用了new，就是构造了对象！！！

  - ```javascript
    function Student(){
        alert("...student");
    }
    
    Student()
    
    //构造的过程中会执行代码块，所以下面new过程也会执行
    var stu = new Student();
    alert(stu)
    // 上面的执行结果为[object Object]
    ```

- 类的定义和构造函数的定义是放在一起的，function就是构造函数，可以声明属性

- ```javascript
  function User(a,b,c){
      this.sno = a;
      this.sname = b;
      this.sage = c;
  }
  
  var u1 = new User(111,"zhangsan",30);
  
  alert(u1.sname);
  ```

- 访问对象的属性还可以：`u1["sno"]`

- 定义类的另外一种方法：`Emp = function(ename,sal){this.ename = ename; this.salary = sal;}`

- 例子：

  - ```javascript
    Product = function(pno,pname,price){
        this.pno = pno;
        this.pname = pname;
        this.price = price;
        
        this.getPrice = function(){
            return this.price;
        }
    }
    
    var pro = new Product(111,'西瓜',4.0);
    var pri = pro.getPrice();
    alter(pri);
    ```

- 可以**通过propotype来对于类动态拓展属性以及函数**`Product.prototype.getPname = function(){return this.pname};`

- 给系统自带的类也可以拓展函数：`String.propotype.suiyi = function(){alert("This a a extended fuction of js")};`

  调用的方法：`"abc".suiyi();`





# null-NaN-undefined的区别

- null -> object , undefined -> undefined , NaN -> number
- ![image-20210319191758936](https://i.loli.net/2021/03/19/oDVLbf7AetkQ6Om.png)

- JS中两个不中不同的等号：
  - == -> 判断值是否相同
  - === -> 即判断值是否相同，又判断数据类型是否相同
- 然后你就会发现`1 == true`是相同的，这是和C语言是一样的嗷！
- `1 === true`和Java中的equals一样的嗷！
- 综上，一般比较的时候用的是`===`





# JS的常用事件

## 事件种类

1. 焦点

- blur -》失去
- focus -》获得

2. 

- change -》下拉列表中项改变，文本内容改变

3. 键盘

- keydown -》按下
- keyup -》弹起

4. 鼠标

- click

- dblclick
- mousedown
- mouseover
- mousemove
- mouseout
- mouseup

5. 表单

- reset
- submit

6. 文本

- select -》文本选定

7. 页面

- load -》整个HTML所有元素加载完毕后发生





## 事件属性

- 任何一个事件都会对应一个事件句柄，就是在事件前面加上on
- onXXX这个事件句柄出现在一个标签的属性位置上



## 注册事件

1. `<input onclick="sayHello()">`，这个sayHello()函数叫做回调函数，callback->函数自己写的，调用函数是由别的函数调用的。这是一种直接注册的方式。

2. 使用纯纯的JS代码:

   - 获取按钮对象

   - document是全部小写，内置对象，直接用，document是整个HTML页面

   - var btnObj = document.getElementById("mybtn")

   - 获得对象后，注册回调函数

   - `btnObj.onclick = doSome;`，千万别加小括号，加上酷哦好是错的

   - window是窗口，document是文档。

   - 匿名函数使用！！！

     - ```javascript
       var btn1 = document.getElementById("mybtn1");
       btn1.onclick = function(){
       	alert("test...");
       }
       
       //这里的匿名函数也是回调函数，事件被注册，只有被调用时才发生
       ```

     - `document.getElementId("id").onclick = function(){...}`，注意这里用的是onclick，这是一个属性。

   - 注意脚本的位置哈，页面从上往下加载的，id要先存在于内存中，我们才能从页面中取到这个结点对象嗷！！！

- 为了解决这个问题，我们先让整个页面元素加载完毕之后，再去调用js代码。这个时候就想到了onload ！！！`<body onload="ready()">;`

## 执行顺序

- ![image-20210319194433415](https://i.loli.net/2021/03/19/VCKaSGo8EzdkiIN.png)

- ![image-20210319194504082](https://i.loli.net/2021/03/19/mIn2ZHPaj71zJFo.png)

- 理清楚加载的关系，这个代码才好些，简化为window先加载完毕，回调函数才能发生。
- 加载过程中，a函数注册给了load事件。加载之后，load事件发生了，此时执行回调函数fuction，执行的时候，把onclick函数注册给了btn按钮。btn按钮被点击的时候，事件发生了。
- 固定搭配：`window.onload = function(){...};`





## 小栗子（文本框点击->复选框）

- 结点对象有的属性你都可以通过.来获得和修改

```html
<html>
    <head>
        <title>This is a test</title>
    </head>
    <body>
        <script type="text/javascript">
        	window.onload = function(){
                document.getElementById("btn").onclick = function(){
                    var test = document.getElementById("mytext");
                    test.type = "checkbox";
                }
            }
        </script>
        <input type="text" id="mytext">
        <input type="button" value="单击变为复选框" id="btn">
    </body>
</html>
```



## 捕捉回车键

- 使用场景：输入完账号密码后，点击enter键自动提交表单！！！

- 回车键键值是13 ， ESC键值是27

- ```html
  <script type="text/javascript">
      window.onload = function(){
          var usernameElt = document.getElementById("username");
          usernameElt.onKeydown = function(event){
              //浏览器会向回调函数传入值的
              //获取键值
              //对于键盘事件来说，有keyCode属性可以获取键值
              if(event.keyCode === 13){
                  alert("用户名: "+usernameElt.value)
              }
          } 
      }
  </script>
  <input type="text" id="username">
  ```

- 所以捕捉`onKeydown`句柄的时候，浏览器会默认对于回调函数传值的，所以你写函数的时候，要接收这个参数

- JS中函数太随意了，我参数塞到函数里去了，你爱用不用，你定义函数的时候写了就用，没写就算了呗！

- 可以测试一下，看看浏览器会返回多少个参数，用alert去康康嗷！！！





# JS运算符之void

- 有好多个，这个玩意儿有好多哇，加减乘除都有，这儿就将一个特殊的，void！！！其他的和Java中的都是一样的。

- 例子：Mission：保留住超链接的样式，同时用户点击超链接的时候，执行一段JS代码，但是页面不能跳转。

  `<a href="" onclick="alert('hehehe!!!')"></a>`

- 上面不对，路径为空就是本身，所以又跳转到本页面的顶部去了。

- void表达式：void(表达式)，运算原理：执行void中的表达式，并且不返回任何结果！！！

- ！！！`href=""`，这其实就是空字符串，就是本页面。但是如果是void的话，**啥都不返回！！！**。连空字符串都不返回嗷！！！代表双引号里面啥都没有！！！！

- `<a href="javascript:void(0)" onclick="window.alert('test code')"></a>`，这里一定要加`javascript`，这个是要告诉浏览器是一段`js`代码，不然浏览器会认为`href`中的是页面链接，就会进行跳转。这个东西是不能省略的！

- ![image-20210320115532712](https://i.loli.net/2021/03/20/RSNZdHITFaWxgJw.png)

- 新世界的大门！！！这玩意儿就这么来的！！！意思就是单击超链接不跳转，这个用法是最常用的嗷！！！

- 这个东西相当于把`href`后的双引号直接废掉了。

- void里面空着是要报错的，一定要写东西，写啥到无所谓，一般是0嗷！！！



- 其余的运算符和用法和Java是一样的！！！



# JS的控制语句

1. if

2. switch

3. while

4. do..while

5. for

6. break

7. continue

   - 上面和Java中都是一样的，下面的是JS特有的

8. for...in语句

   - 数组初始化: `var arr = []`
   - 数组就和python中的数组一样样的！！！
   - 元素的类型和个数随意
   - 遍历数组：
     - `for(var i=0; i<arr.length ;i++){...}`
     - `for(var i in arr){ arr[i] }`//返回的是元素的下标，和Java还不一样。
     - 遍历对象：`for(var propertyName in u){alert(u[propertyName])}` -> 注意哈，返回的是属性的字符串！！！`"username"`用在对象，返回的就是字符串

9. with

   - ```javascript
     with(u){
         alert(username+","+password);
     }
     ```

   - with会使得后面用的变量前面自动加上`u.`，就相当于后面可以直接写属性名来访问！！！



# JS三块

- ECMAScript
  - ES规范，JS核心语法
- DOM
  - 文档对象模型，操作网页中的内容的，DOM树
- BOM
  - 浏览器对象模型，B指的是browser，BOM编程
  - 存在类似于关闭浏览器窗口啊，打开浏览器窗口啊，后退，前进等等
- DOM和BOM的区别和联系
  - BOM的顶级对象是window
  - DOM的顶级对象是document
  - BOM是包括DOM的
  - ![image-20210320153113899](https://i.loli.net/2021/03/20/ra51bvBRM42DF9I.png)





# DOM编程

## 获取文本框的value

```html
<script type="text/javascript">
    window.onload = function(){
        var btn = document.getElementById("btn");
        btn.onclick = function(){
            var usernameElt = document.getElementById("username");
            var username = usernameElt.value; //注意，value是属性嗷！！！
            alert(username);
        }
    }
</script>


<input type="text" id="username">
<input type="button" value="获取文本框的value" id="btn">
```



- `onblur = "alert(this.value)";`，这个时候的this指代的是当前结点，value是节点属性。



## InnerHTML和InnerText操作div和span



```html
<script type="text/javascript">
	window.onload = function(){
        var btn = document.getElementId("btn");
        btn.onclick = function(){
            var divElt = document.getElementById("div1");
            divElt.innerHTML = "......."  //这里面甚至可以是html结点代码。
            divElt.innerText = "....." //这里面就算是HTML代码，也是按照文本输出
        }
    }
</script>


<input type="button" value="设置div中的内容" id="btn">
<div style="backgroundcolor: aquamarine width=300px height=300px border=1px black solid" id="div1">
    
</div>
```

- innerHTML和innerText都是属性嗷！不是方法！！！都是用来设置节点内的内容。





## 正则表达式



### 一些基本语法

- Regular Expression ， 主要用在字符串格式匹配方面嗷！实际上是一门独立的学科，大部分编程语言都支持正则表达式。
- 常见的正则表达式符号：

![image-20210320155946890](https://i.loli.net/2021/03/20/UvhpNnILGlzfWg7.png)

- `[A-Za-z0-9]`,表示A-Z，a-z，0-9中任意一个字符

- `[A-Za-z0-9-]`，表示，A-Z,a-z,0-9,-，以上字符中的任意一个字符。前面的减号带区间，后面的-代表减号

- QQ号正则表达式：

  `^[1-9][0-9](4,)$`，正则中()的优先级较高，如果没有小括号，`(4,)`默认是对于前一个元素的重复次数嗷！

- |表示或者

- 邮箱地址：

  `^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$`

- `\.`代表真正的点，而`.`代表和表中一致。中括号中的.本身代表的就是.嗷！



### 创建正则对象

- 两种方式：
  - `var regExp = /正则表达式/flags;`
  - `var regExp = new RegExp("正则表达式","flags");`
- flags的取值：
  - g
    - 全局匹配
  - i
    - 忽略大小写
  - m(ES规范指定之后才指定m的)
    - 多行匹配
  - 组合
  - 如果/flags前面是正则表达式的话，flags的位置是不能写m的嗷！！！
  - 其实不写正则表达式，写普通的字符串也是可以实现这种操作的嗷！
  - 所以正则主要用的m和i嗷！！！

- test()方法

  - `正则表达式.test()`，true表示成功，false表示失败。

  - 邮箱验证的小例子：

    ```html
    <script type="text/javascript">
    	window.onload = function(){
            document.getElementById("btn").onclick = function(){
                var email = document.getElementById("email").value;
                var emailRegExp = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;
                var ok = emailRegExp.test(tmail);
                if(!ok){
                    document.getElementById("emailError").innerText = "邮箱地址不合法";
                }else{
                     document.getElementById("emailError").innerText = "邮箱地址合法";
                }
            }
            
            document.getElementById("emailError").focus = function(){
                document.getElementById("emailError").innerText = "";
            }
        }
    </script>
    
    
    <input type="text" id="email">
    <span id="emailError" style="color: red; font-size: 12px;"></span>
    <br>
    <input type="button" value="验证邮箱" id="btn">
    ```



## Trim方法

- `var username = document.getElementById("xxx").value.trim();`

- 去除空白字符串！！！

- 低版本的IE浏览器不支持trim方法怎么办？扩展字符串的trim方法即可！！！

  ```javascript
  String.prototype.trim = function(){
  	return this.replace(/^\s+/,"").replace(/\s+$/,"");
      //return this.replace(/^\s+|\s+$/g,"");
  }
  ```

- prototype非常强大！！！



## 表单验证

```html
<html>
    <head>
        <meta charset="utf-8">
        <title>表单验证</title>
        <style type="text/css">
            span{
                color: red;
                font-size: 12px;
            }
        </style>
    </head>
    <body>
        <script type="text/javascript">
        	// 表单结果显示在输入框后面，为了不换行，我们使用span标签
            window.onload = function(){
                var usernameErrorSpan = document.getElementById("usernameError");


                //给用户名文本框绑定blur事件
                var usernameElt = document.getElementById('username');
                usernameElt.onblur = function(){
                    //写一步看一步
                    // alert(111)
                    //获取用户名，看看是不是为空哇
                    var username = usernameElt.value;
                    //去除空白的意识！！！
                    username = username.trim();
                    // alert(username)
                    //判断用户名是否为空
                    if(username){
                        //代表用户名不是空字符串，那就要继续判断长度嗷！
                        // alert("username="+username)
                        if(username.length<6||username.length>14){
                            usernameErrorSpan.innerText = "用户名长度必须在[6-14]之间"
                        }else{
                            //满足以上要求就要判断是否含有特殊符号
                            var regExp = /^[A-Za-z0-9]+$/
                            var ok = regExp.test(username)
                            if(!ok){
                                usernameErrorSpan.innerText = "用户名只能由数字和字母组成"
                            }else{

                            }
                        }
                    }else{
                        // alert("username为空字符串")
                        usernameErrorSpan.innerText = "用户名不能为空嗷呜！！！"
                        //清空非法的输入！！！
                        usernameElt.value = ""
                    }
                }

                //给用户名文本框绑定focus事件
                usernameElt.onfocus = function(){
                    usernameErrorSpan.innerText="";
                }





                //密码和确认密码康康嗷！！！
                var userpwd2Elt = document.getElementById("userpwd2")

                var pwdErrorSpan = document.getElementById("pwdError")


                userpwd2Elt.onblur = function(){
                    //获取密码和确认密码
                    var userpwdElt = document.getElementById("userpwd");
                    var userpwd = userpwdElt.value;
                    var userpwd2 = userpwd2Elt.value;

                    if(userpwd != userpwd2){
                        //密码不一致
                        pwdErrorSpan.innerText = "密码不一致"
                    }else{

                    }
                }

                userpwd2Elt.onfocus = function(){
                    pwdErrorSpan.innerText = ""
                }







                //给email绑定blur事件
                var emailElt = document.getElementById("email")

                var emailSpan = document.getElementById("emailError");

                emailElt.onblur = function(){
                    //获取email
                    var email = emailElt.value;

                    var emailRegExp = /^[A-Za-z\d]+([-_.][A-Za-z\d]+)*@([A-Za-z\d]+[-.])+[A-Za-z\d]{2,4}$/;

                    var ok = emailRegExp.test(email)
                    if(ok){
                        //合法
                    }else{
                        //不合法
                        emailSpan.innerText = "邮箱地址不合法";
                    }
                }

                //绑定邮箱的onfocus事件
                emailElt.onfocus = function(){
                    if(emailSpan.innerText != ""){
                        emailElt.value = "";
                    }
                    emailSpan.innerText = ""
                }






                //给submit绑定鼠标单击事件
                var submitBtnElt = document.getElementById("submitBtn");
                submitBtn.onclick = function(){
                    //如何确定表单合法？？？
                    //让每一项都经过blur符合要求，就没问题了嗷！！！
                    //方式很多嗷！！！
                    //通过代码自动触发各种不同的blur

                    usernameElt.focus();
                    usernameElt.blur();

                    userpwd2Elt.focus();
                    userpwd2Elt.blur();

                    emailElt.focus();
                    emailElt.blur();

                    if(usernameErrorSpan.innerText=="" && pwdErrorSpan.innerText==""&&emailSpan.innerText==""){
                        var userFormElt = document.getElementById("userForm");
                        // userFormElt.action = ""，这里是可以设置的
                        //form有方法可以直接提交表单
                        userFormElt.submit();
                    }
                }




            }
        </script>
        <form id="userForm" action="http://localhost:8080/jd/save" method="get">
            用户名<input type="text" name="username" id="username"><span id="usernameError"></span><br>
            密码<input type="password" name="userpwd" id="userpwd"><br>
            确认密码<input type="password" id="userpwd2"><span id="pwdError"></span><br>
            邮箱<input type="text" name="email" id="email"><span id="emailError"></span><br>
            <!-- 改成button是为了实现，所有表单的内容符合条件之后，才能够提交表单 -->
            <input type="button" value="注册" id="submitBtn">
            <input type="reset" value="重置">
        </form>
    </body>
</html>
```



## 多选框一键全选，一键取消

- 对于全选的那个框加一个id，绑定单击事件，如果单击了，后面的框全部选中即可。
- 可以同时获取多个元素`var aihaos = document.getElementsByName("aihao")`
- 进行遍历`for(var i=0;i<aihaos.length;i++){ aihaos[i].checked = true;}`
- checked属性要知道嗷，这个属性为true表示选中了，这个属性为false表示没选上。所以这里的话，可以通过onclick事件里面获取底下全部的元素，并且全部改成false或者true。
- checkbox的onclick事件实际上会更改checked这个属性的值。
- 对于多个元素设置回调函数

```javascript
for(var i=0;i<aihaos.length;i++){
    var checkedCount = 0;
    aihaos[i].onclick = function(){
        checkedCount += 1;
    }
    firstChk.checked = (checkedCount==aihaos.length);
}
```





## 获取下拉列表选中项的value

- JS获取下拉列表中内容对应的value
- 市区和省份的关系是：一对多。有一张省份表，一张市区表。
- 前端选中的数据对应的值穿给后端，后端通过前端的value查询相应的值，然后数据库返回新的给前端，浏览器对于新的数据渲染后显示出来。
- onchange事件，表示下拉列表选中项改变的事件
- `provinceListElt.onchange = function(){}`
- 这个属性是对于下拉列表`select`来说的，onchange事件对应的被选中的那个option的值被赋予了select下拉列表，然后就可以`alert(selectName.value)`









# JS内置支持类Date

- `var nowTime = new Date()`获取当前使劲按

- `document.write(nowTime);`可以输出事件

- 转换成具有本地语言环境的日期格式: `nowTime = nowTime.toLocalString()`，这就获取更加清晰的时间

- 可以自定义日期格式

  ```javascript
  var t = new Date();
  var year = t.getFullYear();//全格式返回年份
  var month = t.getMonth();//月份是0-11
  //var day = t.getDay();//获取一周的第几天(0-6)
  var day = t.getDate();//这个才是日信息
  document.write(year+"年"+(month+1)+"月"+day+"日")
  
  document.write("<br>")
  ```

- 怎么获取毫秒数？（从1970.1.1 0点到现在的总毫秒数: `var times = t.getTime()`

- 一般使用毫秒数当作时间戳。



# setInterval()

- 周期函数嗷！！！

- 每隔一秒钟调用函数来显示时间

```html
<script type="text/javascript">
function displayTime(){
    var time = new Date();
    var strTime = time.toLocaleString();
    document.getElementById("timeDiv").innerHTML = strTime;
    
    //window.setInterval("displayTime()",1000)
}

function start(){
    v = window.setInterval("displayTime()",1000) //全局变量,这个好细啊！！！
}
    
function stop(){
    window.clearInterval(v);
}
</script>

<input type="button" value="点击显示时间！！！" onclick="start()">
<input type="button" value="点击时间停止！！！" onclick="stop()">
<div id="timeDiv">
    
</div>
```





# 内置支持类: Array



- `var arr = []` , `var arr2 = [false,1,2,3,4,"abc]`

- `arr2[7] = "test"`

- ```javascript
  for(var i = 0;i < arr2.length;i++){
      document.writeln(arr2[i]+"<br>");
  }
  ```

- 发现数组可以自动赋值，而且自动补上undefined！！！

- `var a = new Array(3)`，三表示元素

- `var a = new Array(1,2)`，里面的表示元素



## 数组内置方法

- ```javascript
  var a = [1,2,3,9]
  var str = a.join("-")
  //连接成字符串
  ```

和python做下对比哈：

![image-20210322172757573](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210322172757573.png)

而且python中连接的对象本身要是字符串

- push，pop，和python中的方法是一样的嗷！！！JS中的数组可以自动模拟栈的数据结构，遵循后进先出，先进后出的原则！！！在python里面，push是append嗷！！！还有Index方法嗷！！！
- reverse()方法也有嗷！！！`a.reverse()`，和python中一样，改变的数组自身嗷！！！







# BOM编程

## open和close

- window是顶级对象，open是开启窗口，close是关闭窗口

```html
<input type="button" value="开启百度" onclick="window.open("www.baidu.com","_blank");">
```

- 第二个参数之间见过，父窗口啊，子窗口啊，当前窗口啊等等。
- 点击浏览器的叉号和点击`window.close()`是一样的！！！





## alert和confirm

- 弹出消息框和确认框

```javascript
function del(){
    //var ok = window.confirm("要删除数据吗？");
   // alert(ok);
    if(window.confirm("要删除数据吗？")){
        //删除操作
    }else{
        ...
    }
}
```

- 联合起来一起写













## 历史记录

- `onclick = "window.history.back()"`
- 和页面中的后退时一样的
- `window.history.go(step)`，step表示前进的步数
- 第一次进入的时候，前进和后退都走不了，后退的代码比前进的代码要写的多



## 设置浏览器地址上的url

- `var location = window.location;`
- `location.href = "http://www.baidu.com"`
- 或者换一种写法：`window.location.href = "http://baidu.com"`
- document也有location , `document.location.href = "..."`也行，甚至把`href`删了也没事儿。`document.location="http://www.baidu.com"`
- 如何向服务器发出request:
  - form
  - 超链接: `<a href="http://..."></a>`
  - `window.loaction()`
  - `document.location()`
  - `window.open(url)`
  - 直接输入URL然后回车嗷！！！



## 将当前窗口设置为顶级窗口

```javascript
function setTop(){
    if(window.top!=window.self){
        window.top.location = window.self.location;
    }
}
```

- 使用场景：

![image-20210322172700184](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210322172700184.png)

- 点击左边的，切换右边的顶层的页面。

- `<iframe src="...">`这个玩意可以设置桌面的嵌套





# JSON（非常重要）

## 什么是JSON?

- `Javascript Object Notation`，是一种数据交换格式

- JSON主要的作用作为一种标准格式进行数据交换，轻量级的标准数据格式

- 特点：

  - 体积小
  - 易解析

- 常用数据交换格式：

  - JSON
  - XML:
    - 体积大，解析麻烦
    - 但是语法严谨
  - XML可以用浏览器解析语法是否正确

- ```javascript
  var jsonObj = {
      "sno":"110",
      "sname":"zhangsan",
      "sex":"男"
  }
  ```

- `alert(jsonObj.sno+","+jsonObj.sname+",")`

- JSON也被称为无类型对象，体积小，易解析。

- `var students = [{Json1},{Json2},{Json3},...]`

- Java里面可以从数据库里查出数据，组合成`JSON`,实现和浏览器的数据交互。



## JSON语法格式

`var jsonName = {"属性名":"属性值};`

- 复杂的json对象

```javascript
var user = {
    "usercode": 10,
    "username": "liututu",
    "sex": true,
    "address":{
        "city": 1,
        "street": 2,
        "zipcode": 3,
    }
}
```

- 设计JSON格式数据，包含总人数信息和每个人的信息

```javascript
var class = {
    "total_num": 60,
    "students":{
        {},
    	{},
    	{},
    	...
    }
}
```

- eval函数的作用是将一段字符串当作JS代码解释并执行，`window.eval("var i = 100;");`。

- Java连接数据库查询数据之后，java中拼接成JSON格式的字符串，将JSON格式的字符串相应到服务器！！！注意，是JSON格式的字符串，还不是一个JSON对象。

- 可以使用eval，将JSON格式的字符串转换为JSON对象。

- `window.eval("var jsonObj = "+"{\"name\":\"zhangsan\",}")`

- 把字符串拼起来，然后转换为JSON对象，完美！！！

- 面试题：在JS当中，{}和[]之间有什么区别？

- JS中：

  - []是数组
  - {}是JSON（在JS中是对象）

- 访问JSON中的对象：

  - `json.username`
  - `json["username"]`

  - 这个学过很多次了嗷！！！





# 设置Table的tbody

```javascript
var data = {
    "total":4,
    "emps":[
        {"empno":1,"ename":"liututu","sal":800},
        {"empno":2,"ename":"liutu","sal":900},
        {"empno":3,"ename":"liu","sal":800},
        {"empno":4,"ename":"lele","sal":1600}
    ]
}
```

- 假设这是后端传回来的数据，要在table里面展示出来嗷！！！
- 展示的tbody:

```html
<input type="button" value="显示员工信息列表" id="displayBtn">
<table>
    <tr>
    	<th></th>
        <th></th>
    </tr>
    <tbody id="emptbody">
        <!--
        <tr>
            <td></td>
            <td></td>
        </tr>
		-->
        <!--这个里面放置的是后端传回来的数据-->
    </tbody>
</table>
总共<span id="count">0</span>条数
```

- 上面都是题目，下面才是解决方法：

```javascript
window.onload = function(){
    var displayBtnElt = document.getElementById("displayBtn");
    displayBtnElt.onclick = function(){
        var emps = data.emps;
        var html = ""
        for(var i=0;i<emps.length;i++){
            var emp = emps[i];
            html += "<tr>";
            html += "<td>"+emp.empno+"</td>"
            html += "<td>"+emp.ename+"</td>"
            html += "<td>"+emp.sal+"</td>"
            html += "</tr>"
        }
        document.getElementById("emptbody").innerHTML = html；
        document.getElementById("count").innerHTML = data.total;
    }
}
```







# 后期学习方向

- XML
- Servlet
- Filter
- Listener
- JSP
- EL
- JSTL
- MVC架构模式
- 动态代理
- Maven（包管理工具，已经学完了）



















