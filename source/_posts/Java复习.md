---
title: Java大二上所学内容
date: 2021-02-20 16:05:21
tags: 
	后端


---



# 是Java, 但是由于没有配置图床，有部分内容系统重装后丢失了orz, 有机会再补上吧QAQ

---



<!--more-->



# Java复习:

1. 

```java
double d = 1.2 + 24 / 5;
d = 5.2
```

```java
double d = 1.2 + 24.0 / 5;
d = 6.0
```

- 这里注意一下运算的顺序，先运算除法，导致精确度不同哈！

2. 

![image-20201228162148615](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201228162148615.png)

3. `int n1 = (int) 12.3`
4. ![image-20201228162907239](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201228162907239.png)

5. 短路运算：（三元运算也是短路运算）

![image-20201228163139005](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201228163139005.png)

6. 显示一个字符的Unicode编码，只需将`char`类型直接赋值给`int`类型即可
7. 转义字符挺重要的嗷，本身不算在字符串的长度里面。两个转义字符表示一个`\`，这个时候才算一个字符嗷！就是算字符是按照显示出来的字符来算的。

```java
String s = "ABC\n\u4e2d\u6587"; // 包含6个字符: A, B, C, 换行符, 中, 文
```



8. 多行字符串如何表示，可以用`"""..."""`来表示,还用过

```java
String s = "first line \n"
         + "second line \n"
         + "end";
```

这种形式表示

9. 字符串赋值本质上是引用的过程，和C语言里面的指针一样样的，所有应用类型都是指针的用法。
10. 关于字符和字符串类型的转换，java中字符和字符串是不一样的。

![image-20201229151302807](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201229151302807.png)

双引号被认为是string , 单引号是char , 不允许把char赋值给string ， 但是可以通过技巧去转换（char是可以转换成string的）

![image-20201229151430452](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201229151430452.png)

比如加一个"" ， 这种方式被默认为字符串的拼接，会将后面的char转换为string然后拼接起来。转换时只有string + char有用，char + string也可以有这种效果哦！！！

11. 用`array.length`方法获取数组大小
12. 数组定义`int[] ns = new int[] { 68, 79, 91, 85, 62 };   var ns = new int[] {10,20,30}   int[] ns = {1,2,3,4,5};`
13. 数组也可以指向引用类型比如`String` ， 和上面同理，对于数组元素的重新赋值，本质上也是指向的改变。
14. 读取用户的输入:

```java
	   Scanner scanner = new Scanner(System.in); // 创建Scanner对象
        System.out.print("Input your name: "); // 打印提示
        String name = scanner.nextLine(); // 读取一行输入并获取字符串
        System.out.print("Input your age: "); // 打印提示
        int age = scanner.nextInt(); // 读取一行输入并获取整数
```

15. 像if之类的语句，判断条件是要加括号的，如果后面执行语句不加花括号，就说明执行的语句只有一条。注意哈  , 使用的级联是用`if...else if...else`这种形式。
16. 判断浮点数相等都是用两个相减小于某个数来判断的嗷，因为浮点数不靠谱。`Math.abs(x - 0.1) < 0.00001`
17. 注意`java`语句后面是有 `;`的嗷！！！
18. `==用于判断是否为同一个对象，equals才用于判断是否相等。为了避免比较时为空的情况。、

```java
	    String s1 = null;
        if (s1 != null && s1.equals("hello")) {
            System.out.println("hello");
        }
```

19. `switch`语句还可以匹配字符串。字符串匹配时，是比较“内容相等”。
20. for循环实现数组遍历:

```java
 	    int[] ns = { 1, 4, 9, 16, 25 };
        int sum = 0;
        for (int i=0; i<ns.length; i++) {
            System.out.println("i = " + i + ", ns[i] = " + ns[i]);
            sum = sum + ns[i];
        }
        System.out.println("sum = " + sum);
```

学了太久go语言，对于很多老东西都不记得了。

```java
		int[] ns = { 1, 4, 9, 16, 25 };
        for (int n : ns) {
            System.out.println(n);
        }
// for each方式对于元素进行遍历
```

