---
title: Servlet学习
date: 2021-04-02 17:13:04
tags: 后端
categories: 动力结点后端课程
---





# Servlet学习

好长呜呜呜，终于看完了，后面的计划是从JSP再转到SSM框架，先赶进度，暑假一定抽时间把CRM实际开发和Linux上的部署那一部分给看完嗷！！！



<!--more-->



# Servlet规范

- 来自于JavaEE规范中的一种
- 作用：
  - Servlet规范中，指定动态资源文件开发步骤
  - 指定Http服务器调用动态资源文件规则
  - 指定Http服务器管理动态资源文件实例对象规则



## 接口实现类：

1. Servlet接口来自于Servlet规范下一个接口，这个接口存在Http服务器提供jar包
2. Tomcat服务器下lib文件有Servlet接口
3. Http服务器能够调用的动态资源文件必须是一个Servlet接口实现类

## Servlet接口实现类开发步骤：

1. 创建Java类继承与HttpServlet父类，使之成为一个HttpServlet接口实现类。
2. HttpServlet实现了Servlet里面我们不需要写的一些方法，简化了开发难度
3. 将Servlet接口实现类注册到Tomcat服务器



## 一些注意对象

1. 接口：

   - init
   - getServletConfig
   - getServletInfo
   - destroy

   --上面四个对于Servlet接口实现类没用

   - service

    --有用

2. Tomcat根据Servlet规范调用Servlet接口类规则：

   - Tomcat有权创建Servlet接口类实现对象。
   - Tomcat根据实例对象调用service方法处理当前请求。

3. 开发步骤：

   - 创建一个Java类继承HttpSevlet方法
   - 重写HttpServlet父类的两个方法：doGet和doPost
     - 浏览器以get就会调用doGet，post就会调用doPost
   - 将Servlet接口实现类注册到Tomcat服务器

4. Tomcat如何调用service方法：

   - HttpServlet这个类已经实现了service类，本质上是通过分析请求头中的字段来调用对应的处理方法。
   - 已经帮你处理好了，后面都不用动了呀
   - 这是一种设计模式，这种设计模式被称为模板设计模式。

## Tomcat注册

- 网站   ->   web   ->   web-inf   --->   web.xml
- 将Servlet实现类类路径交给Tomcat

```xml
<!--第一步，动态资源文件告诉Tomcat-->
<servlet>
	<servlet-name>tuzi</servlet-name><!--声明变量存储类路径-->
    <servlet-class>com.liututu.controller.OneServlet</servlet-class>
</servlet>

<!--第二步，为了降低Servlet接口实现类难度，设置短别名-->
<servlet-mapping>
	<servlet-name>tuzi</servlet-name>
    <url-pattern>/one</url-pattern><!--设置简短请求别名-->
</servlet-mapping>
```

- 上面这些是通知Tomcat哪些是动态的Java文件
- Tomcat会在内存中`Tomcat String tuzi = "com.liututu.controller.OneServlet"  `
- 别名以/为开头
- 如果浏览器向Tomcat索要OneServlet,就是直接端口后后面加上网站名再加上`/name`就可以。
- 给注册的servlet-name对象设置一个简短的请求别名



## Servlet生命周期

1. 所有Servlet接口实现类只能由Http服务器负责创建。开发人员不能手动创建Servlet接口实现类的实例对象。

2. 默认情况下，Http服务器接收第一次请求时，自动创建这个Servlet接口实现类的实例对象。

3. 手动配置情况下可以要求服务器启动时就自动创建某个servlet接口实现类的实例对象。

   在<servlet></servlet>中加入<load-on-start></load-on-start>标签就好啦！在里面填一个大于0的整数就成，比如说30.

4. 在Http服务器运行期间，一个Servlet接口只能被创建出一个实例对象。

5. 在Http服务器关闭的时候，所有server的对象都会被Servlet对象进行销毁。



## 快速生成Servlet

![image-20210403173303891](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210403173303891.png)





![image-20210403173208462](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210403173208462.png)

- 这样就能快速创建Servlet



# HttpServletResponse接口

## 介绍：

- doGet和doPost方法都有两个部分：
  - Request
  - Response
  
- 介绍：
  - 接口来自于Servlet规范中，在Tomcat中存在。
  - 实现类由Http服务器提供。
  - Http接口将方法结果写入到响应体交给浏览器。
  - 开发人员习惯称为相应对象。
## 主要功能：

  - 将执行结果以二进制写入到响应体中。
  - 设置响应头中的content-type属性，控制浏览器使用对应编译器来编译。
  - 设置响应头中的location属性，将一个请求地址赋值给location，从而控制浏览器向指定服务器发送请求。



## 响应对象将结果写入到响应体

