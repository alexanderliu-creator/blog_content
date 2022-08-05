---
title: Java常问面试题总结与提高
date: 2021-08-05 21:38:05
tags: 大三自学
cover: https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20211012103611.png
---



# Java高频考点



![image-20210805214421596](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805214421.png)

![image-20210805214638422](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805214638.png)

![image-20210805214700114](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210805214700.png)







# 1. 局部变量表和计算栈

![image-20210829092912831](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829092913.png)

[非常有意思的一道题目嗷！！！](https://blog.csdn.net/happy_bigqiang/article/details/90414541)

- 知识点在于栈，局部变量表等东西的操作嗷，类似于Println和符号运算这样的操作。本质上，都是先把操作数放在栈里面，然后对于栈里面的数字进行操作的。
- 然后就是++这样的操作，在前在后，对于局部变量表操作和栈操作的顺序是不一样的嗷！！！

# 2. Singleton示例

- 单例，系统中只有一个实例对象可以被获取和使用的代码模式。例如JVM运行环境中的Runtime类

- 要点：

  - 某个类只能由一个实例

    - 构造器私有化

  - 必须自行创建这个实例

    - 静态变量来保证实例唯一

  - 必须向整个系统提供这个实例

    - 向整个系统提供这个实例：

    1. 直接暴露
    2. 静态变量的get方法获取

- 创建类型：

  1. 饿汉式：

  - 直接实例化饿汉式

  ![image-20210829163713857](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829163721.png)

  获取这个对象：

  ![image-20210829164100868](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829164100.png)

  2. JDK1.5之后，枚举类型：

  ![image-20210829163830867](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829163830.png)

  获取这个对象：

  ![image-20210829164212855](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210829164212.png)

  枚举类型会帮助我们重写String方法嗷！

  3. 静态代码块的方式：

  ![image-20210901222143617](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210901222143.png)

  类初始化的时候new处对象，保证对象只在初始化的时候创建一次嗷！！！

  2. 懒汉式：

  



# 3. volatile:



volatile是什么

- 轻量级的同步机制
- 特性：
  - 保证可见性
  - 不保证原子性
  - 禁止指令重排



## JMM：

- JVM（Java虚拟机）
- JMM（Java内存模型）：
  - 并不真实存在，描述的是一组规则或规范
- 几个概念：
  - 主内存：
    - **共享**内存区域，所有线程都可以访问嗷！
  - 工作内存：
    - 每个线程私有的工作内存空间
- 各自线程的工作内存会拷贝主内存中对应的数据，然后各自对于数据进行修改，修改完成之后，才会尝试将数据**写回**主物理内存。
- 只有在工作内存才能够对于变量进行修改嗷！！！不同的线程是不能够访问对方的工作内存嗷！！！工作内存的结果需要写回主内存之后，才能够真正的生效嗷！！！

![image-20210808083346242](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808083346.png)

### 可见性定义：

- 一个线程对于数据进行了修改并且写回了主内存之后，其他的线程要马上能够知道嗷！！！只要有变动，大家都要立马知道嗷！！！
- 第一时间通知到所有线程的过程，这个就叫做可见性。




### 原子性：







## Volatile特性：

![image-20210913190044888](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210913190045.png)

### 可见性：

```java
public class MyData(){
    int number = 0;

	public void addTo60(){
    	this.number = 60;
	}
}
```



```java
public static void main(String[] args){
    MyData myData = new MyData();
    
    new Thread(() -> {
        System.out.println(Thread.currentThread().getName()+"\t come in");
        try{TimeUnit.SECONDS.sleep(3);}catch{...}; //模拟线程处理过程
        myData.addTo60();
    },"AAA").start();
}

while(myData.number == 0){
    //main线程就一直在这里等待嗷！！！
}

System.out.println("number变成了60啦！！！");
```

- 没有volatile就没有多线程的可见性嗷！！！
- 然后你发现不得行！！！主线程仍然是卡住的，因为没有任何线程来通知主线程，这个值发生了改变，主线程自己的工作空间中的值，是没有变化的嗷！！！（默认是没有可见性的嗷！！！）
- 如何实现可见性呢？Volatile！！！


```java
public class MyData(){
    volatile int number = 0;

	public void addTo60(){
    	this.number = 60;
	}
}
```

- 测试和上面的代码是一样的嗷！！！然后发现，volatile其实就是起到了及时通知main线程，某个变量发生了改变的作用。

![image-20210808083024568](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210808083024.png)



### 不保证原子性：

- 原子性是什么？
  - 不可分割，完整性，也即某个线程不能被分割，要整体完成，要么同时成功，要么同时失败。
- 例子：

`valatile i`

`public methodPlus(){i++}`

然后启动多个线程来同时操作（也就是常见的i++的例子）

![image-20210913192205069](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210913192205.png)

![image-20210913192012747](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210913192013.png)

结果不是20000嗷！！！

- 原理解释：
  - 可以在方法上加上sychronized吗？
    - 可以，可以保证20000
    - 但是不好，效率消耗太大了嗷！！！
- 为啥会<2000呀！！！

多线程的调度出问题了嗷！！！

A和B同时读入了0，同时加1。A写入1，还没来得及通知别的线程，B就再次被调度写入了1，这就出问题了orz，就会导致这种情况的出现嗷！！！

- 字节码：

![image-20210913192921349](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210913192921.png)



- 如何解决原子性：

  1. 加入sync，太重了，不用这个
  2. `AutomicInteger automicInteger = new AutomicInteger();`

  用法的话，建议去看看官方文档中怎么写的嗷！！！

  ![image-20210913193550094](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210913193550.png)

  说白了底层为CAS，比起volatile来说，还是加了个轻量级锁orz。



### 禁止指令重排：

- 多线程：

![image-20210914182640288](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914182640.png)

- 重排：

![image-20210913194820605](C:\Users\Alexander\AppData\Roaming\Typora\typora-user-images\image-20210913194820605.png)

- 处理器在进行重排序的时候必须要考虑指令之间的**数据依赖性**
- 由于数据依赖性，指令重排也要考虑数据依赖性哈！！！volatile来决定嗷！！！
- 指令重排多线程下出现，这个重排是客观存在的，两个线程中使用的变量能够保证一致性嗷！！！避免指令重排优化，从而避免多线程环境下程序出现乱序执行的情况。

![image-20210913200926447](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210913200926.png)

- 指令屏障（保证禁止指令重排嗷！！！）：

![image-20210913201108269](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210913201109.png)

禁止指令重排嗷！！！保证了安全性嗷！！！

![指令重排](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210913201109.png)

- volatile的特点：
  - 可见性（强制刷出各种CPU的缓存数据嗷！！！）
  - 禁止指令重排

## 什么时候用Volatile：

### 单例模式：

- 单例模式！！！本来是不用volatile的，但是当单线程 -> 多线程的时候，就会出问题了嗷orz

![image-20210914183438141](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914183439.png)

- 解决方法：
  - getInstance方法前面加上synchronized，没有问题嗷！！！可以正常执行嗷！！！
  - 但是太重了orz



### DCL模式：

- Double Check Lock，双端检锁机制。

![image-20210914183722408](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914183722.png)

对吗？不对！！！多线程，为了指令的效果，会指令重排！！！

- DCL不一定安全，指令重排！！！

![image-20210914184249691](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914184250.png)

这个时候就可能取到null嗷！！！

- 指令重排只保证单线程情况下的指令一致性，多线程则不保证嗷！！！

![image-20210914184531643](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914184532.png)

- 禁止指令重排：`public static volatile SingletonDemo instance = null;`

# 4. CAS：

- Compare And Set，比较并且交换
- Synchronized太重了，CAS就比synchronized轻量哦！！！
- 例如AutomicInteger , `compareAndSet(expect , update)`

## 底层原理：

- why CAS not Synchronized？底层原理：
  - 自旋锁
  - Unsafe
- getAndIncrement底层实现：
  - 调用的JDK自带的unsafe类嗷！！！（都是底层的Native方法嗷！！！）

![image-20210914190803601](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914190803.png)

![image-20210914190825087](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914190825.png)

- Unsafe类：

![image-20210914191643821](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914191644.png)

![image-20210914191909041](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914191909.png)

![image-20210914192041067](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914192042.png)

- getAndAddInt

![image-20210914192335325](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914192335.png)

关键是底层的CAS的靠的是CPU的原子操作来实现的嗷！！！因此这样能够保证原子性嗷！！！读取，比较和设置一气呵成。不存在中间有线程特判比较的这种情况出现嗷！！！

- getAndIncrement：

![image-20210914215818159](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914215818.png)

- 原理：

![image-20210914220153739](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914220153.png)

**Unsafe类+CAS思想（自旋）**

![image-20210914220305279](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914220305.png)

## 缺点：

1. 循环时间长，CPU开销大
2. 只能保证一个变量的原子性操作，如果要一段代码，那就要synchronized嗷！！！
3. 引出了ABA问题。

# 5. AtomicInteger的ABA问题：

- 原子引用更新 --->>> 如何规避ABA问题
- 狸猫换太子：
  - 就是快速反复横跳，把值该到了，欸嘿，我又改回来了，吐了，慢的根本不知道嗷orz。



## AutomicReference：

![image-20210914221952711](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210914221952.png)

对于我们需要的类进行原子包装嗷！！！

## 时间戳的原子引用：

- 规避ABA问题：时间戳的原子引用

### AtomicStampedReference

![image-20210915200908489](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915200923.png)

`atomicStampedReference.compareAndSet(expectedReference , newReference , expectedStamp , newStamp)`

常用用法：

`boolean result = atomicStampedReference.compareAndSet(100 , 101 , automicStampedReference.getStamp() , automicStampedReference.getStamp()+1);`



`int stamp = atomicStampedReference.getStamp();`

`int result = atomicStampedReference.getReference();`

相当于现在比较两个值了而已嘛，本质没啥区别

# 6. ArrayList线程不安全：

- 这里的名字不准确，应该是Collection类普遍不安全嗷！！！

## 写时复制：

- 扩容：默认的capacity是10，超过10了要扩容

![image-20210915203909547](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915203909.png)

![image-20210915204001207](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915204001.png)

- 线程不安全！！！

ArrayList的add方法为了保证并发性和效率，没有加锁嗷！！！

![image-20210915204429858](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915204430.png)

1. 故障现象：

java.util.ConcurrentModificationException

2. 导致原因：

并发争抢修改导致，两个进程抢着写的时候，造成了数据不一致的情况嗷！！！

CopyOnWrite , OnWrite的时候才Copy

![image-20210915210514393](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915210514.png)



3. 解决方案：

- 加锁！！！可以吗？不可以，JDK源码你加个锤子的锁？？？
- 3.1：用Vector类！！！

![image-20210915204814766](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915204815.png)

![image-20210915204845542](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915204846.png)

好使！！！但是太重了orz，而且Vector用的少orz，并发性低嗷orz。牺牲了安全性，提高了性能嗷！！！

- 3.2：Collection接口和Collections类！！！注意嗷，后面那个是类！！！Collections就是帮助我们来解决这个问题的嗷！！！![image-20210915205139824](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915205140.png)

  ![image-20210915205210076](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915205210.png)

你设想一下，这里竟然有这么多Map , List , Set之类的，说明这些东西都是线程不安全的嗷！！！

- 3.3：CopyOnWriteArrayList

![image-20210915210029661](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915210029.png)

![image-20210915210105142](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915210105.png)

```java
public boolean add(E e) {
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        Object[] elements = getArray();
        int len = elements.length;
        Object[] newElements = Arrays.copyOf(elements, len + 1);
        newElements[len] = e;
        setArray(newElements);
        return true;
    } finally {
        lock.unlock();
    }
}
```
- 老版本的copy出来，然后把自己的信息写进去，再让系统中的引用编程现在这个数组嗷！！！

4. 优化建议：



## HashSet线程不安全：

![image-20210915210926940](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915210928.png)

也是一样的问题嗷！！！

- 解决方法：
  1. `Collections.synchronizedSet(new HashSet<>());`
  2. JUC中的`CopyOnWriteSet<>()`

![image-20210915211450931](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915211451.png)

- HashSet底层数据结构就是HashMap<>()

![image-20210915211620437](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915211620.png)

- HashSet底层如果是HashMap，那为啥add方法只有一个参数捏？？？

![image-20210915211825796](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915211826.png)

![image-20210915211802866](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915211803.png)

value是个恒定的值哦！！！



## HashMap线程不安全：

![image-20210915212239707](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915212240.png)

- 解决方法：
  1. `ConcurrentHashMap`
  2. `Collections.synchronizedMap`



# 7. TransferValue醒脑：

![image-20210915215802972](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915215803.png)

![image-20210915215840582](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915215840.png)

String比较特殊嗷！！！它是final，赋予了地址之后，这个地址是不会变动的。String str = "abc"，String指向了常量池中的地址。而str = "XXX"，确实找到了常量池中的"XXX"，如下右图：

![image-20210915220409692](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915220409.png)

说白了就是传进去就是一个地址，方法中参数的str本质上也是一个拷贝，一个指向这个地址的指针的拷贝，这里的赋值，无非是把拷贝指针的指向变了，改变不了原指针的指向嗷！！！



# 8. Java中的锁们：

## 公平锁和非公平锁：

![image-20210915220949873](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915220950.png)

这里不传参数等于传入了false参数嗷！！！天生这个即使非公平锁嗷！！！公平锁就是先来后到，非公平就是允许某些线程能够“不公平竞争”，能够加塞！！！

![image-20210915221300727](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915221301.png)



### ReentrantLock：

![image-20210915221458845](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210915221459.png)

默认为非公平锁，非公平锁的有点在于吞吐量比公平锁大嗷！！！说白了又是体现了优先级呗！！！优先级高的能被响应，吞吐量就会大嗷！！！

- Synchronized而言，也是一种非公平锁哦！！！



## 可重入锁（递归锁）：

- ReentrantLock就是可重入锁哦！！！
- 可重入锁就是递归锁
- 指的是统一线程外层函数获得锁之后，内层递归函数仍然能获取该锁的代码！在同一个线程在外层方法能够获取锁的时候，在进入内层方法会自动获取锁嗷！！！（也就是说：线程可以进入任何一个**它已经拥有的锁所同步着的代码块**！！！）
- **线程可以进入任何一个它已经拥有的锁所同步的代码块。**
- ReentrantLock/Synchronized就是典型的非公平可重入锁嗷！！！
- **最大作用：避免死锁**
- 代码：

#### Case1：

![image-20210916201750895](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210916201751.png)

这个就是例子：sendSMS()调用了sendEmail，进入sendSMS中执行sendEmail的时候会自动获取锁并进入这个代码块执行嗷！！！

#### Case2：

![image-20210916202040755](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210916202040.png)

- 运行结果：

![image-20210916202204510](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210916202204.png)

- 思考：

![image-20210916202314495](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210916202314.png)

只要锁配对正确，加几把锁都没问题嗷！！！可以正确运行嗷！！！！



## 自旋锁：

- spinlock：尝试获取锁的线程不会立即阻塞，而是采用循环的方式尝试去获取锁。好处是减少了上下文切换的消耗，缺点是循环会消耗CPU嗷！！！

### 手写自旋锁：

- 本质：while + compareAndSet()方法

本质上利用一个拥有compareAndSet()方法的变量来帮助我们实现就行嗷！！！

```java
AtomicReference<Thread> atomicReference = new AutomicReference<>();

//加锁
public void myLock(){
    Thread thread = Thread.currentThread();
    System.out.println(thread.currentThread().getName + "\t come in O(n_n)");
    
    while(!atomicReference.compareAndSet(null , thread)){
        
    }
    //如果成功进入，根本就不进入while循环，直接执行下面的业务代码
}

//解锁
public void myUnLock(){
    Thread thread = Thread.currentThread();
    System.out.println(thread.currentThread().getName + "\t come out O(n_n)~~");
    automicReference.compareAndSet(thread , null);
}
//上面这些统一放到一个类中就可以了嗷(叫做SpinLockDemo)！！！
```

```java
public static void main(String[] args){
    SpinLockDemo spinLockDemo = new SpinLockDemo();
    
    new Thread(()->{
        spinLockDemo.myLock();
        //执行业务逻辑，例如sleep等待
        spinLockDemo.myUnLock();
    },"AA").start();
    
    new Thread(()->{
        spinLockDemo.myLock();
        //执行业务逻辑，例如sleep等待
        spinLockDemo.myUnLock();
    },"BB").start();
}
```

- 这里非常有技巧性，这里的AtomicReference的泛型就可以拥有compareAndSet方法，但是为了让每个线程都可以“独立的拥有一些特征来获得自旋锁”，我们采用了线程作为泛型对象进行操作嗷！！！
- `thread.currentThread()`就能够获得当前的线程，不同线程的这个值肯定不一样嗷！！！这样就比较好一些嗷！！！



## 独占锁（写锁）/ 共享锁（读锁）/ 互斥锁

### 独占锁（写锁）

- 指该锁一次只能被一个线程所持有。
- 对于ReentrantLock和Synchronized而言都是独占锁。
- 发展历程：`sync ---> lock ---> ReentrantReadWriteLock`
- 读写锁提高了并发效率嗷！！！从保证原子性到提高效率嗷！！！
- 规则：
  - 读-读可以共存
  - 读-写不能共存
  - 写-写不能共存

- 实例代码：

![image-20210917110337741](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917110338.png)

写操作：必须原子 + 独占（写锁）

这里模拟的时候，底层都是要用volatile的，保证可见性非常重要！！！缓存的可见性和及时性非常重要！！！

- ReentrantReadWriteLock读写同体，对于不同的方法要调用不同的方法适配嗷！！！

```java
ReentrantReadWriteLock rwlock = new ReentrantReadWriteLock();
try {
    //这里也很简单，直接根据不同情况获取不同的锁就成
    rwlock.readLock().lock();
} catch (Exception e) {
    e.printStackTrace();
}
```

- 加写锁例子：

![](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917112125.png)

- 加读锁例子：

![image-20210917112154108](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917112154.png)

- 以上两者对比：

### 共享锁（读锁）：

- 该锁可以被多个线程所持有

![image-20210917112350047](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917112350.png)



# 9. CountDownLatch / CyclicBarrier / Semaphore

## CountDownLatch：

- 有点像Linux中那个等待子线程结束（waitpid / pthread_join），主线程才能够最后退出的函数方法。

- Demo：

![image-20210917113301833](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917113302.png)

- 可谓是非常像了，这个玩意儿就是卡在这里，等所有线程的东西执行完了，才会继续往下执行嗷！！！



### 枚举：

- 可以当作小数据库来用嗷？？？还有这种操作？？？

![image-20210917114857319](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917114858.png)

![image-20210917115028480](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917115028.png)

- 原来的代码的修改：

![image-20210917115116832](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917115117.png)

程序耦合度急剧降低，只要修改枚举中的元素，处处生效嗷！！！

- 枚举中元素的读取：

![image-20210917115227796](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917115228.png)

第一个相当于行的名字，第二三行相当于读取这一行某个元素的值嗷！！！

- 非常实用嗷！！！



## CyclicBarrier：

- 增加到多少就能够发生某件事，和上面相反，这个是做甲方。
- 等人齐了，才能够开会~。

![image-20210917144338011](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917144338.png)

![image-20210917144706100](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917144706.png)



## Semaphore：

- 多个资源抢多份资源怎么控制哇？
- 这个就是信号量，一个是用于多个共享资源的互斥实用，另外一个适用于并发线程控制嗷！！！

![image-20210917145653800](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917145654.png)

![image-20210917145712432](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917145712.png)

- 这个和操作系统中的没啥区别，都是信号量嗷！！！一个信号量可以当作锁来用嗷！！！

# 10. 阻塞队列

## 阻塞队列：

- 阻塞队列有好的一面。
- 不得不阻塞，如何管理？

### 队列+阻塞队列以及其api：

- 就是生产者和消费者问题的Java版本而已
- 空的队列拿元素，阻塞。满的队列添加元素，阻塞。
- why BlockingQueue？不需要关心什么时候阻塞和唤醒线程，BlockingQueue给我们一手包办了嗷！！！这个效率非常高嗷！！！（手动挡变成了自动挡嗷！！！）

![image-20210917192349179](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917192349.png)

- 有好多实现类嗷！

![image-20210917192442233](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917192442.png)

![image-20210917192506647](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917192507.png)

红色的是重点，这三个就是线程池的底层嗷！！！

最后一个是Deque嗷！！！双端队列，和上面的Queue是有区别的嗷！！！

SynchronousQueue专属定制版，就一个嗷！！！

- 常用情况（抛出异常）：

![image-20210917192913651](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917192913.png)

![image-20210917213941158](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917213941.png)

像这个Queue，符合先进先出的概念，element就是检查第一个元素嗷！！！add和remove还有element就是有这样的一个特点：如果对应的操作失败，就会抛出异常嗷！！！

- 常用情况（特殊值）：

操作成功返回true，操作失败返回false嗷！！！

取出来的时候，取得到就是对应的元素，取不到就是NULL

- 常用情况（阻塞）：

如果操作失败的话，默认会进行阻塞，等待嗷！！！

![image-20210917214504234](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917214504.png)

- 常用情况（超时）：

不要那么猛，折中的一种做法嗷！！！我可以等，但是过时不候，如果超时了我线程就自己退出了嗷！！！

![image-20210917214652384](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917214652.png)

![image-20210917214711162](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210917214711.png)



## SynchronousQueue：

- 本质上是一个不存储元素的BlockingQueue，每一个put都要等待一个take嗷！！！

```java
SynchronousQueue<String> blockingQueue = new SynchronousQueue<>();

new Thread(()->{

    try {
        System.out.println(Thread.currentThread().getName() + "\t put 1");
        blockingQueue.put("1");
        
        System.out.println(Thread.currentThread().getName() + "\t put 2");
        blockingQueue.put("2");
        
        System.out.println(Thread.currentThread().getName() + "\t put 3");
        blockingQueue.put("3");
    } catch (InterruptedException e) {
        e.printStackTrace();
    }


},"Producer1").start();

new Thread(()->{

    try {
        System.out.println(Thread.currentThread().getName() + "\t put 4");
        blockingQueue.put("4");

        System.out.println(Thread.currentThread().getName() + "\t put 5");
        blockingQueue.put("5");
        
        System.out.println(Thread.currentThread().getName() + "\t put 6");
        blockingQueue.put("6");
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
},"Producer2").start();

new Thread(() -> {
    try {
        TimeUnit.SECONDS.sleep(3);
        System.out.println(blockingQueue.take());
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    try {
        TimeUnit.SECONDS.sleep(3);
        System.out.println(blockingQueue.take());
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    try {
        TimeUnit.SECONDS.sleep(3);
        System.out.println(blockingQueue.take());
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    try {
        TimeUnit.SECONDS.sleep(3);
        System.out.println(blockingQueue.take());
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    try {
        TimeUnit.SECONDS.sleep(3);
        System.out.println(blockingQueue.take());
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    try {
        TimeUnit.SECONDS.sleep(3);
        System.out.println(blockingQueue.take());
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
},"Consumer1").start();
```



## 用在哪里？

### 生产者消费者模式（老版）

- `ProdConsumer_TraditionDemo`
- `ProdConsumer_BlockQueueDemo`

- 传统版：

![image-20210918202929146](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210918202929.png)

老版sync向新版lock的转换：

![image-20210918203703145](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210918203703.png)

- 代码：

```java
public class ProdConsumer_TraditionDemo {
    public static void main(String[] args) {
        ShareData shareData = new ShareData();

        new Thread(()->{
            for (int i = 0; i < 5; i++) {
                try {
                    shareData.increment();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        },"Thread A").start();

        new Thread(()->{
            for (int i = 0; i < 5; i++) {
                try {
                    shareData.decrement();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        },"Thread B").start();
    }
}

class ShareData // 资源类
{
    private int number = 0;
    private Lock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();

    public void increment() throws Exception
    {
        //1. 判断
        lock.lock();
        try{
            //判断
            while (number != 0){
                //等待，不能生产
                condition.await();;
            }

            //干活
            number ++ ;
            System.out.println(Thread.currentThread().getName() + "\t" + number);
            //通知唤醒
            condition.signalAll();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }

    public void decrement() throws Exception
    {
        //1. 判断
        lock.lock();
        try{
            //判断
            while (number == 0){
                //等待，不能生产
                condition.await();;
            }

            //干活
            number -- ;
            System.out.println(Thread.currentThread().getName() + "\t" + number);
            //通知唤醒
            condition.signalAll();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}
```

- **多线程判断都要用while**，不要用if，if就有可能出现问题嗷！！！

```shell
Thread A	1
Thread B	0
Thread A	1
Thread B	0
Thread A	1
Thread B	0
Thread A	1
Thread B	0
Thread A	1
Thread B	0
```



### synchronized和lock的区别

1. 原始构成

- synchronized是Java关键字，lock是java5以后出现的一个类。
- synchronized是属于JVM层面的，monitorenter和monitorexit，底层是通过monitor对象来完成的，其实wait / notify等方法也是依赖于monitor对象，只有在同步块或同步方法中才能调用wait / notify等方法。
- Lock是具体类，JUC中，是api层面的锁嗷！！！

![image-20210918210748451](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210918210748.png)

2. 使用方法

- synchronized不需要用户去手动释放锁，当synchronized代码执行完成后系统会自动让线程释放对于锁的占用嗷！！！
- ReentrantLock需要用户手动释放锁，如果用户没有主动释放锁，那么就有可能会出现死锁现象嗷！！！需要lock ， unlock方法配合try / finally语句块来完成嗷！！！

3. 判断是否可以中断

- 等待是否可以中断，synchronized不可中断，除非抛出异常或者正常运行完成。ReentrantLock可中断：
  1. 设置超时方法 trylock（long timeout , TimeUnit unit）
  2. LockInterruptibly()放代码块中，调用interrupt方法可以中断

4. 加锁是否公平

- synchronized非公平锁
- ReentrantLock两者都可以，默认非公平锁，构造方法可以传入boolean值，true为公平锁，false为非公平锁。

![image-20210918212212308](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210918212212.png)

5. 锁绑定多个条件的Condition

ReentrantLock可以实现分组唤醒需要唤醒的线程们，可以**精确唤醒**，而不是像synchronized要么随机唤醒一个要么唤醒全部线程嗷！！！

- 例如A唤醒B，B唤醒C：

![image-20210918213531894](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210918213531.png)

```java
class ShareResource{
    private int number = 1; //A:1 , B:2 , C:3
    private lock = new ReentrantLock();
    private Condition c1 = lock.newCondition();
    private Condition c2 = lock.newCondition();
    private Condition c3 = lock.newCondition();
    
    public void print5(){
        lock.lock();
        try{
            //1. 判断
            while(number != 1)
            {
                c1.await();
            }
            
            //2. 干活
            for(int i = 1 ; i <= 5 ; i++)
            {
                sout(Thread.currentThread().getName()+"\t"+i);
            }finally {
            	lock.unlock();
        	}
            
            //3. 通知
            number = 2;
            c2.signal();
        }
    }
    
    public void print10(){
        lock.lock();
        try{
            //1. 判断
            while(number != 2)
            {
                c2.await();
            }
            
            //2. 干活
            for(int i = 1 ; i <= 10 ; i++)
            {
                sout(Thread.currentThread().getName()+"\t"+i);
            }
            
            //3. 通知
            number = 3;
            c3.signal();
        }finally{
            lock.unlock();
        }
    }
    
    public void print15(){
        lock.lock();
        try{
            //1. 判断
            while(number != 3)
            {
                c3.await();
            }
            
            //2. 干活
            for(int i = 1 ; i <= 15 ; i++)
            {
                sout(Thread.currentThread().getName()+"\t"+i);
            }finally {
            	lock.unlock();
        	}
            
            //3. 通知
            number = 1;
            c1.signal();
        }
    }
}
```



### 生产者消费者模式（阻塞队列）

```java
class MyResource{
    private volatile boolean FLAG = true;//默认开启，进行生产+消费
    private AutomicInteger automicInteger = new AutomicInteger();
    
    BlockingQuere<String> blockingQueue = null;
    public MyResource(BlockingQueue<String> blockingQueue){
        this.blockingQueue = blockingQueue;
        sout(blockingQueue.getClass().getName());
    }
    
    public void myProducer() throws Exception
    {
        String data = null;
        boolean retValue;
        while(FLAG)
        {
            data = automicInteger.incrementAndGet()+"";
            retValue = blockingQueue.offer(data,2L,TimeUnit.SECONDS);
            if(retValue){
                sout(Thread.currentThread().getName()+"\t 插入队列" + data + "成功");
            }else{
                sout(Thread.currentThread().getName()+"\t 插入队列" + data + "失败");
            }
            TimeUnit.SECONDS.sleep(1);
        }
        
        sout(Thread.currentThread().getName()+"\t 停下来了嗷！！！" + "表示FLAG=false，生产动作结束！");
    }
    
    public void myProducer() throws Exception
    {
        String result = null;
        while(FLAG)
        {
            result = blockingQueue.poll(2L,TimeUnit.SECONDS);
            if(null == result || result.equalIgnoreCase(""))
            {
                FLAG = false;
                sout(Thread.currentThread().getName()+"\t 超过两秒没有取到产品，消费退出");
                return;
            }
            sout(Thread.currentThread().getName()+"\t 消费队列" + result + "成功");
        }
        
        sout(Thread.currentThread().getName()+"\t 停下来了嗷！！！" + "表示FLAG=false，生产动作结束！");
    }
}
```

- main代码块中的一些代码：

![image-20210920151235614](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920151235.png)





## Callable接口（第三种多线程方式）：

- 两个接口：

![image-20210920151425701](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920151426.png)

区别：

1. Runnable没有返回值，Callable有返回值
2. Callable会抛出异常
3. 接口需要实现的接口不一样嗷！！！

- 中间层同时适配了Callable和Runnable接口嗷！！FutureTask这个类同时实现了Callable接口和Runnable接口，是按了适配嗷！！！！

- 背景？为什么会出现呢？

并发，异步导致。高并发最牛逼的控制嗷！！！初衷就是：麻烦的事儿交给别的线程去处理，实现高并发需要使用的嗷！！！

兔兔想法：实现更高程度的并行嗷！！！（分支合并操作嗷！！！）

- 小建议：
  - 把get方法放在最后哦！！！get如果没有获取到值的话，会阻塞在这里嗷！！！
  - 等它算完了再去取就会比较方便嗷！！！所以我们尽量放在后面取。
  - 当然如果某一步就要结果，又不想阻塞的话，我们就可以类似于自旋锁，通过`futureTask.isDone()`来实现哦！！！

![image-20210920154316590](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920154316.png)

提高了并行性嗷！！！

- 如果启动了多个线程来抢着执行同一个任务呢？
  - ![image-20210920154709812](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920154710.png)
  - 不好意思，只执行了一次嗷！！！同样的动作就应该复用嗷！！！如果你真的想走两遍的话，再自己多去做一个FutureTask嗷！！！



## 线程池使用以及优势（第四种多线程方式）：

### 概念：

- 查看电脑CPU核数量：

`Runtime.getRuntime().availableProcessors()`可以获得硬件中可用的CPU的核数嗷！！！

- 从new对象到IoC的反转，依赖注入 -> 全都给你准备好，你直接复用我准备好的就成嗷！！！
- 底层就是阻塞队列嗷！！！
- 底层是`ThreadPoolExecutor ThreadPoolExecutor ThreadPoolExecutor`这个类嗷！！！Executor这个接口就对应了Executors这个工具类嗷！！！（和Array，Arrays一样的嗷！！！）

![image-20210920163453304](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920163453.png)

- 了解：
  - `Executors.newScheduledThreadPool()`时间参数用于调度的
  - `Executors.newWorkStealingPool(int)`可用的处理器作为它的并行级别
- 重点：
  - `Executors.newFixedThreadPool(int)`
  - `Executors.newSingleThreadPool()`
  - `Executors.newCachedThreadPool(int)`



### 代码：

#### newFixedThreadPool：

- 固定数目线程池，处理线程数目固定哦！！！

- 模拟十个用户用五个请求来进行处理嗷！！！



![image-20210920162739367](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920162740.png)

处理结果：

![image-20210920162759410](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920162759.png)

- 关闭比使用更加重要嗷！！！



#### newSingleThreadExecutor：

- 固定单个数目线程池

![image-20210920162925543](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920162925.png)



#### newCachedThreadPool：

- 线程池数目不固定，够用就好嗷！！！如果线程不够的话，还会自动扩容嗷！！！

![image-20210920163117475](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920163118.png)

- 核心思想：

![image-20210920163152856](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920163153.png)

我尽力办，我办的完的话，就ok。我要是办不完，我就摇人了嗷！！！

#### 总结：

- 发现上面这三个玩意儿，底层都是`return new ThreadPoolExecutor(...)`！！！所以ThreadPoolExecutor如此重要嗷！！！

![image-20210920163734036](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920163734.png)

![image-20210920163751881](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920163752.png)

![image-20210920163820239](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920163820.png)

- 底层源码重点：
  - ThreadPoolExecutor这个类很重要！！！
  - 底层的阻塞队列也很重要嗷！！！！

- 使用方法：

![image-20210920164038942](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920164039.png)



### 七大参数简介：

![image-20210920203543898](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920203544.png)

1. corePoolSize：常驻核心线程数

![image-20210920203809064](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920203809.png)

2. maximumPoolSize：

物理上线程池能够容纳的同时执行的最大线程数，此值必须大于等于1嗷！！！

3. keepAliveTime：

多余的空闲线程的存活时间，当线程池数量超过CorePoolSize的时候，空闲时间达到keepAliveTime的值的时候，多余的空闲线程会被销毁直到只剩下corePoolSize个线程为止嗷！！！

没有业务了，“加班”的人会慢慢回退嗷！！！

4. unit：keepAliveTime的单位

5. workQueue：任务队列，被提交了，但是由于核心线程都被占用而无法执行，处于等待状态的任务嗷！！！

类似于银行中的候客区

6. threadFactory：生成线程池中线程的线程工厂，用于创建线程，一般用默认的就可以了嗷！！！



7. handler：拒绝策略，如果线程池满了，阻塞队列都满了，那就......只能够采取一些操作了orz

类似于银行网点，上班的人都在服务（但是还有多的空位），等待的人爆了，银行紧急通知其他休假的人赶紧赶回来上班嗷！！！

![image-20210920204832709](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920204832.png)

当所有人都在上班了（物理线程池被占满了），候客区也满了，不好意思。我银行已经尽力的，多来的请求我就不收了。



### 底层工作原理：

![image-20210920210509956](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920210510.png)

这幅图就是上面的银行网点那幅图嗷！！！

- 主要处理流程：

![image-20210920211449576](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920211449.png)

![image-20210920211539142](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920211539.png)



### 四种拒绝理论：

#### 大坑：

- 上面学的三种，实际开发中，**一个都不用嗷！！！（超级大坑！！！）**
- 阿里开发手册：

![image-20210920212314139](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920212314.png)

![image-20210920212414735](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920212414.png)

就是这里说的，由于底层是链表，没有最大长度，默认是Integer.MAX_VALUE。这就一个非常大的问题，就是如果我一直往阻塞队列里面加请求，加满了，你的Linux也就爆了orz。





#### 自定义线程池：

`ExecutorService threadPool = new ThreadPoolExecutor(2,5,1L,TimeUnit.SECONDS,new LinkedBlockingQueue<Runnable>(3),Executors.defaultThreadFactory(),new ThreadExecutor.AbortPolicy());`

![image-20210920213917146](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920213917.png)

- 测试什么时候出问题呢？

![image-20210920214035505](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920214035.png)

最大承载量 = maximumPoolSize + 阻塞队列最大长度，因此来9个时候可能出现这个问题嗷！！！





#### AbortPolicy：

- 概念：

![image-20210920211801937](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920211802.png)

- 种类：

![image-20210920211826013](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920211826.png)

- 如上图所示，出了问题直接报错嗷，程序运行终止。



#### CallerRunsPolicy：

- 调用者运行的一种调节机制，不会抛弃任务也不会抛出异常，而是将一些任务**回退给调用者进行处理**，从而降低流量嗷！！！

![image-20210920214706006](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920214706.png)

这里就会退给main来处理了嗷！！！



#### DiscardOldestPolicy：

- 抛弃等待最久的，将最新的加入到队列中再次尝试提交嗷！！！



#### DiscardPolicy：

- 多来的直接就给了扔掉了嗷！！！



### 配置合理线程数：

- 熟悉硬件：

  1. CPU密集型

  ![image-20210920215354656](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920215354.png)

  2. IO密集型

  - I/O密集型任务线程并不是一直在执行任务，配置尽可能多的线程，如CPU核数*2
  - ![image-20210920215528181](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920215528.png)



## 死锁编码及定位分析：

- 两个火柴人拿枪指着对方orz，好家伙orz。

### 写个死锁呗：

![image-20210920220626213](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920220626.png)

run没有写全，写全的版本在下面嗷！！！

![image-20210920220647467](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920220647.png)

- main函数：

![image-20210920221006854](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920221006.png)



### 小介绍：

1. jps

linux中`ps -ef | grep xxx`


windows下的java运行程序捏？也有类似于ps查看进程的命令，但是目前我们需要查看的只是java的程序嗷！！

jps = java ps嗷！！！

![image-20210920222052258](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920222052.png)



2. jstack

jstack + `进程号`

结合上面的jps一起使用，找到对应的线程编号，再查看线程的错误信息嗷！！！

![image-20210920222340886](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920222340.png)

- summary：
  1. jsp命令定位进程号
  2. jstack找到死锁查看

![image-20210920222637114](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210920222637.png)





# 11. JVM+GC

## 复习串讲：

- JVM内存结构：

![image-20210922204101050](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922204116.png)

- GC作用域：

![image-20210922204137849](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922204138.png)

- 常见垃圾回收算法：

  1. 引用计数：

  有对象应用+1，没对象-1，到0就回收。**较难解决循环引用问题**，因此JVM不采用这种方式。

  2. 复制算法：

  复制算法在年轻代中用

  ![image-20210922205325740](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922205326.png)

  名言：复制之后有交换，谁空谁是To

  ![image-20210922204816262](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922204836.png)

  缺点就是复制大对象耗时，浪费空间嗷！！！

  3. 标记清除：

  Mark-Sweep，分为标记清除两个阶段。先标记出要回收的对象，然后统一回收这些对象。

  好处：不用复制，耗时少了。

  坏处：出现了内存碎片

  ![image-20210922205102880](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922205103.png)

  4. 标记清除整理：

  Mark-Compact

  ![image-20210922205200495](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922205201.png)

  移动过程中，耗时比较多嗷！！！



## 如何确定垃圾+GCRoot：

- 内存中不再被使用的空间就是垃圾

- 如何判断一个对象是否是垃圾？

  1. 引用计数法：简单，但是循环引用解决不了
  2. 枚举根结点做可达性分析：

  复制，标记清除，标记压缩都要用GCRoots来判断哪些是垃圾嗷！！！

- GCRoots：

![image-20210922205724103](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922205724.png)

从GC对象进行扫描，看是否可达。

- 哪些对象可以作为GC Roots对象：

![image-20210922205901103](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922205901.png)

- 可以作为GCRoot的根呢？

![image-20210922210038211](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922210038.png)

![image-20210922210552919](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922210553.png)



## JVM的参数介绍和调整：

### 查看参数：

![image-20210922215607756](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922215608.png)

- `-Xms和-Xmx`尽量调成一致，防止GC频繁收集，忽高忽低。

- 参数类型：

  1. 标配参数：

     - 随着java变更，基本不会动的参数，没有大的变化。
     - ![image-20210922210931972](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922210932.png)

  2. X参数：

     - ![image-20210922211037799](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922211038.png)
     - ![image-20210922211114713](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922211115.png)
     - ![image-20210922211132961](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922211133.png)

  3. XX参数：

- Boolean类型：

       - ![image-20210922211322130](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922211322.png)
  
- ![image-20210922211556106](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922211556.png)
  
  - 先让一个程序空转，比如`sleep(Integer.MAX_VALUE)`：
     - 查看线程情况：
  
       `jps -l`和`jinfo`，这个jinfo是显示当前正在运行的线程的信息嗷！！！
       
       - 查看某个线程的情况：
       
       ![image-20210922212646075](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922212646.png)
       
       - 编辑参数：
       
       ![image-20210922211446165](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922211446.png)
       
       ![image-20210922212743398](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922212743.png)
       
       开启参数之后再测试：
       
       ![image-20210922212834155](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922212834.png)



- KV设值类型：

![image-20210922213125480](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922213125.png)

![image-20210922213146784](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922213147.png)

查出某个进程的Metaspace的默认设置：

![image-20210922213251833](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922213252.png)

设置对应的值：

![image-20210922213406757](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922213407.png)

例如这个MaxTenuringThreshold就是young区的对象进入老年区所需要的年纪嗷！！！

![image-20210922213708933](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922213709.png)

所以这里就可以看出来默认的一些配置嗷！！！

- jinfo的配置使用：

![image-20210922213920863](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922213921.png)

-flags：

![image-20210922214035429](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922214035.png)

这个东西是要求得到运行中的线程的id，再去flags才能看得到的嗷！！！

`Non-default`是系统干的，帮我们自动配置好的。

`Command line`是我们自己配置的嗷！！！

- 题外话，坑题（-Xms和-Xmx如何解释？）

![image-20210922214901303](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922214901.png)

这两个参数也是一样样的，本质上就是-XX参数，用的多。这两个不属于`X`参数哈，还是属于`XX`参数嗷！！！只是用到多，单独提出来方便使用而已嗷！！！这两个值一般都是配置成一样的嗷！！！这样才能保证GC尽可能少哦！！！

### 查看参数二：

![image-20210922215639580](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922215640.png)

- 方式：

1. `-XX:+PrintFlagsInitial`查看初始参数值

![image-20210922215725094](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922215725.png)

![image-20210922215802398](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922215803.png)

2. `-XX:+PrintFlagsFinal`查看修改更新的参数值

![image-20210922220037254](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922220037.png)

- `=和:=`
  - 等于是没有被改过的初始值
  - `:=`是我们人为改过的或者是JVM加载的时候修改过的参数值嗷！！！
- -version的话，只是在最后多打出了JVM的版本号而已嗷！！！
- 命令行修改：

![image-20210922220932163](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922220932.png)

![image-20210922220945294](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922220945.png)

修改之后，运行T之后，可以看出来元空间的值发生了变化嗷！！！

3. `-XX:+PrintCommandLineFlags`打印命令行参数

![image-20210922221211484](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210922221211.png)

这个命令最方便就是看最后一个参数！！！垃圾回收器的种类和默认参数嗷！！！



## JVM基本配置：

### Java8调整：

- Java的MetaSpace：

![image-20210923085939278](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923085939.png)

![image-20210923090428487](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923090428.png)

- 通过程序获得初始内存大小和最大的堆内存大小：

![image-20210923090141078](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923090141.png)



### 通用参数：

![image-20210923090542706](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923090542.png)

1. ![image-20210923090615310](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923090615.png)
2. ![image-20210923091332550](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923091333.png)

- 如何配置：

![image-20210923091426848](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923091427.png)

注意：**栈管运行，堆管存储**嗷！！！！这个栈是线程运行的时候所拥有的栈的大小

当我们不配置-Xss参数的时候，出现了神奇的问题：

![image-20210923091541472](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923091541.png)

它默认值为0

![image-20210923091733044](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923091733.png)

这里为0其实上表示采用系统的默认值，默认值又是多少呢？上图实际上告诉我们了嗷！！！（这个东西在Java的官方文档中都有嗷！！！）

3. `-Xmn`：设置年轻代的大小嗷！！！

- 一般自己不用调，采用系统默认的

![image-20210923092139544](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923092140.png)

4. `-XX:MetaspaceSize`

#### 典型参数设置案例：

- 设置元空间大小：

![image-20210923092247567](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923092248.png)

- 坑：

虽然说着元空间有这么多可以用，但是元空间自己取的少啊orz，就有可能报`OOM:Metaspace`

![image-20210923092536820](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923092536.png)

这里默认值就20多M而已啊orz，我内存16个G呢orz。

- 为了保证不会出现OOM：

参数配置实例：

下面是我们要手动配置的参数：

![image-20210923092916447](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923092916.png)

`-XX:+UserSerialGC`是串行垃圾回收器嗷！！！

默认设置也可以瞅一眼哦：

![image-20210923093113771](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923093114.png)

运行结果：

![image-20210923093152554](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923093152.png)

查看默认参数：

![image-20210923093223512](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923093223.png)

可以看到默认是这些，我们修改之后再康康嗷！！！

我们配置一下上面最开始提到的参数：

![image-20210923093334904](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923093335.png)

参数设置后的结果：

![image-20210923093511004](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923093511.png)

完全按照我们的配置来启动了嗷！！！



5. `-XX:+PrintGCDetails`

- 输出GC的详细收集日志信息
- 可以通过限定堆的大小，然后人为new一个贼大的对象来构造GC的错误嗷，这个过程中会产生GC嗷！！！

上下是对应的嗷！！！

![image-20210923182346063](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923182346.png)

![image-20210923182404627](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923182405.png)

规律：`[名称： GC前内存占用 -> GC后内存占用(该区总内存大小)]`



6. `SurvivorRatio`

- 设置新生代中eden区和S0/S1空间的比例。
- 默认：

![image-20210923182944953](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923182945.png)

- 查看默认参数：

![image-20210923183152525](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923183152.png)

![image-20210923183235580](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923183235.png)

改了之后再打印就是我们修改之后的值了嗷！！！



7. `-XX:NewRatio`

![image-20210923185442895](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923185443.png)

设置老年代占比，剩下1给新生代

![image-20210923185749201](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923185749.png)

默认就是1：2嗷！！！



8. `-XX:MaxTenuringThreshold`

- 设置垃圾的最大年龄，超过这个年龄就从年轻代进入老年代嗷！！！

![image-20210923185951515](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923185951.png)

![image-20210923190031165](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923190031.png)

通过这个参数就可以设置垃圾的最大年龄嗷！！！





## 四种引用：

![image-20210923190412091](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923190412.png)

1. 强引用（默认支持模式）

`Book book = new Book("兔子姐姐的小窝")`

- 当内存不足，JVM开始垃圾回收，对于强引用的对象，**就算是出现了OOM也不会对于这个对象进行回收**嗷！！！死都不收！！！



2. 软引用

- 内存足够的前提下我不收（就算手动GC也不回收哦，我够用嘛！！！），内存不够的情况收了你，来保证尽量不要出现OOM嗷！！！
- 软引用是相对强引用弱化了一些的引用，需要用`java.lang.ref.SoftReference`类来实现，可以让对象豁免一些垃圾回收。
- 通常用到对于内存敏感的程序中，高速缓存中就有用到嗷！！！

![image-20210923190911406](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923190911.png)

![image-20210923191004767](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923191004.png)

注意上面这里哈，实际上告诉了我们获得软引用的方式嗷！！！

- 故意整活：

![image-20210923191149341](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923191149.png)

![image-20210923191208008](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923191208.png)

5M的小内存，分配了30M的对象，必垃圾回收嗷！！！

![image-20210923191308316](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923191308.png)

- 使用场景：

![image-20210923192050793](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923192051.png)

`Map<String,SoftReference<Bitmap>> imageCache = new Hashmap<String,SoftReference<Bitmap>>();`

在保证我程序正常运行的情况下，把空间给你多存一点照片，提高你的效率，这是好事儿。但是如果我都不够用了，那我就不客气了，把你收了嗷，保证不OOM最重要嗷！！！



3. 弱应用

- 不管够不够用，垃圾回收一律都收掉嗷！！！不管啥情况，回收我就收了你！！！

![image-20210923191525848](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923191526.png)

![image-20210923191539510](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923191539.png)

- 使用场景：

`WeakHashMap`

key是弱key，如果被垃圾回收了的话，这个entry就会被自动移除嗷！！！

![image-20210923194113847](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923194114.png)

```java
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    V value;
    Node<K,V> next;

    Node(int hash, K key, V value, Node<K,V> next) {
        this.hash = hash;
        this.key = key;
        this.value = value;
        this.next = next;
    }

    public final K getKey()        { return key; }
    public final V getValue()      { return value; }
    public final String toString() { return key + "=" + value; }

    public final int hashCode() {
        return Objects.hashCode(key) ^ Objects.hashCode(value);
    }

    public final V setValue(V newValue) {
        V oldValue = value;
        value = newValue;
        return oldValue;
    }
```

底层本质是一个Node的数据结构，实现了Map的Entry，比如上面，本质上put的时候，map底层新建立了一个Node，Node中的key的引用就是传入的Integer的应用，因此如果你修改的是key的话，实际上对于对应位置的引用没有任何的改变，因此这个打印出来的结果还是上面那个结果嗷！！！

![image-20210923194648193](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923194648.png)

![image-20210923194711738](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923194711.png)

这个key不会阻止垃圾回收嗷，如果key失效了，就会被remove嗷！！！

适用于对于内存敏感的使用嗷！！！



4. 虚引用：

- 又被称为幽灵引用

![image-20210923194905029](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923194905.png)

![image-20210923194930822](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923194930.png)

![image-20210923195019141](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923195019.png)

- 回收之前需要被引用队列保存一下嗷！！！
- 弱引用队列的例子：

![image-20210923200814571](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923200814.png)

![image-20210923200754945](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923200755.png)

- 虚引用队列的例子：

![image-20210923201325447](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923201325.png)

![image-20210923201500175](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923201500.png)

重点就是后续操作，可以做一些通知之类的嗷！！！

## GCRoots + 四大引用小总结：

![image-20210923201825458](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210923201825.png)

- Java中可以作为GC Roots的对象：
  - 虚拟机栈（栈帧中的局部变量区，也叫做局部变量表）。
  - 方法区中的类静态属性引用的对象。
  - 方法区中常量引用的对象。
  - 本地方法栈中JNI（Native方法）引用的对象







# 12. SOFE

## 各种异常分类：

**栈管运行，堆管存储。**

1. StackOverflowError

这里的栈就是程序运行时对应的栈，由于内存不大，在调用方法的时候就会压栈，我们可以人为构造循环调用方法来制造SOFE：

![image-20210924203910628](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924203911.png)

这个是错误，不是异常嗷！！！异常是Exception，错误是Error：

[下面这个就是Error和Exception的解释嗷！！！](https://blog.csdn.net/weixin_43698561/article/details/104382643)

![image-20210924204413080](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924204413.png)

![image-20210924204613291](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924204613.png)

栈爆了。



2. OOM：java heap space

先把heap的大小调小：

![image-20210924204800557](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924204801.png)

![image-20210924204858781](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924204859.png)不断新建对象，这样就可以了嗷！！！

![image-20210924204932664](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924204932.png)

上面这样也行，直接new一个80M的数组，上面调过堆内存只有10M，这个对象远远大于10M。

![image-20210924205021400](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924205022.png)

这个也是个Error嗷！！！堆爆了。



3. OOM：GC overhead limit exceeded

- 程序特别诡异，Java内存急剧上升，大量对象被装载被创建出来。启动GC之后，超过最高警戒了。

![image-20210924205327339](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924205328.png)

- GC反复执行，恶行循环，执行存在非常大的问题嗷！！！
- 实例：

![image-20210924210445570](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924210445.png)

![image-20210924210624990](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924210625.png)

![image-20210924210647590](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924210648.png)



4. OOM：Direct buffer memory

- 是由于底层NIO引起的嗷！！！

![image-20210924211358960](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924211359.png)

里面够用，外面用满了orz。默认堆外可用内存是总内存的1/4嗷！！！

- 示例代码：

查看本地可用的JVM内存：

![image-20210924211557162](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924211557.png)

内存的图：

![image-20210924213349784](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924213350.png)

内存：

![image-20210924213444653](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924213444.png)

- 下面是示例哈，把外部内存的可用参数调成5M之后，在内存中new一个6M的对象，就会出现上面的问题嗷！！！

![image-20210924213507575](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924213507.png)

![image-20210924214304637](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924214305.png)

这个问题就是由于NIO所引出的哦，面试的时候就有可能问你这个`Direct buffer memory`的问题，来刺探你是否使用过NIO嗷！！！



5. OOM：unable to create new native thread

![image-20210924215151360](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924215151.png)

- 准确的讲，该native thread异常与对应的平台有关。说白了就是：创建的线程的数量超过了系统允许的最大值嗷，导致出现了这个异常嗷！！！！！
- 示例：

![image-20210924220700427](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924220700.png)

Linux中默认允许单个进程可以创建的线程数是1024嗷！！！

![](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210924221132.png)

- 解决方案：
  1. 减少跑起来的线程数
  2. 扩大底层机器允许的最大线程数
  
  ![image-20210925150439766](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925150440.png)
  
  ![image-20210925150524905](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925150525.png)
  
  注意，linux下的root用户对于线程是没有上限的，其他用户默认一个进程最多创建的线程数就是1024，修改：
  
  ![image-20210925150621473](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925150621.png)





6. OOM：Metaspace

- 元空间是方法区，方法区装类的模板，常量池啊之类的东东。

![image-20210925150909936](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925150910.png)

- 元空间与持久代的最大区别：Metaspace并不在虚拟机内存中，而是使用本地内存。

![image-20210925151015937](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925151016.png)

- 示例：

设置元空间的大小为8M：

![image-20210925151904788](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925151905.png)

![image-20210925151929884](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925151930.png)



## GC垃圾回收器的理解：

- GC有四种垃圾回收算法（引用计数/复制/标记清除/标记整理），垃圾收集器本质上就是这些算法的落地实现。
- 没有完美的垃圾收集器，都有具体的使用场景嗷！！！
- 四种垃圾收集器

### 垃圾收集器策略：



#### Serial

- 串行

![image-20210925152550265](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925152550.png)

- 为单线程环境设计，只使用一个线程进行回收，并且会**暂停全部的用户线程**，不适用于服务器环境。（中间业务线程被打断了orz）

#### Parallel

- 并行

![image-20210925152745385](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925152745.png)

- 多个垃圾收集线程并行工作，此时用户线程是暂停的，适用于科学计算 / 大数据处理等弱前台交互系统。
- 停顿回收垃圾的时间回比串行少，但是仍然和前面一样的，需要把所有的用户线程**停下来**！！！

#### CMS

- 并发垃圾回收器（并发标记垃圾清除）
- 用户线程和垃圾回收线程同时执行（不一定是并行，有可能交替执行），不需要停顿用户线程。互联网公司大多都用CMS，它适用于对于**响应时间有要求的场景**。

![image-20210925153058844](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925153059.png)



#### 小总结：

![image-20210925153237964](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925153238.png)

![image-20210925153256070](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925153256.png)

#### G1

- 将堆内存分割为不同的区域，然后并发对于不同的区域进行垃圾回收嗷！！！

#### ZGC

- G1的升级版本

- 比较新，Java12之后才出现的，需要自己下去看看嗷！





## 服务器默认垃圾收集器查看：

- GC深透明细，思想：四大垃圾回收算法 --->>> 落地实现

![image-20210925153851252](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925153851.png)

- 打印出垃圾收集器：

![image-20210925153934881](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925153935.png)

默认使用的是并行垃圾回收器

- 设置垃圾收集器：

![image-20210925154143267](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925154143.png)

## 默认垃圾收集器有哪些？

![image-20210925154304571](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925154304.png)

- CMS（ConcMarkSystem）
- 总共理论上有七种的，UseSerialOldGC和引用计数一样，已经被这个时代废除了，源码中也看不到了，但是它曾经存在过嗷！！！
- 查看某个运行中的进程的垃圾回收器：

![image-20210925155113074](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925155113.png)



## 七种垃圾收集器：

![image-20210925155941812](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925155942.png)

![image-20210925160145418](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925160146.png)

- 年轻代更多使用复制算法，老年代我们更多使用标记清除或者标记整理算法。



### 部分参数说明 / C/S模式有什么区别

- 参数：
  - DefNew - Default New Generation
  - Tenured - Old
  - ParNew - Parallel New Generation
  - PSYoungGen - Parallel Scanvenge Young Generation
  - ParOldGen - Parallel Old Generation

- C/S模式：

  - 只需要掌握Server模式即可，Client模式基本不会用
  - 操作系统：

  ![image-20210925160642642](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925160643.png)





### 新生代：

- Young和Old的垃圾收集器是搭配使用的嗷，注意上面那张图图捏！！！新生代的指定了，老年代的基本上就确定了嗷！！！

#### Serial / Serial Copying：

- 串行+串行

![image-20210925161602019](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925161602.png)

简单高效，没有线程交互的开销，可以获得最高的单线程垃圾收集效率，因此Serial垃圾收集器依然是Java虚拟机运行在Clinet模式下默认的垃圾收集器

![image-20210925161807280](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925161807.png)

- Y和O都是Serial回收，只是回收过程中，采取的回收算法不一样而已嗷！！！

#### ParNew：

- 并行+串行

![image-20210925162130287](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925162131.png)

- 年轻代的ParNew是搭配老年代的CMS使用的嗷！！！

![image-20210925162839624](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925162840.png)



#### ParallelScanvenge：

- 并行+并行

![image-20210925163017590](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925163018.png)

- ![image-20210925163200837](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925163201.png)

串行收集器在新生代和老年代的并行化

![image-20210925163325742](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925163326.png)

- 配置这个模式：

![image-20210925163433318](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925163434.png)

![image-20210925163508148](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925163508.png)



### 老年代：

#### Serial Old / Serial MSC：

- 老年代的串行收集器
- 单线程+标记整理算法，这个收集器也是主要运行在Client默认的java虚拟机默认的老年代垃圾收集器。

![image-20210925172206379](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925172206.png)

![image-20210925172227843](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925172228.png)

- 已经优化掉了，都不用了都orz

#### Parallel Old：

- 并行+并行

- Parallel Scavenge的老年代版本，使用多线程的标记-整理算法，Parallel Old收集器在1.6才开始使用嗷！！！

![image-20210925163901411](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925163901.png)

- 不加的话默认使用的是`UseParrellelGC`，就在上面，所以老年代默认的也是使用Parallel Old嗷！！！





#### CMS：

- 并发标记清除GC（CMS）
- 好处：解决空间和时间，坏处：内存碎片

![image-20210925170642985](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925170643.png)

![image-20210925170707008](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925170707.png)

- 过程：

![image-20210925170740207](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925170740.png)

![image-20210925171345757](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925171346.png)

1，3要停，2，4不要停：

重新标记：

![image-20210925171053270](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925171053.png)

并发清除：

![image-20210925171123242](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925171123.png)

标记和清除这两个最耗时的操作，可以并发执行嗷！！！

- 优缺点：
  - 优点：并发收集低停顿
  - 缺点：并发执行，CMS在收集与应用线程会同时增减对于堆内存的占用，CPU的压力比较大！！！CMS必须在老年代堆内存用尽之前完成垃圾回收，否则就会回收失败，触发担保机制。串行老年代收集器将会以STW的方式进行一次GC，从而造成较大停顿时间。此外，标记清除算法会导致大量碎片嗷！！！

![image-20210925171814442](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925171815.png)



- 同时配置多个垃圾收集器：

![image-20210925172416956](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925172417.png)

关联激活，完全没有必要这么去写嗷！！！



## 选择合适的垃圾收集器：

- 适用场景：

![image-20210925172835478](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925172835.png)

- 对应关系：

![image-20210925173107600](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210925173108.png)





## G1垃圾回收器：

- G1又被称为`Garbage-First`嗷！！！

![image-20210926191553163](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926191656.png)

- 打印结果显示：

![image-20210926191630759](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926191729.png)

别的都有两个区哦这里，G1只有一个garbage-first heap，这就是上面所提到过的，G1横跨两个区哦！！！

![image-20210926191942141](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926191942.png)

G1是一种服务端的垃圾收集器，应用在**多处理器和大容量内存环境**中，实现高吞吐量的同时，尽可能满足垃圾收集暂停时间的要求。

- 特性：
  1. 像CMS一样，能与应用程序并发执行
  2. 整理空闲时间空间更快
  3. 需要更多的时间来预测GC停顿时间
  4. 不希望牺牲大量的吞吐性能
  5. 不需要更大的Java Heap

- G1就是为了取代CMS而被设计的，不会产生很多内存碎片，而且添加了预测机制，用户可以指定期望停顿时间嗷！！！
- Eden和Survivor和Tenured等区域不再是连续的，而是变成了一个个大小一样的region。

![image-20210926192932201](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926192932.png)

Java9开始就作为默认的垃圾收集器了嗷！！！

- 特点：

![image-20210926193155568](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926193156.png)



### 底层原理：

- Region区域化垃圾收集器，化整为零，避免全内存扫描，只需要按照区域进行内存扫描嗷！！！

- ![image-20210926193423711](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926193424.png)
- G1的改变：

![image-20210926193703956](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926193704.png)

化整为零，避免了全内存扫描嗷！！！相当于把垃圾回收的基本单位变小了orz，粒度小了，并发度就高了嗷！！！

![image-20210926193946667](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926194304.png)

大的对象，也可以通过紧凑的方法来腾出空间，边清理，边内存整理，当然效率高哇！！！

- G1分为了四个区嗷：
  - Eden区
  - Survivor区
  - Old区
  - Humongou大对象区

- 垃圾回收流程：

![image-20210926194411751](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926194412.png)

![image-20210926194432326](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926194432.png)

G1中的内存处于一个动态变化的过程中嗷！！！有点像分页算法？？？？？？？这种熟悉的感觉，emmmmmm

![image-20210926194519537](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926194520.png)

回收步骤倒是和CMS一模一样，这不就和HashTable和ConcurrentHashMap一样嘛orz，对于每一个小块，采用的都是同样的CMS算法嗷！！！

- 好多参数嗷！！！

![image-20210926195229637](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926195230.png)

这些都可以尝试着看一下哈！！！一般都是用默认值的嗷！！！



### G1和CMS相比的优势

1. G1没有内存碎片嗷！！！
2. G1可以精确控制停顿嗷！！！



## JVMGC + SpringBoot微服务部署

- JVMGC --> 调优 --> 应用于微服务

- 步骤：
  1. IDEA开发完成微服务工程
  2. maven进行clean和package
  3. 微服务启动的时候，同时配置我们JVM / GC的调优参数：
     - 内
     - 外 => 中带你
  4. 公式：`java -server jvm的各种参数 -jar jar/war包的名字`

- 示例：

![image-20210926200212372](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926200212.png)

![image-20210926200313040](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926200313.png)



# 13. Linux相关



## 生产环境服务器变慢了

### top：

- ![image-20210926201513031](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926201513.png)
- 在top的情况下，按下`1`，就会显示所有的CPU的占用情况嗷！！！

![image-20210926202105515](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926202106.png)

可以看到每个CPU的各种参数的使用情况嗷！！！



### uptime：

![image-20210926202244709](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926202244.png)

精简版的ps，uptime你值得拥有



### vmstat：

`vmstat -n 2 3`

- 主要用于查看CPU，但是包含不限于嗷！！！
- 每2秒采样一次，共计采样3次

```shell
[root@VM-12-10-centos ~]# vmstat -n 2 3
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 3  1      0  76776 139660 1405012    0    0     0    29    2   12  1  0 99  0  0
 0  0      0  76272 139660 1405044    0    0     0    48 1145 2390  3  3 94  0  0
 1  0      0  76256 139660 1405060    0    0     0     0 1093 2187  0  0 100  0  0
```

- 头和尾的两个比较重要嗷！！！
- r是run , b是block。

![image-20210926202752052](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926202753.png)



### mpstat：

- `mpstat -P ALL 2`

每两秒采样一次，查看所有CPU的核信息：

![image-20210926203029814](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926203030.png)

查看每个进程使用CPU的用量分解信息：

`pidstat -u 1 -p 进程编号`

![image-20210926203218298](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926203218.png)





### free：

- 查看程序可用内存数

- `free -m`按照M来看，系统内存的可用数量

![image-20210926203425023](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926203425.png)

- `pidstat -p 进程号 -r 采样间隔秒数`



### df：

- disk free，查看磁盘剩余空间数

- `df -h` 用人类看得懂的方式来告诉我们，可用的磁盘空间大小



### ioStat：

- `iostat`，查看磁盘IO，磁盘I/O性能评估
- `iostat -xdk 2 3`

![image-20210926204127348](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926204128.png)

- `pidstat -d 时间间隔 -p 进程号`



### ifstat：

- 默认本地没有，要下载的嗷！！！

![image-20210926204425422](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926204426.png)



## CPU占用过高，分析+定位？

- Linux加上JDK共同分析

- 步骤：

  1. top命令找出惹事儿的Java程序，记录它的id
  2. ps -ef | grep 进程号或者 jps 进一步定位，得知怎样的后台程序在惹事儿
  3. 定位到具体的**线程**或者**代码：**

  **`ps -mp 进程 -o THREAD,tid,time`**

  tid是哪一个线程，time是某个线程已经耗费的时间

  参数解释：

  - -m显示所有的线程
  - -p pid进程使用cpu的时间
  - -o 该参数后面是用户自定义格式

  4. 将需要的**线程ID**转换为**16进制格式**（英文小写格式）

  ![image-20210926205054790](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926205055.png)

  或者手动算或用计算机算也成嗷！！！

  ![image-20210926205130839](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926205131.png)

  注意哈，机器内存里面全是小写嗷！！！

  5. `jstack 进程ID | grep tid（16进制线程id小写英文）-A60`

  -A60是打印出前60行嗷！！！

  ![image-20210926205848429](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210926205849.png)

  

# 14. Github骚操作

## 常用词：

- watch：持续获得某个项目更新的通知嗷
- fork：复制某个项目到自己的仓库中
- star：项目点赞数
- clone：把项目下载到你本地
- follow：关注感兴趣的作者，会收到他们的动态



## 具体骚操作：

### In关键词限制搜索范围：

- 公式：`xxx关键词 in:name 或 description 或 readme`

- 例如`seckill in:name`，项目的名字中必须有seckill这个关键字嗷！！！

- 组合使用：`seckill in:name,description`



### stars或fork项目查找项目：

- 公式：`xxx关键字 stars 通配符`
  - 例如`:> 或者 :>=`
  - 区间范围数字：`数字1...数字2`
- 例如：`springboot stars:>=5000`
- 例如：`springboot forks:>=500`
- 组合使用：`springboot forks:100..200 stars:2000..4000`



### awesome加强搜索：

- awesome系列一般是用来收集
- 查找官方收集的学习，工具，书籍类相关的项目
- 公式：`awesome 项目名字`
- 例如：`awesome redis`



### 高亮显示代码：

- 给别人指出关键代码的行数或者行号

- 公式：`项目对应代码的URL + #L + 数字`

- 例如：`https://github.com/coding/.../killDao.java#L13`

- 范围高亮：

  `项目对应代码的URL + #L + 数字 + - + L + 数字`

  例如：`https://github.com/coding/.../killDao.java#L13-L23`



### T搜索：

- 项目内搜索
- 在项目主页按下小写字母t，这个快捷键就是项目中搜索嗷！！！
- ![image-20210927102153357](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210927102201.png)
- 进入列表显示和搜索栏，可以列出项目中全部的文件，方便我们查找和检索阅读嗷！！！
- 还有好多好多快捷键嗷！！！

[github官网快捷键嗷！！！](https://docs.github.com/en/get-started/using-github/keyboard-shortcuts)





### 搜索某个地区的大佬：

- 公式：搜索栏中`location:beijing language:java`

![image-20210927102956944](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210927102957.png)