![image-20201229161311616](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201229161311616.png)

![image-20201229162215008](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201229162215008.png)

![image-20201229162233318](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201229162233318.png)

21. continue语句在跳过本轮循环时，还是会执行for语句中对于计数器的操作的嗷！！！注： 可以用label然后再`break label`这种形式跳出循环的嗷！！！
22. 数组声明时是不能确定数组大小的，但一旦初始化结束后，数组大小就确定了，不能再改动大小了。
23. 数组的排序：

![image-20201229161512921](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201229161512921.png)

必须注意，对数组排序实际上修改了数组本身。像是冒泡排序之类的算法，显然操作的就是数组本身。

23. 注意哦，`java`中包名都是大写字母打头的哦！！！
24. for each实际上提供了一种崭新的思考方式，可以遍历数组中的子数组，甚至可以进行多层的数组嵌套遍历，实现了类似于树的遍历，对于数据进行了有效的处理。
25.  可变参数，可变参数用`类型...`定义，可变参数相当于数组类型, 这里和go语言中是一致的。完全可以把可变参数改写为`String[]`类型。

```java
 public void setNames(String[] names) {
        this.names = names;
    }//又或者是下面那种。但是，调用方需要自己先构造String[]，比较麻烦。而且可能调用方可以传入null导致程序出现问题。
 public void setNames(String... names) {
        this.names = names;
    }
```

26. 在本语言中，传值和传址体现在是不是引用对象，如果是引用对象就相当于传址操作，引用同一个类型的实例。
27. 构造方法实例：

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person("Xiao Ming", 15);
        System.out.println(p.getName());
        System.out.println(p.getAge());
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}

```

由于构造方法是如此特殊，所以构造方法的名称就是类名。构造方法的参数没有限制，在方法内部，也可以编写任意语句。但是，和普通方法相比，构造方法没有*返回值（也没有`void`）*，调用构造方法，必须用`new`操作符。要特别注意的是，如果我们自定义了一个构造方法，那么，编译器就不再自动创建默认构造方法。

一个构造方法可以调用其他构造方法，这样做的目的是便于代码复用。调用其他构造方法的语法是`this(…)`

28. 构造方法执行顺序，期中出的阴间题目。

```java
在Java中，创建对象实例的时候，按照如下顺序进行初始化：

//1. 先初始化字段，例如，int age = 10;表示字段初始化为10，double salary;表示字段默认初始化为0，String name;表示引用类型字段默认初始化为null；(所以这里就可能会给你插入一条打印输出的语句来考你orz)

//2. 执行构造方法的代码进行初始化。

//因此，构造方法的代码由于后运行，所以，new Person("Xiao Ming", 12)的字段值最终由构造方法的代码确定。
```

29. 初始化执行顺序问题：[初始化课件](C:\Users\Alexander Liu\OneDrive\桌面\课件\大二上课件\软件工程实践\软件工程部分\第4章补充 构造函数调用顺序.pdf)

30. 在Java中，任何`class`的构造方法，第一行语句必须是调用父类的构造方法。如果没有明确地调用父类的构造方法，编译器会帮我们自动加一句`super(); 注意，自动加的是super() , 如果超类没有空构造方法就会报错嗷！！！

31. 为了避免向下转型出错，Java提供了`instanceof`操作符，可以先判断一个实例究竟是不是某种类型：

    ```java
    Person p = new Person();
    System.out.println(p instanceof Person); // true
    System.out.println(p instanceof Student); // false
    
    Student s = new Student();
    System.out.println(s instanceof Person); // true
    System.out.println(s instanceof Student); // true
    Person n = s;
    Student s2 = (Student) n;
    
    Student n = null;
    System.out.println(n instanceof Student); // false
    ```

一般用法如下:

```java
Person p = new Student();
if (p instanceof Student) {
    // 只有判断成功才会向下转型:
    Student s = (Student) p; // 一定会成功
}
```