1. `PrintWriter out = response.getWriter()`
2. `out.write(result)`
3. 不用释放这个out，这个out是你从Tomcat里面借的，不是你创建的，所以不用你释放嗷！！！



## 出现的问题和相应的知识点

1. 

- out.writer是可以写入数字啊，字符串之类的
- 但是要注意写入的应该是`ACSII`码，这个时候直接输入数字就会出现问题，Tomcat自动将这个数字当`ACSII`码转换了。
- 这个时候改方法，用out.print()方法。将真实数据写入响应体，注意，真实开发过程中也是这种用到多嗷。

2. 

- 你甚至可以输入字符串，里面写html代码？？？浏览器会自动解释代码实现html的功能，例如换行吗？不会，你发现浏览器并不会将其转换为html代码。
- 调整content-type来设置编译处理的方式，默认是`content-type="text"`，这怎么解决呢？
- 一定在得到输出流之前，通过对象响应头中的content-type属性进行一次重新赋值指定浏览器使用正确的编译器。

```java
String result = "java<br/>Mysql";
//要先进行设置嗷！！！
response.serContentType("text/html");
//这个时候向Tomcat索要输出流
PrintWriter out = response.getWriter();
out.print(result);
```

3.

- 每次调整完重启Tomcat很麻烦怎么办呀

![image-20210403202803338](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210403202803338.png)

用debug启动，可以实时更新相关内容到Tomcat。

4. 

- 中文在网页中显示不出来怎么办呀
- 设置对应的编码格式，可以通过浏览器查看编码方式在哪儿，然后修改一下。

```java
String result = "java<br/>Mysql";
//要先进行设置嗷！！！
response.serContentType("text/html;charset=utf-8");
//这个时候向Tomcat索要输出流
PrintWriter out = response.getWriter();
out.print(result);
```

5. 设置location

```java
String result = "https://www.baidu.com";
response.sendRedirect(result);
```

- 浏览器收到相应包之后，如果存在location，自动通过地址栏对于指定网站发送请求
- `sendRedirect`可以远程控制浏览器请求行为，请求地址，方式，参数等

带参数：
```java
String result = "https://www.baidu.com?username=Mike";
response.sendRedirect(result);
```

- 这个工作前后端都可以做嗷





# HttpServletRequest接口

## 介绍：

1. 来自于Servlet规范中，在Tomcat中存在。
2. 实现类由Http服务器提供。
3. HttpServletRequest负责在doGet/doPost方法运行时读取Http请求协议包中的信息。
4. 开发人员习惯于称为请求对象



## 主要功能：

1. 读取Http请求行中的信息
2. 保存Http请求头或者请求体中的参数信息
3. 可以代替浏览器向Http服务器申请资源调用



## 读取请求行中的url信息

- `String url = request.getRequestURL().toString()`
- `String method = request.getMethod()`

### URI和URL的区别：

- URI：资源文件精准定位，请求行中并没有，实际上是从URL中截取的一部分字符串。这个字符串格式为"/网站名/资源文件名"，用于让Http服务器对于访问的资源文件进行定位。

### 读取URI:

`String uri = request.getRequestURI()`

### 读取请求参数名称：

```java
Enumeration paramNames = request.getParameterNames();
while(paramNames.hasMoreElements()){
    String paramName = (String)paraNames.nextElement();
    //处理
}
```

### 读取指定请求名：

`request.getParameter(paramName);` - 这里的Name和前端就对应上了嗷。表单中传回的数据的name就是这个玩意儿。



### 读取请求体中参数信息：

`request.getParameter(paraName)`



### 一些问题：

- 以get方式发送中文的参数内容得到正常结果，以post方式发送中文的参数内容的时候，就有问题。
- 原因：
  - get发送，信息保存在请求头中
  - post发送，信息保存在请求体中
  - 请求头中的数据由Tomcat解码，而Tomcat使用的utf-8变法解码，问题不大
  - 请求体中的数据由request解码，request不使用utf-8，而使用`ISO-8859-1`字符集解码，导致中文无法正常解释。
- 解决：Post方式下，在读取请求头内容之前，通知使用utf-8进行解码。`request.setCharaceterEncoding("utf-8");`







# 生命周期

- Http服务器接收到浏览器发送的Http请求协议包之后，服务器就会自动生成一个request和一个response对象。
- Tomcat调用这些方法的时候，就自动将这两个对象作为实参传递到方法中。
- Tomcat准备推送相应协议包之前就会把request和response进行销毁处理。

- 生命周期贯穿一次请求的处理过程中。
- 实际上相当于用于在服务端的代言人和负责人。

![image-20210405144357420](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210405144357420.png)









# 静态资源文件

- 放在对应的文件夹中
- 请求资源文件的时候要写全的，例如`/one.html`，servlet就会自动返回前端的资源。



# 在线考试管理系统的例子







