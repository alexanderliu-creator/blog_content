---
title: 初识Java反射机制
date: 2021-03-13 19:10:26
tags: 后端
categories: 动力结点后端课程
---





# 由于JDBC的学习过程中，用到了反射，就去粗浅看了看反射的原理，笔记如下！！！



<!--more-->





[网课bilibili资源](https://www.bilibili.com/video/BV1C4411373T)

# 反射基本概念

## 反射三阶段

![image-20210311185113030](https://i.loli.net/2021/03/13/9FweQxUuaSnrdXl.png)

- 第一部分在硬盘上，先进行了编译的过程。（保存代码Idea就自动编译好了）
- 然后通过类加载器`ClassLoader`，将编译好的`Person.class`，读取到内存中，此时为第二个阶段，Class类对象阶段。里面将原来代码中的成员变量，构造方法和成员方法分别表示出来。
- 当调用构造方法的时候，从内存中构造出相关的Person对象，这是才到了第三个部分，Runtime运行时阶段。



## 反射好处：

- 可以在程序的运行过程中，操作这些对象
- 可以解耦合，提高可拓展性。





# API操作

- 获取class类的对象：

  - `Class.forName("全类名")`，这种方式是将字节码文件加载进入内存，返回Class对象，这是在第一阶段。
    - 这种方法多用于**配置文件**，将类名定义在配置文件中。读取文件，加载类。
  - `类名.class`，通过类名的属性来获取，这是在第二阶段。
    - 多用于**参数传递**
  - `对象.getClass()`在，所有对象都有这种方法！！！
    - 多用于**对象**的获取字节码的方式

- 这里注意一下，Idea中保存运行中，java文件会自动编译，所有一开始是有字节码的。

- 结论：同一个字节码文件，它在一次程序的运行中，只会被加载一次。只是在运行的不同过程被

- 使用Class对象：

  - 大部分是获取功能！！！
  - 详细获取功能：
    - 过去成员变量们（Field）
      - `Field[] getFields()`   #获取所有public修饰的成员变量
      - `Field getField(String name)`
      - `Field[] getDeclaredFields()`   #获取所有的成员变量
      - `Field getDeclaredField(String name)` 
    - 获取构造方法们（Constructor）
      - `Constructor<?>[] getConstructors()`
      - `Constructor<T> getConstructor(类<?>... parameterTypes)`
      - 还有类似于上面的
    - 获取成员变量们（Method)
      - `Method[] getMethods()`
      - 类似于上面的
    - 获取类名（）
      - `String getName()`

- Field成员变量

  - 操作：

    - 设置值
      - set
    - 获取值
      - get

- 一些操作例子(Field)

  - ```java
    Class personClass = Person.class;
    Field a = personClass.getField("a");
    Person p = new Person;
    Object value = a.get(p);
    //下面这个是设置！！！
    //1.首先获得了Class对象
    //2.由Class对象获取Field对象
    //3.通过Field对象，对于特定的构造出来的对象进行设置或者读取操作。
    a.set(p,"张三")
    ```

  - ```java
    Field d = personClass.getDeclaredField("d");
    Object values = d.get(p);
    //私有的利用反射也不访问的嗷QAQ！！！
    //但是可以“暴力反射”。忽略安全检查！！！
    d.setAccessible(true);
    Object values = d.get(p);
    ```

  - 这里其实可以先康康有哪些Field，然后再去获取Field进行类的操作！！！

- 一些操作例子(构造器)

  - ```java
    Class personClass = Person.class;
    
    //由于不同构造器传入值不同，所以这里要给定参数，在第二部分的位置，所有东西都是以class存在的，因此要得到相应的构造器，就需要传入对应的.class。
    Constructor constructor = personClass.getConstructor(String.class,int.class)
    ```

  - ```java
    //可以用于创建对象
    Object person = constructor.newInstance("兔兔",20);
    //这样就能够获取到构造器
    //同时获取空参的构造器，我们可以简化操作
    Object person2 = personClass.newInstance();
    //和上面一样的，若调用私有的构造器，也需要
    constructor.setAccessible(true);
    ```

- 一些操作例子(操作方法)

  - ```java
    Class personClass = Person.class;
    personClass.getMethod();
    //方法三要素：方法名，返回值列表，参数列表
    //确定方法：方法名，参数列表
    //方法名一样，参数列表不一样能构成重载关系
    Method eat_method = personClass.getMethod("eat");
    Person p = new Person();
    eat_method.invoke(p); //执行eat方法
    ```

  - ```java
    Method eat_method2 = personClass.getMethod("eat",String.class);
    Person p = new Person();
    
    eat_method2.invoke(p,"饭");
    ```

  - ```java
    Method[] methods = personClass.getMethods();
    for (Method method : methods){
    	System.out.println(method);
        String name = method.getName();//获取方法的名称
        System.out.println(name);
    }//这个能获取所有的public方法，object的方法也在其中嗷！！！
    ```

- ```java
  //获取全类名
  String className = personClass.getName();
  //这里获得的的是全类名！！！
  ```

- 不管什么私有啊，还是什么修饰符修饰方法，在反射面前都是可以被执行的嗷！！！在反射面前没有什么是隐私！！！





# 案例：写一个框架可以帮我们取创建任意类的对象，并且执行其中的任意方法

- ```properties
  //这儿如果新建一个对象，再调用方法，意义就不大了，所以要用到反射
  //1.配置文件   2.反射
  //1.将需要创建的对象的全类名和执行的方法的定义配置在文件中
  //2.程序读取配置文件
  //3.反射技术加载文件到内存
  //4.创建对象
  //5.执行方法
  
  className = //包名字
  methodName = //方法名
  ```

- ```java
  Properties pro = new Properties();
  
  ClassLoader classLoader = ReflectTest.class.getClassLoader();//获取了类加载器，类加载器把类加载到内存，它就会知道类的位置啊，所以就可以在那个目录下找到properties
  
  InputStream is = classLoader.getResourceAsStream("pro.properties");
  //加载配置文件，转换为一个集合
  pro.load(is);
  
  //获取配置文件中的数据
  String className = pro.getProperty("className");
  String methodName = pro.getPropety("methodName");
  
  
  Class cls = Class.forName(className);  //把对应的class加载进入内存
  Method method = cls.getMethod(methodName);
  //执行方法
  Object obj = cls.newInstance();
  Method method = cls.getMethod(methodName);
  method.invoke(obj);
  ```

- 这个框架就很有意思嗷！！！一个是改代码，一个是改配置文件，反射实现了这种方式，使得程序更加简洁。

- 以后看到了要知道使用了反射嗷！！！