31. 多态: 多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。

    假设我们编写这样一个方法：

    ```
    public void runTwice(Person p) {
        p.run();
        p.run();
    }
    ```

    它传入的参数类型是`Person`，我们是无法知道传入的参数实际类型究竟是`Person`，还是`Student`，还是`Person`的其他子类，因此，也无法确定调用的是不是`Person`类定义的`run()`方法。

32. 多态的特性就是，运行期才能动态决定调用的子类方法。对某个类型调用某个方法，执行的实际方法可能是某个子类的覆写方法。

33. 如果一个父类不允许子类对它的某个方法进行覆写，可以把该方法标记为`final`。如果一个类不希望任何其他类继承自它，那么可以把这个类本身标记为`final`。用`final`修饰的类不能被继承！！！

```java
abstract class Person {
    public abstract void run(); // 注意，方法中也要加上abstract表示是抽象方法，接口中就不用添加。
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}

```

31. 使用`abstract`修饰的类就是抽象类。我们无法实例化一个抽象类

32. 在Java中，使用`interface`可以声明一个接口：

    ```java
    interface Person {
        void run();
        String getName();
    } // 这里不用加上abstract嗷！！！
    ```

因为接口定义的所有方法默认都是`public abstract`的，所以这两个修饰符不需要写出来（写不写效果都一样）。

33. 在Java中，一个类只能继承自另一个类，不能从多个类继承。但是，一个类可以实现多个`interface`

```java
class Student implements Person, Hello { // 实现了两个interface
    ...
}
```

33. 一个`interface`可以继承自另一个`interface`。`interface`继承自`interface`使用`extends`，它相当于扩展了接口的方法。
34. ![image-20201231110854275](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20201231110854275.png)

34. 在使用的时候，*实例化的对象永远只能是某个具体的子类*，但总是通过接口去引用它，因为接口比抽象类更抽象：

    ```java
    List list = new ArrayList(); // 用List接口引用具体子类的实例
    Collection coll = list; // 向上转型为Collection接口
    Iterable it = coll; // 向上转型为Iterable接口
    ```

35. 实现类可以不必覆写`default`方法。`default`方法的目的是，当我们需要给接口新增一个方法时，会涉及到修改全部子类。如果新增的是`default`方法，那么子类就不必全部修改，只需要在需要覆写的地方去覆写新增方法。

    `default`方法和抽象类的普通方法是有所不同的。因为`interface`没有字段，*`default`方法无法访问字段，而抽象类的普通方法可以访问实例字段*。

36. 因此，不推荐用`实例变量.静态字段`去访问静态字段，因为在Java程序中，实例对象并没有静态字段。在代码中，实例对象能访问静态字段只是因为编译器可以根据实例类型自动转换为`类名.静态字段`来访问静态对象。

37. 静态方法也经常用于辅助方法。注意到Java程序的入口`main()`也是静态方法。注意一下，静态方法这玩意儿挺鬼的，由于其生命周期和作用域，静态方法一般智能访问静态变量。

38. `interface`是可以有静态字段的，并且静态字段必须为`final`类型：

    ```java
    public interface Person {
        public static final int MALE = 1;
        public static final int FEMALE = 2;
    }
    ```

实际上，因为`interface`的字段只能是`public static final`类型，所以我们可以把这些修饰符都去掉，上述代码可以简写为：

```java
public interface Person {
    // 编译器会自动加上public statc final:
    int MALE = 1;
    int FEMALE = 2;
}
```

编译器会自动把该字段变为`public static final`类型。

39. 还有一种`import static`的语法，它可以导入一个类的静态字段和静态方法

40. 因此，编写class的时候，编译器会自动帮我们做两个import动作：

    - 默认自动`import`当前`package`的其他`class`；
    - 默认自动`import java.lang.*`。

     注意：自动导入的是`java.lang`包，但类似`java.lang.reflect`这些包仍需要手动导入。