# 特别恶心的在Maven中添加Servlet

## 添加Servlet依赖：

![image-20210405202210090](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210405202210090.png)

- 如果添加完之后显示红色且没有反应的话

![image-20210405202323380](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210405202323380.png)





## 关于具体的文件夹添加过程：

[Maven添加简单的Servlet依赖](https://blog.csdn.net/qwer123459/article/details/90373343)

mention:

1. Java那个文件包的名字是可以改的，只要是source目录就行
2. Java这个包原来没有，需要你自己去建立，最好就是建立Java和Resource
3. 一开始创建项目成功的标志是：自动生成的webapp文件夹上面有个蓝点！！！一开始是没有Java这个包的
4. 创建完Java和Resources为了保证完整性最好把test之类也创建好。
5. 为什么我要做这个？因为教程中自己导包是在负载，尤其是和之后的Mysql结合使用，晕死了，Maven才是神仙。
6. 这里的Java实际上和正常Java-web项目中的src资源目录文件包是等价的。
7. 只有在依赖成功导入的情况下，main文件夹下才能方便创建Servlet项目，不用自己创建Java类，直接在main下创建对应的Servlet就好。
8. ![image-20210405203135809](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210405203135809.png)不要忘了不要忘了不要忘了，忘了好惨的orz
9. ![image-20210405203101898](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210405203101898.png)

注意这里导入包的时候选择的是这个带exploded的，这个才是你写的Servlet好吧

10. ![image-20210405203223825](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210405203223825.png)

下面这个记得填上嗷，完整的访问应该是`本机:8080/webTest/one`这种形式，搞定！！！















# 数据库前后端交互小例子：

## 所需的两个类：

1. 对象类，封装前端返回的post或者get构成的对象，将每一个字段封装为类中的一个属性
2. DAO类，封装数据库的增删改查操作（或者是util类）。接受上面所说的User对象，将对象中的各个字段用于与数据库进行操作。接受参数常为第一个对象类的实例对象，返回结果为数据库操作后类似于修改的行数等数据。



## 关于资源文件访问：

- 对于动态资源文件，我们配置过Servlet了，在访问`myWeb/one`这种情况下，例子中后端接收了请求然后拿了一个out的句柄，然后写到页面里面去了。
- 对于静态页面来说，访问`myWeb/one.html`才会访问这个网站，如果和后端发送了交互，会像上面一样在action被get的时候，跳转到一个动态资源文件的url上。交互后的内容和上面一样，这样又回到了上面。

- `response.sendRedirect(url)`可以重定位网页。
- 登陆验证失败了其实可以返回一个新的页面和登陆页面一样的，只是上面多了一行提示请重新登陆。

## 默认欢迎资源文件：

- 在Vue里面就是一个路由的配置将其导航到导航页面而已

- 由Http服务器自动从当前网站返回的资源文件

- Http自动定位返回的文件就是默认欢迎资源文件

- Tomcat对于默认欢迎资源文件的定位规则：

  1. Tomcat安装位置/conf/web.xml中配置有写的：

     - welcome-file-list中有写默认文件

     - 好像是index.html , index.htm , index.jsp就是默认的文件，你当然可以自己配置鸭！！！

     - 如果配置中的默认的这三个都没找到就会返回404页面

  2. 思考：

     - 这个东西好像在服务器上装一个nginx前端就跑起来了，不用后端操心这个烂事儿。

  3. 设置当前默认欢迎资源文件：

     - 规则位置：网站/web/WEB-INF/web.xml

     - 规则命令：

       ```xml
       <welcome-file-list>
       	<welcome-file>login.html</welcome-file>
       </welcome-file-list>
       ```

     - 自定义会使得默认的规则失效嗷。

  4. 这里其实也可以配置动态资源文件的，在上面那个位置其实可以使用配置中动态资源文件的别名来指定动态资源文件。

  5. 在配置动态资源文件的时候，和静态资源文件一样样的，前面第一个横线要删掉！！！不用前面第一个位置上的横线的。



# Http状态码：

- 三位数字组成的一个符号。
- Http服务器推送相应包之前，根据本次请求处理情况，将Http状态码写入到相应包中的【状态行】上。
- 通过Http状态码通知浏览器如何处理这个结果，解释提供服务的状态和原因。

## 分类：

- 从100--599

### 1xx:

- 最有特点的是100
	- 本次返回的资源文件不是独立的资源文件，需要浏览器继续向服务端发送请求，获取后续的资源。(例如浏览器中的页面中嵌套的图片)。
	- 这个状态码是由Tomcat服务器处理后发送给服务器的。
	- 服务器得到100后，根据html中的内容，再去向服务器请求资源文件即可。
	- 这一次的交互后，Tomcat就会返回200

### 2xx：

- 最有代表性的是200，标识的是返回的资源文件是一个完整的资源文件。

### 3xx：

- 最有代表性的是302：
  - 通知返回的不是一个资源文件内容而是一个资源文件地址。浏览器收到302就会向这个地址自动发起请求来索要这个资源文件
  - `response.sendRedirect("资源文件地址")`，这个会自动写入响应头中的location，这个实际上服务器就会将302写入到状态行。
  - 浏览器收到了302就不会去读取这个响应的响应体，而是根据响应头中的location的地址发起第二次请求。

### 4xx：

- 最具代表的是404：
  - 没有定位到被访问的资源文件。
- 还有405：
  - 服务端定位到了被访问的资源文件
  - 访问文件必须要是Servlet
  - Servlet对于浏览器采用的请求方式不能够处理，例如只写了doGet，但是浏览器是用Post请求，这个时候Tomcat就会返回405。
- 404和405一般都在开发调式过程中被灭掉了。



### 5xx：

- 典型例子：500
  - 通知浏览器，Servlet在处理过程中，Java处理过程中却出现问题了，处理不好？？？你小子演我呢？？？







# 多个Servlet之间的调用规则



## 相互调用：

- Servlet文件之间相互调用嗷！！！！

### 前提条件：

- 请求需要多个Servlet协同处理，但是浏览器一次只能够访问一个Servlet，如果用户来依次访问执行用户人都傻了orz。

### 满足用户感受规则：

- 用户访问一次，浏览器后端无论多少个Servlet交互，你后端都要解决

### 调用规则：

- 重定向解决方案
- 请求转发解决方案

### 重定向解决方案：

- OneServlet工作完毕后，将TwoServlet地址写入到响应头location交给浏览器。
- 浏览器拿到这个后（对应的状态码是302），浏览器就会自动去发起第二次请求。（说白了是浏览器自动干的，就不用用户去操作了）。
- 原理图：

![image-20210411200208279](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411200208279.png)

- 实现命令：
  - `response.sendRedirect("请求地址")`
- 实际例子：
  - `response.sendRedirect("/myWeb/two")`
  - 格式：`/网站名/资源名`
- 降低了用户获取资源的难度
- 特征：
  1. 请求地址：
     - 既可以把内部资源内部文件地址给浏览器
     - 也可以把其他网站资源文件地址给服务器
  2. 请求次数：
     - 发送请求次数至少为2次
     - 只有第一次是用户手动发送的
  3. 请求方式：
     - 后续浏览器的请求方式，实际上都是以get方式来操作的。
     - 用浏览器地址栏来发送请求的都是get方法嗷！！！
  4. 缺点：
     - 浪费时间：发多次请求，来回非常浪费时间。
     - 增加用户等待时间。





### 请求转发解决方案：

- 原理：
  - 服务端直接通过当前请求对象代替浏览器向Http服务器继续申请调用。
  - Tomcat接收这个请求后自动调用Servlet来完成剩余任务

- 原理图：

![image-20210411201613353](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411201613353.png)

例子是网课的，这破例子不是我举的，我是无辜的呜呜呜。

- 实现命令：

  1. 

  - `RequestDispatcher report = request.getRequestDispatcher("/资源文件名")`
  - 一定要以/来开头！！!

  2. 

  - `report.forward(当前请求对象，当前响应对象)`
  - 这里的就是：request , response
  - 上面这两条是卸载请求发送方的。

- 优点：

  - 无论本次请i去涉及到多少个Servlet，用户手动发送一次。
  - Servlet调用发生在服务端，节省了往返次数。

- 特征：

  1. 请求次数：
     - 请求转发过程中，浏览器只发送一次请求
  2. 请求地址：
     - 只能向Tomcat服务器申请调用当前网站下的资源文件地址
     - 请求地址直接斜杠+资源文件名，不要去写网站名
  3. 请求方式：
     - Tomcat中相互调用的时候用的也是刚刚开始的时候，浏览器发送的那个请求协议包。
     - 如果浏览器第一次申请用的是GET，后面的就都是GET，如果是POST，后面的就都是POST。
     - **多个Servlet的响应请求的方式和浏览器发送的请求协议包都是保持一致的。**
  4. 如果是动态资源文件的话，继续执行对应的doPost和doGet，然后返回。如果是静态资源文件，则直接携带对应的cookie返回给浏览器则可。



# 多个Servlet之间的数据共享



## 数据共享：

- OneServlet工作完毕之后，数据交给TwoServlet来使用

### Servlet规范中四种方案：

1. ServletContext接口
2. Cookie类
3. HttpSession接口
4. HttpServletRequest接口



### ServletContext接口：

- 介绍：
  - Servlet规范中的一个接口，Tomcat中存在servlet-api.jar，Tomcat中负责提供这个接口实现类
  - 如果两个Servlet来自于同一个网站，彼此通过ServletContext实例对象实现数据共享
  - 习惯于称为**全局作用域对象**
- 工作原理：
  - 每个网站都存在一个全局作用域对象。
  - 当前网站中其他Servlet都可以从这个对象中取到对应的数据进行使用。
  - 相当于一个Map。
- 概念图：

![image-20210411211645099](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210411211645099.png)

- 全局作用域对象生命周期：

  - Http服务器启动过程中，自动对于当前网站在内存中创建一个全局作用域对象。
  - Http服务器运行期间，一个网站只有一个全局作用域对象。
  - Http服务器运行期间，一直存活
  - Http服务器准备关闭的时候，当前网站中的全局作用域对象进行销毁处理。
  - 全局作用域对象生命周期贯穿网站整个运行期间

- 命令实现：

  1. 

  - 同一个网站中，OneServlet将数据共享给TwoServlet
  - 通过请求对象，向Tomcat索要全局作用对象：`ServletContext application = request.getServletContext()`
  - application默认用法

  2. 

  - `application.setAttribute("key1",任意类型数据)`
  - 这是用于设置对象

  3. 

  - 得到指定的数据
  - `Object data = application.getAttribute("key1")`
  - 这里的Object是可以换成`String`,`Integer`之类的类型
  - 这里可以用于预防空指针异常。

- 占用的是服务端的一块儿内存，很珍贵，存的应该是关键数据嗷呜！！！





### Cookie：

- 介绍：
  - 是一个工具类
  - 存在于servlet-api.jar中
  - 如果两个Servlet来自于同一个网站，并且为同一个浏览器提供服务，此时借助cookie实现数据共享。
  - cookie存放的是当前用户的私人信息，用于共享数据的过程中提高服务质量
  - 显示场景：Cookie相当于客户端得到的一个`会员卡`
- 原理图：

![原理图](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210412091227710.png)

这例子真不是我举的，这也是老师举的，我是无辜的。

- 原理：

  - 第一次向MyWeb网站发送请求申请OneServlet
  - 运行过程中，生成一个cookie放在响应头中交还给浏览器
  - 浏览器收到之后，缓存cookie在浏览器缓存
  - 用于通过同一个浏览器再次访问此网站时，浏览器无条件携带之前的cookie，将其写入请求头中发送过去。
  - cookie也是以key-value的形式存在的嗷！！！
  - cookie是存在于请求头，响应头中！！！

- 一个Cookie对象就只有一个key-value如果要多个的话要去构建多个对象，然后全部都要addCookie嗷！！！

- 实现命令：

  - OneServlet:

    - 创建cookie对象：

      `Cookie card = new Cookie("key1","abc");`

      `Cookie card1 = new Cookie("key2","efg");`

    - 这个键值对的key和value都必须要是String

    - **string是不同编程语言中的通用对象，Java中的String会自动转换为浏览器的string**

    - key不能够是中文

    - 把cookie给用户：

      `response.addCookie(card);`

      `response.addCookie(card1);`

    - 多个cookie就多add几次嘛！！！

  - TwoServlet:

    - 这时候请求头有：

      - 请求参数
      - Cookie: key1=abc;key2=efg

    - 读取Cookie数组：

      `Cookie cookieArray[] = request.getCookies();`

    - 循环遍历数据得到每一个cookie的key和value：

      ```java
      for(Cookie card:cookieArray){
          String key = card.getName();
          String value = card.getValue();
      }
      ```

#### 网上订餐会员卡例子：

- 原理图：

![image-20210412094427597](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210412094427597.png)



- Cookie感觉适用于重定向嗷，只有重定向才需要浏览器来发送多次请求，才需要利用cookie来通信。





### Cookie生命周期

#### Cookie的生与死：

- 什么时候死？？？
  1. 默认情况下Cookie对象存放在浏览器的缓存中，浏览器关了之后Cookie默认就被清空了。
  2. 可以设置将接收的Cookie存放在客户端计算机硬盘上嗷！！！同时指定Cookie在硬盘上的存活时间。存活时间范围内，关闭服务器都不会导致Cookie被销毁。存活时间到达时，Cookie自动从硬盘上被删除掉。
- 语法实现：
  - `card.setMaxAge(60);`，存活一分钟，一分钟到了，Cookie直接暴毙，不管有没有缓存哈！！！





### HttpSession接口：

#### Session接口介绍：

- 来自于Servlet规范下的一个接口，存在于Tomcat中servlet-api.jar
- 如果两个Servlet来自于同一个网站，并且为同一个浏览器提供服务，此时借助于HttpSession对象进行数据共享
- 开发人员称为会话作用域对象

#### Session和Cookie的区别：

- Session是接口，Cookie是类

- 区别：

  1. Cookie存放在客户端计算机，HttpSession在服务端计算机中
  2. Cookie对象存储类型只能是String , HttpSession客户以存储任意类型的共享数据Object
  3. 一个Cookie只能存储一个共享数据，HttpSession使用Map存储数据，可以存储任意数量的共享数据
  4. 参照物：
     - Cookie是会员卡，HttpSession是私人保险柜

- 命令实现：

  - 浏览器访问OneServlet
    - 获得会话
    - `HttpSession session = request.getSession();`
    - 将数据添加到个人存储柜：

  `session.setAttribute("key1",共享数据)`

  - 浏览器访问/myWeb/twoServlet

    - 获得会话

    `HttpSession session = request.getSession();`

    - 获得数据

    `Object data = session.getAttribute("key1");`

#### 读取Session中的数据：

```java
Http session = request.getSession();
Enumeration goodsName = session.getAttributeNames();
while(goodNames.hasMoreElements()){
    String goodsName = (String)goodsName.nextElement();
    int goodsNum = (int)session.getAttribute(goodsName);
}
```



#### 其他一些很重要的概念：

- 每一个用户（浏览器），都自带一个Session，服务器是如何认识你的呢？？？
- Tomcat在创建Session对象时，会自动为这个HttpSession对象生成一个唯一编号，Tomcat将编号保存到Cookie对象，推送到浏览器保存，这个Cookie中的东西是固定的。`Cookie: JSESSIONID = ...`
- 后续的访问，Tomcat服务器会根据Cookie和其中的JSESSIONID来判断用户在服务端的私人柜子。保险柜是不会给用户的，最多给用户的就是保险柜的钥匙，也就是Cookie，靠Cookie来分辨保险柜嗷！！！
- 但是上面这个拿Cookie判断柜子这一步，不用俺们自己写，Tomcat都已经帮我们实现啦！！！如





#### getSession(false)的区别：

- getSession()：如果有私人柜，就返回。如果服务端发现你没有Cookie，这个时候就会自动帮你新建一个“保险柜”，再把Cookie返回给你嗷！！！
- getSession(false)：如果有私人柜，就返回。如果没有的话，那就不理你咯，也不给你分配，爪巴。
- 应用场景：
  - 如果用户合法登陆，就用第一个。
  - 如果用户不合法登陆，就用第二个，安全一点。



#### HttpSession销毁时机：

- 用户与HttpSession关联时使用的Cookie只能存放在浏览器缓存中

- 浏览器关闭时，意味着用户与他的HttpSession关系被切断

- 由于Tomcat无法监测浏览器何时关闭，因此在浏览器关闭时，并不会导致浏览器关联的HttpSession进行销毁

- 为了解决这个问题，会为每一个HttpSession对象设置空闲时间（默认是30分钟），如果30分钟你还没来，此时Tomcat就会销毁掉HttpSession

- HttpSession空闲时间手动设置：

  - `/web/WEB-INF/web.xml`

  - ```xml
    <session-config>
    	<session-timeout>5</session-timeout>
    </session-config>
    ```

  - 5代表五分钟嗷呜！！！

- 实际设置的时候需要多部门统一的！！！



### HttpServletRequest接口：

#### 接口介绍：

- 同一个网站中，两个Servlet之间通过**请求转发**的方式进行调用，彼此之间共享一个请求协议包。一个请求协议包只对应一个请求对象，因此Servlet之间共享一个请求对象。
- 请求对象实现Servlet之间数据共享时，开发人员将其称为请求作用域对象。
- 本质上用Map存储，可以存任意类型的数据。

#### 命令实现：

1. 数据添加到attribute属性。

`request.setAttribute("key1"，数据)`

2. 向Tomcat申请调用TwoServlet

`req.getRequestDispatcher("/two").forward(req,response);`

3. TwoServlet依然是同样的处理方法（指的是doGet或者doPost）

`Object data = req.getAttribute("key1");`



#### 一些补充：

- 最后一个Servlet才会返回对应的页面上的数据。
- response不常用，最后需要的时候才去调用，一般都是用request。



# Servlet规范拓展

## ServletContextListener：

- 介绍：
  - 一组来自于Servlet规范下的接口，共有八个接口
  - 监听器的接口需要我们亲自实现的，服务器并没有提供
  - 监听器接口作用于监控对象生命周期变化时刻以及作用域对象共享数据变化时刻
- 作用域对象：
  - 在Servlet中，服务端内存中为两个Servlet之间提供数据共享规范的对象，-> 作用域对象
  - 作用域对象：
    - ServletContext
    - HttpSession
    - HttpServletRequest
    - Cookie不算哦！！！Cookie出身不同，它不在服务端嗷，它是在客户端。
- 开发规范：（三步）
  - 根据监听实际情况来选择对应监听器接口进行实现。
  - 重写监听器接口声明（监听事件处理方法）。
  - 在web.xml文件将监听器接口实现类注册到Http服务器。
- ServletContextListener接口：
  - 作用：通过这个接口合法检测全局作用域对象被初始化时刻以及被销毁时刻
  - 监听器处理方法：
    - public void contextInitiallized() ：全局作用域对象初始化时被调用。（Tomcat启动时）
    - public void contextDestroy()：全部作用域对象被销毁。（Tomcat结束时）

- 实现：
  - 首先，相应的监听器的实现要放在listener这个包里面。
  - 实现相应的接口

```java
public class OneListener implements ServletContextListener{
    @Override
    public void contextInitialized(ServletContextEvent sce){
        ...
    }
    
    @Override
    public void contextDestroyed(ServletContextEvent sce){
        ...
    }
    
}
```

- web.xml中注册：

```xml
<listener>
	<listener-class>com.liututu.OneListener</listener-class>
</listener>
```



- summary:
  - 有点像生命周期钩子

## ServletContextAttributeListener:

- 介绍：
  - 对于全局作用域中共享数据变化时刻进行监控
  - 方法：
    - public void contextAdd()：在添加共享数据
    - public void contextReplaced()：更新数据
    - public void contextRemove()：删除数据
  - 对应变化时刻：

![image-20210414090513234](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210414090513234.png)





## 监听器作用：

- JDBC中Connection的创建和销毁最浪费时间，那就是说，如果不创建Connection，时间会陡然下降。
- 解决方法：在Tomcat启动的时候，就先生成一批Connection，给有需要的用户使用。用户用完了也不要销毁，保存下来给下一个有需求的用户使用嗷！！！
- 这个时候就要用到监听器，Tomcat启动的时候生成一定数量的Connection，Tomcat结束时销毁这些Connection即可。
- 实例应用：

![image-20210414170512231](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210414170512231.png)



![image-20210414170808450](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210414170808450.png)



## Filter接口：

- 介绍：
  - 来自于Servlet规范下接口。
  - 接口实现类由开发人员提供，Http服务器不负责提供
  - Filter接口在Http服务器调用资源文件之前，对于Http服务器进行拦截
- 作用：
  - 帮助Http服务器检测当前请求合法性。
  - 拦截Http服务器，对于当前请求进行增强操作。
- 开发步骤（三步）：
  - 创建Java类实现Filter接口
  - 重写Filter接口中的doFilter方法(注意这里的Filter有好几个，你要用的是javax中的那个嗷！！！)
  - web.xml文件中过滤器接口实现类注册到Http服务器



### 过滤器接口检测请求合法性：

- 实例：

![image-20210414174642373](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210414174642373.png)

如果检测没有问题，Filter就将request和response返回给Tomcat让其继续执行。如果失败，就不还了，Filter直接代替Tomcat服务器发回不通过的信息。

注册：

```xml
<filter>
	<filter-name>oneFilter</filter-name>
    <filter-class>com.liututu.filter.OneFilter</filter-class>
</filter>
```

和Listener一样，Filter也要单独用一个文件夹来存哦！！！



### 过滤器对于拦截的请求进行增强：

- 第一个`doPost里面总要有个req.setCharacterEncoding("utf-8")`，这样就显得有些麻烦，不同的Servlet都要写这一条就很烦。
- 本质上就是过滤器中帮忙进行了一些统一的处理嘛！！！

![image-20210414203023799](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210414203023799.png)

- `<filter-mapping>`就是可以配置过滤器过滤的对象，在什么页面前进行过滤。

![image-20210414203151480](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210414203151480.png)

- 拦截地址格式：

  - ```xml
    <filter-mapping>
    	<filter-name>OneFilter</filter-name>
        <url-pattern>拦截地址</url-pattern>
    </filter-mapping>
    ```

  - 拦截地址通知Tomcat调用何种资源文件之前需要调用OneFIlter过滤进行拦截

  - 类似于`/img/mm.jpg`

  - 实际开发过程中：
    - 要求Tomcat在调用某一个文件夹下所有内容之前，来调用OneFilter拦截。
    - `/img/*`
  - 要求Tomcat在调用任意文件夹下某种类型文件之前拦截：`*.jpg`
  - 调用任意文件时，调用拦截：`/*`



# 防止用户恶意登录：



## Part1:

- 用户在地址栏直接向服务器索要资源文件。
- 如果用户成功登录，就给个用户令牌，如果没有，拒绝提供服务。
- 令牌就是一个信物而已，一般用Session作为用户的令牌哦

![image-20210414205444149](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210414205444149.png)

//登录的时候用`req.getSession()`的方式为用户置办柜子，其实这里可以加入登录验证的，没验证过就不给柜子，验证过了即给柜子

//UserFindServlet就是先读取会话，`req.getSession(false)`，如果你没柜子，不好意思我直接给你重定向到登录页面，不给你进来，如果你登录了那OK，你可以访问。





## Part2:

- 上面这个是有问题的嗷！！！

- 缺点：

  - 增加开发难度，每一个Servlet都要写一次判断用户身份的代码
  - 不能对于静态资源文件进行保护

- 解决方法：加一个过滤器！！！

  - 通知Tomcat在调用任意文件之前都要调用当前过滤器
  - `doFilter`方法，向拦截的请求索要HttpSession

  ```java
  HttpSession session = req.getSession(false);
  if(session == null){
      response.sendRedirect("login_error.html");
      return;
  }else{
      filterChain.doFilter(req,resp);
  };
  ```



![image-20210414212730135](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210414212730135.png)



- 这个有点麻烦，ServletRequest是HttpServletRequest的父类。有些方法没有就会显得很尴尬了。解决方法：
  - 强制转型
  - 然后再调用相关的方法

![image-20210416101825759](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210416101825759.png)



```xml
<filter-mapping>
	<filter-name>OneFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

- 按照上面这样设置有问题，登录界面就出了大问题，因为你连登录界面都访问不了...，全被过滤器拦下来了。你就算填了登录信息，点击了登录，又被拦截器拦下来了orz，你连被分配Session的机会都没有嗷orz。



## Part3:

- 有些资源，类似于用户验证和用户登录相关的资源Tomcat是应该去放行的嗷！
- 判断用户的输入是否有login哇！！！
- 调用请求对象中URI来了解用户访问的资源文件是啥

```java
String uri = request.getRequestURI();
//如果本次请求资源文件和登录相关，无条件放行。

//默认访问或者是与login相关的访问的时候就放行。
if(uri.indexOf("login")!=-1 || "/myWeb/".equals(uri)){
    filterChain.doFilter(servletRequest,servletResponse);
    return;
}

//如果本次请求的是其他资源文件，需要得到用户在服务端的HttpSession。

session = request.getSession(false);

if(session != null){
    filterChain.doFilter(servletRequest,servletResponse);
    return;
}

request.getRequestDispatcher("/login_error.html".forward(servletRequest,servletResponse));
```

- 所以命名很重要嗷，登录和与登录相关的文件应该命名中都要有login。







































# 最新通信流程图：

## 三要素：

- 避免出现404
- 控制浏览器的(post/get)，避免出现405
- 控制浏览器请求参数：超链接/表单域标签











# 一些小的花里胡哨的知识点

## 为啥创建Servlet的时候不要点那个小勾勾？

- 如果不去掉的话，idea不会帮你在配置文件中写入对应的<servlet></servlet>项嗷！！！我们就要自己写嗷！！！
- 去掉了IDEA自动帮我们加上这些配置它不香咩？？？





## 关于类型的问题

- `int a = null`这个是出了问题的
- `Integer a = null`这个没问题！！！因此，`Integer num = (Integer)map.get("key1")`，不用操心程序出错嗷！！！
- 所有高级引用类型都可以赋值给null，基本类型不可以嗷！！！









## 请求不同类型的文件

- 静态文件：源文件名
- 动态资源文件：请求别名



## 数字不失优雅变为字符串

- `123+""`，字符串拼接自动将其转换为字符串。



## 反复设置cookie

- 旧设置的cookie还在，新的cookie如果字段和旧的相同的，就会把旧的cookie给覆盖掉。

## Java中判断字典里有无某个元素

- `Integer goodsNum = (Integer)session.getAttribute(goodsName);`
- 这里如果用int会有空指针异常嗷！！！
- 判断有无：`if(goodsNum == null)`



## 超链接，不是表嗷！！！

`<a href="/myWeb/one?goodsName=兔子"></a>`

这里的点击都是**跳转**！！！点击和表单的action效果一样都是跳转嗷！！！

超链接也可以携带信息哈，这是个思路。

## 集合的使用

- 使用Map的情况多，因为Map的效率十分优良，比List , Set都好得多，一身的优点哦！！！



## 重载！！！

- 工作中也是，修改方法不要在原方法上修改
- 一般使用重载来实现方法的修改嗷！！！
- 开闭原则：类，模块和函数应该对于拓展开放，对于修改关闭。



## 关于Iterator和Map

- `Iterator it = map.KeySet().iterator()`
- KeySet()会拿出所有的键构成一个集合，但是无序，难搞，iterator()可以将其生成一个可迭代对象并进行简单的排序。



## 快捷键

`ctrl+o`可以快捷的选择重写的类的方法。



## 缓存！！！

- 有的时候缓存这个东西很烦人，会保留之前的状态，在file里面有个restart之类的意思的选项，可以清楚缓存重新加载，这个时候就需要我们来手动去清楚缓存了嗷！！！