41. 定义为`public`的`field`、`method`可以被其他类访问，前提是首先有访问`class`的权限，如果没有权限的话，连类都访问不了，更加别提方法和属性了。Java支持嵌套类，如果一个类内部还定义了嵌套类，那么，嵌套类拥有访问`private`的权限。定义为`protected`的字段和方法可以被子类访问，以及子类的子类。包作用域是指一个类允许访问同一个`package`的没有`public`、`private`修饰的`class`，以及没有`public`、`protected`、`private`修饰的字段和方法。

42. 包名必须完全一致，包没有父子关系，`com.apache`和`com.apache.abc`是不同的包。

43. Java内建的访问权限包括`public`、`protected`、`private`和`package`权限。一个`.java`文件只能包含一个`public`类，但可以包含多个非`public`类。如果有`public`类，文件名必须和`public`类的名字相同。

44. 它与普通类有个最大的不同，就是Inner Class的实例不能单独存在，必须依附于一个Outer Class的实例。内部类就有点阴间...

```java
public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer("Nested"); // 实例化一个Outer
        Outer.Inner inner = outer.new Inner(); // 实例化一个Inner
        inner.hello();
    }
}

class Outer {
    private String name;

    Outer(String name) {
        this.name = name;
    }

    class Inner {
        void hello() {
            System.out.println("Hello, " + Outer.this.name);
        }
    }
}

```

45. 除了能引用Outer实例外，还有一个额外的“特权”，就是可以修改Outer Class的`private`字段，因为Inner Class的作用域在Outer Class内部，所以能访问Outer Class的`private`字段和方法。
46. Anonymous Class是内部类的其中一种，它不需要在Outer Class中明确地定义这个Class，而是在方法内部，通过匿名类（Anonymous Class）来定义。

```java
public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer("Nested");
        outer.asyncHello();
    }
}

class Outer {
    private String name;

    Outer(String name) {
        this.name = name;
    }

    void asyncHello() {
        Runnable r = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello, " + Outer.this.name);
            }
        };
        new Thread(r).start();
    }
}


//定义匿名类的写法如下：
//Runnable r = new Runnable() {
    // 实现必要的抽象方法...
//};
```

47. ||和|都是表示“或”，区别是||只要满足第一个条件，后面的条件就不再判断，而|要对所有的条件进行判断。也就是说"|"并不存在短路机制。

48. 在计算机中，我们把一个任务称为一个进程，浏览器就是一个进程，视频播放器是另一个进程，类似的，音乐播放器和Word都是进程。进程和线程的关系就是：一个进程可以包含一个或多个线程，但至少会有一个线程。操作系统调度的最小任务单位其实不是进程，而是线程。

49. 多线程专门开一部分嗷：

    - 线程的创建：

      - ```java
        public class Main {
            public static void main(String[] args) {
                Thread t = new MyThread();
                t.start(); // 启动新线程
            }
        }
        
        class MyThread extends Thread {
            @Override
            public void run() {
                System.out.println("start new thread!");
            }
        }
        ```

      - ```java
        public class Main {
            public static void main(String[] args) {
                Thread t = new Thread(new MyRunnable());
                t.start(); // 启动新线程
            }
        }
        
        class MyRunnable implements Runnable {
            @Override
            public void run() {
                System.out.println("start new thread!");
            }
        }
        //要特别注意：直接调用Thread实例的run()方法是无效的：必须调用Thread实例的start()方法才能启动新线程
        ```

    - 线程的状态：

      - New：新创建的线程，尚未执行；

      - Runnable：运行中的线程，正在执行`run()`方法的Java代码；

      - Blocked：运行中的线程，因为某些操作被阻塞而挂起；

      - Waiting：运行中的线程，因为某些操作在等待中；

      - Timed Waiting：运行中的线程，因为执行`sleep()`方法正在计时等待；

      - Terminated：线程已终止，因为`run()`方法执行完毕。

        当线程启动后，它可以在`Runnable`、`Blocked`、`Waiting`和`Timed Waiting`这几个状态之间切换，直到最后变成`Terminated`状态，线程终止。

        一个线程还可以等待另一个线程直到其运行结束。例如，`main`线程在启动`t`线程后，可以通过`t.join()`等待`t`线程结束后再继续运行

    - 中断线程：

      - 中断一个线程非常简单，只需要在其他线程中对目标线程调用`interrupt()`方法，目标线程需要反复检测自身状态是否是interrupted状态，如果是，就立刻结束运行。
      - 仔细看上述代码，`main`线程通过调用`t.interrupt()`方法中断`t`线程，但是要注意，`interrupt()`方法仅仅向`t`线程发出了“中断请求”，至于`t`线程是否能立刻响应，要看具体代码。而`t`线程的`while`循环会检测`isInterrupted()`，所以上述代码能正确响应`interrupt()`请求，使得自身立刻结束运行`run()`方法。

    - 守护线程：

      - 守护线程是指为其他线程服务的线程。在JVM中，所有非守护线程都执行完毕后，无论有没有守护线程，虚拟机都会自动退出。

      - 如何创建守护线程呢？方法和普通线程一样，只是在调用`start()`方法前，调用`setDaemon(true)`把该线程标记为守护线程：

        ```java
        Thread t = new MyThread();
        t.setDaemon(true);
        t.start();
        ```

    - 线程同步：

      - 这说明多线程模型下，要保证逻辑正确，对共享变量进行读写时，必须保证一组指令以原子方式执行：即某一个线程执行时，其他线程必须等待

      - 种加锁和解锁之间的代码块我们称之为临界区（Critical Section），任何时候临界区最多只有一个线程能执行。

        - ```java
          synchronized(lock) {
              n = n + 1;
          }
          ```

      - `synchronized`保证了代码块在任意时刻最多只有一个线程能执行。我们把上面的代码用`synchronized`改写如下：

        - ```java
          public class Main {
              public static void main(String[] args) throws Exception {
                  var add = new AddThread();
                  var dec = new DecThread();
                  add.start();
                  dec.start();
                  add.join();
                  dec.join();
                  System.out.println(Counter.count);
              }
          }
          
          class Counter {
              public static final Object lock = new Object();
              public static int count = 0;
          }
          
          class AddThread extends Thread {
              public void run() {
                  for (int i=0; i<10000; i++) {
                      synchronized(Counter.lock) {
                          Counter.count += 1;
                      }
                  }
              }
          }
          
          class DecThread extends Thread {
              public void run() {
                  for (int i=0; i<10000; i++) {
                      synchronized(Counter.lock) {
                          Counter.count -= 1;
                      }
                  }
              }
          }
          
          ```

        - ```java
          synchronized(Counter.lock) { // 获取锁
              ...
          } // 释放锁
          ```

        - 它表示用`Counter.lock`实例作为锁，两个线程在执行各自的`synchronized(Counter.lock) { ... }`代码块时，必须先获得锁，才能进入代码块进行。执行结束后，在`synchronized`语句块结束会自动释放锁。这样一来，对`Counter.count`变量进行读写就不可能同时进行。上述代码无论运行多少次，最终结果都是0。

          使用`synchronized`解决了多线程同步访问共享变量的正确性问题。但是，它的缺点是带来了性能下降。因为`synchronized`代码块无法并发执行。此外，加锁和解锁需要消耗一定的时间，所以，`synchronized`会降低程序的执行效率。

          我们来概括一下如何使用`synchronized`：

          1. 找出修改共享变量的线程代码块；
          2. 选择一个*共享实例*作为锁；
          3. 使用`synchronized(lockObject) { ... }`。

        - 上述代码的4个线程对两个共享变量分别进行读写操作，但是使用的锁都是`Counter.lock`这一个对象，这就造成了原本可以并发执行的`Counter.studentCount += 1`和`Counter.teacherCount += 1`，现在无法并发执行了，执行效率大大降低。实际上，需要同步的线程可以分成两组：`AddStudentThread`和`DecStudentThread`，`AddTeacherThread`和`DecTeacherThread`，组之间不存在竞争，因此，应该使用两个不同的锁

        - 单条原子操作的语句不需要同步。赋值，引用都是原子操作嗷！！！

      - 思考：锁的本质是什么，是Object，还是可以为其他的东西呢？（本质就是Object）

      - 锁可以在哪里上呢？第一个反应是可以在run函数里面上锁，确保一次只能被一个线程访问。第二个是可以在定义方法的时候就上锁，保证线程本身就是安全的。

      ```java
      public void add(int n) {
          synchronized(this) { // 锁住this
              count += n;
          } // 解锁
      } // 这种写法和下面的是等价的
      ```

      ```java
      public synchronized void add(int n) { // 锁住this
          count += n;
      } // 解锁
      ```

      因此，用`synchronized`修饰的方法就是同步方法，它表示整个方法都必须用`this`实例加锁。

      - 只读不写不需要同步哦！！！如果一个类被设计为允许多线程正确访问，我们就说这个类就是“线程安全”的（thread-safe），上面的`Counter`类就是线程安全的。Java标准库的`java.lang.StringBuffer`也是线程安全的。
      
    - 死锁：

      - 那么我们应该如何避免死锁呢？答案是：线程获取锁的顺序要一致。即严格按照先获取`lockA`，再获取`lockB`的顺序，改写`dec()`方法如下

    - `wait`和`notify`:

      - 使用情况：

        - 如果深入思考一下，我们想要的执行效果是：
          - 线程1可以调用`addTask()`不断往队列中添加任务；
          - 线程2可以调用`getTask()`从队列中获取任务。如果队列为空，则`getTask()`应该等待，直到队列中至少有一个任务时再返回。

      - 对于上述`TaskQueue`，我们先改造`getTask()`方法，在条件不满足时，线程进入等待状态：

        ```java
        public synchronized String getTask() {
            while (queue.isEmpty()) {
                this.wait();
            }
            return queue.remove();
        }
        ```

        - 这里的关键是：`wait()`方法必须在当前获取的锁对象上调用，这里获取的是`this`锁，因此调用`this.wait()`。
        - 必须在`synchronized`块中才能调用`wait()`方法，因为`wait()`方法调用时，会*释放*线程获得的锁，`wait()`方法返回后，线程又会重新试图获得锁。（wait方法会释放锁！！！）

      - 如何让等待的线程被重新唤醒，然后从`wait()`方法返回？答案是在相同的锁对象上调用`notify()`方法。

        ```java
        public synchronized void addTask(String s) {
            this.queue.add(s);
            this.notify(); // 唤醒在this锁等待的线程
        }
        ```

      - 内部调用了`this.notifyAll()`而不是`this.notify()`，使用`notifyAll()`将唤醒所有当前正在`this`锁等待的线程，而`notify()`只会唤醒其中一个（具体哪个依赖操作系统，有一定的随机性）。这是因为可能有多个线程正在`getTask()`方法内部的`wait()`中等待，使用`notifyAll()`将一次性全部唤醒。通常来说，`notifyAll()`更安全。有些时候，如果我们的代码逻辑考虑不周，用`notify()`会导致只唤醒了一个线程，而其他线程可能永远等待下去醒不过来了。

      - ```java
        public synchronized String getTask() throws InterruptedException {
            if (queue.isEmpty()) {
                this.wait();
            }
            return queue.remove();
        }
        ```

        这种写法实际上是错误的，因为线程被唤醒时，需要再次获取`this`锁。多个线程被唤醒后，只有一个线程能获取`this`锁，此刻，该线程执行`queue.remove()`可以获取到队列的元素，然而，剩下的线程如果获取`this`锁后执行`queue.remove()`，此刻队列可能已经没有任何元素了，所以，要始终在`while`循环中`wait()`，并且每次被唤醒后拿到`this`锁就必须再次判断：

        ```java
        while (queue.isEmpty()) {
            this.wait();
        } // while + wait 哦，注意一下
        ```
        
    
50. 在Java字符串中需要用`\\`表示一个`\`






# 复习课件摘要：

---

## 第二章-第三章：

- 值变量在栈中，声明值变量的时候，其实已经在堆中分配好内存了，因为值对象大小确定。

- 但是引用类型的变量在堆中，声明引用类型变量的时候，其实是在栈中分配了一个空间给引用变量，这个时候还没有给对象分配具体的内存。具体的对象是在new或者初始化的时候才在堆中分配好的。`举个例子，String为引用，当你声明String a的时候，其实是还没有给a分配具体的堆空间，只是在栈内存中给a这个变量分配了空间，在a = "abc"的时候，才实实在在的在堆中分配了空间给abc，并将a作为了指针。`这也是为什么引用对象的初始值为null , 而值类型的初始值为它本身的初始值（如0）一样。有很多特殊用法，如Integer可以指示没有分配内存，和分配了内存但是为0两种状态，而int则不能够表示这两种状态嗷！！！

- 直接用赋值语句赋值数组，赋值赋的是数组的首地址。

- 方法区：静态存储分配，编译时就能确定存储空间大小，为他们分配固定的内存空间

- 栈内存：动态分配，方法参数和局部变量只能时八大基本数据类型或者对象的引用。

- 堆内存：对应方法区或运行时的模块入口处。有自动垃圾回收机制。栈不需要自动垃圾回收机制哦，因为结束了某个部分，栈自动就把这个部分的数据全部pop出来了。栈的优势是可以动态分配内存大小。

- 多维数组：

  - ![image-20210113103400465](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113103400465.png)
  - 数组初始化三种形式：

  ![image-20210113103507212](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113103507212.png)

  - 字符串的一些功能： `toUpperCase`，`length`，`copy`
  - 创建一个字符串默认为"" `String str = new String()或者String str = ""`都可以，后者有可能不会再创建，JVM会检查字符串池看看有没有字符串，有的话就让str指向它，体现了数据共享的有点。
  -  字符串比较：`equals   equalsIgnoreCase compareTo compareToIgnoreCase`
  - ![image-20210113104607782](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113104607782.png)
  - `toCharArray方法`字符串转换为数组 ， `valueOf方法`数组转换为字符串 ， `toString()`将任何对象转换为字符串。
  - ![image-20210113104836780](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113104836780.png)

  字符串中的内容是不可改变的，可以改变的其实也是分配了一块儿新的空间再写入新的内容。类似于String和Integer等封装类型本身被设计为final嗷！！！，就是本质上不允许修改的！！！

  - Java中**没有地址传递**，只有值传递这一种策略。传地址本质也是传值，实现机制是"栈-堆"二元结构。

  ---

## 第四章：

  - ![image-20210113112251233](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113112251233.png)

  这里注意一下，仅仅在类被加载的时候执行一次，往后对象加载时是不会执行静态代码块的，相当于对于类中的某些元素进行了初始化（针对于静态变量）

  - ![image-20210113112600622](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113112600622.png)

  - ![image-20210113112719547](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113112719547.png)

    ![image-20210113112803196](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113112803196.png)

  - ![image-20210113112925930](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113112925930.png)

    ![image-20210113112940423](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113112940423.png)

    这样可以保证，我们传入的某些对象或者是值在使用过程中不会被修改（或修改指向）

  - 一定要注意，对于值来说就是值得初始化，对于对象来说就是指向的初始化，不代表对象的值不能改变哦！

  - ![image-20210113113250096](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113113250096.png)

    注意一下，就是实现了常量的功能，但是本质和常量有区别。尤其注意一下方法区和栈内存！！！

  - 内部类的类定义一定要在另外一个类的内部。

  - 内部类不同层级的访问：

![image-20210113144811237](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113144811237.png)

this.age代表的是this这个对象的内部成员age，就不是临时变量了

outer.this.age代表的是外部类的age成员变量，是外部的age成员

this其实就可以翻译为此实例对象。

- Java具有单一继承的特性

- 类前面的访问控制只能是public和缺省两种，其中一个.java文件中只能有一个public

- ![image-20210113151300226](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113151300226.png)

- 比较两个对象是否相同用`.equals`方法哦

- 方法覆盖的时候，覆盖方法的访问控制不能够比被覆盖方法更加严格

- ![image-20210113151750510](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113151750510.png)

  **返回值不能够作为判断方法之间是否重载的依据**，一定要是方法的参数类型，或者参数个数，或者参数顺序发生了改变。JVM是根据参数的不同来判断不同的方法的。
  
  ![image-20210113152245075](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113152245075.png)

- ![image-20210113152049721](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113152049721.png)
- ![image-20210113152309441](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113152309441.png)

---

## 第五章：

- 多态只针对于方法
- 多态分类：

![image-20210113153802150](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113153802150.png)

编译时多态在编译时就能确定执行哪一个

运行时多态在运行的时候才能决定运行哪一个，常见的有父类引用子类，然后只有运行时调用方法才可以体现。

- ![image-20210113154118379](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113154118379.png)

- instanceof判断实例，很好用的哦！！！

---



## 第六章（异常处理）：



- 无非就是try ... catch ...嘛
- try中如果捕捉到了错误，就不再往下执行，转去执行catch中的代码段，如果遇到了return , 就先执行finally再return。这种情况下，finally之后的代码时不会执行的
- 但是如果catch中没有退出，那么就跳过有问题的语句，执行catch , finally 然后会执行finally往后的语句
- 总之，finally和它之下的代码块的区别，finally只要发生了异常一定会执行嗷！！！

---

## 第七章：

- 多线程的创建：

  - `class MyThread extends Thread`,然后又覆写`public void run()`方法

  ```java
  MyThread r = new MyThread();
  r.start();
  ```

  

  - ```java
    class ThreadRun implements Runnable{
    	@Override
        public void run(){
            ...
        }
    }
    
    class TestRunnable{
        public static void main(){
            ThreadRun r = new ThreadRun();
            Thread t = new Thread(r);
            t.start();
        }
    }
    
    //public static void main(string[] args){
    
    //}
    ```

- 线程及调度：

  - 状态： 新建 ， 就绪 ， 运行 ， 阻塞 ， 死亡

---

## 网络编程：

- TCP/IP
- Socket 和 ServerSocket



---

## 第九章：

- 集合框架：
  - 对外的接口
  - 接口的实现
  - 对集合处理的算法

---

## 第十章：

- 字节流以字节为单位，字符流以字符为单位

- 分类：

  - 节点流
  - 处理流

- ![image-20210113171050531](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113171050531.png)

- ![image-20210113171342447](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113171342447.png)

- ![image-20210113171439808](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113171439808.png)

  凡是实现了ObjectInputStream的都是装饰流(处理流)

- 嵌套`DataOutputStream = new DataOutputStream(new FileOutputStream(filename))`

- ![image-20210113171640142](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113171640142.png)

  将数据存取于缓冲区，提高输入输出数据的速度

  常见用法：

  ![image-20210113171752237](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113171752237.png)

  相当于在流之间加了一层Buffer ， 有奇效哦！

- ![image-20210113172008503](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113172008503.png)

- ![image-20210113172457397](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113172457397.png)

- 目前看了这么多只有ByteArrayInputStream和FileInputStream为结点流，其他的虽然有什么data啊之类的，依旧逃离不了处理流的魔爪。

- 字节输入输出流之间并不是一一对应的关系哦！！！

- ![image-20210113172723862](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113172723862.png)

- ![image-20210113172759614](C:\Users\Alexander Liu\AppData\Roaming\Typora\typora-user-images\image-20210113172759614.png)

- Reader和InputStream都是**抽象类！！！**

- InputStreamReader和OutputStreamWriter是桥梁，将两者联系起来。注意一下嗷，`InputStreamReader`是也是装饰流，装在`InputStream`之外的，让其变为好处理的字符。

- 一样的，如果读取或者写入的内容很多的话，要考虑换成更为高效的缓冲流`BufferedReader   BufferedWriter`,一样的是装饰流，所以要套用在节点流外、

- 除了`CharArrayReader   StringReader`之外，还有`FileReader`是节点流

  




