---
title: 深入学习-Java基础
date: 2021-08-31 15:16:09
tags: 大三自学
---

# 这个是对于Java基础的补充学习嗷！！！



<!--more-->



## ConcurrentHashMap

- 多线程情况下，HashMap的put操作可能造成数据丢失，为了避免这种情况，用ConcurrentHashMap代替HashMap
- HashTable为啥线程完全？
  - synchronized来锁住整张Hash表实现线程安全，让线程独占，效率低下。
- ConcurrentHashMap就可以做到读取数据而不用加锁，内部结构也可以使得锁的粒度尽量的小，允许多个修改操作并发执行，重点在于使用了锁分段技术。使用了多个锁来控制对于Hash表不同部分进行的修改。
- 内部使用Segment（段）来表示这些不同的部分，每个段其实就是一个小的HashTable，它们有自己的锁。多个修改发生在不同的段上的时候，就可以并发执行。
- 定位操作：第一次Hash -> Segment , 第二次Hash -> 元素所在的链表头部。Hash过程比普通Hash要长，好处是，对于元素所在的Segment进行操作即可，不会影响到其他的Segment嗷！！！

![image-20210902193650404](https://cdn.jsdelivr.net/gh/alexanderliu-creator/cloudimg/img/20210902193650.png)

- 为什么要用二次Hash？
  - 构造分离锁
  - 对于map的修改不会锁住整个容器，提高并发能力嗷！！！
  - ConcurrentHashMap的时间比HashMap要长，因此不是并发的情况下就不要去使用ConcurrentHashmap了嗷！！！
- Java7之前主要采用锁机制（悲观），Java8之后采用CAS无锁算法！！！
- [知乎原文解答嗷！！！](https://zhuanlan.zhihu.com/p/104515829#:~:text=ConcurrentHashMap%E5%8F%AF%E4%BB%A5%E5%81%9A%E5%88%B0%E8%AF%BB%E5%8F%96%E6%95%B0%E6%8D%AE%E4%B8%8D%E5%8A%A0%E9%94%81%EF%BC%8C%E5%B9%B6%E4%B8%94%E5%85%B6%E5%86%85%E9%83%A8%E7%9A%84%E7%BB%93%E6%9E%84%E5%8F%AF%E4%BB%A5%E8%AE%A9%E5%85%B6%E5%9C%A8%E8%BF%9B%E8%A1%8C%E5%86%99%E6%93%8D%E4%BD%9C%E7%9A%84%E6%97%B6%E5%80%99%E8%83%BD%E5%A4%9F%E5%B0%86%E9%94%81%E7%9A%84%E7%B2%92%E5%BA%A6%E4%BF%9D%E6%8C%81%E5%9C%B0%E5%B0%BD%E9%87%8F%E5%9C%B0%E5%B0%8F%EF%BC%8C%E5%85%81%E8%AE%B8%E5%A4%9A%E4%B8%AA%E4%BF%AE%E6%94%B9%E6%93%8D%E4%BD%9C%E5%B9%B6%E5%8F%91%E8%BF%9B%E8%A1%8C%EF%BC%8C%E5%85%B6%E5%85%B3%E9%94%AE%E5%9C%A8%E4%BA%8E%E4%BD%BF%E7%94%A8%E4%BA%86%20%E9%94%81%E5%88%86%E6%AE%B5%E6%8A%80%E6%9C%AF,%E3%80%82%E5%AE%83%E4%BD%BF%E7%94%A8%E4%BA%86%E5%A4%9A%E4%B8%AA%E9%94%81%E6%9D%A5%E6%8E%A7%E5%88%B6%E5%AF%B9hash%E8%A1%A8%E7%9A%84%E4%B8%8D%E5%90%8C%E9%83%A8%E5%88%86%E8%BF%9B%E8%A1%8C%E7%9A%84%E4%BF%AE%E6%94%B9%E3%80%82%E5%AF%B9%E4%BA%8EJDK1.7%E7%89%88%E6%9C%AC%E7%9A%84%E5%AE%9E%E7%8E%B0%2C%20ConcurrentHashMap%E5%86%85%E9%83%A8%E4%BD%BF%E7%94%A8%E6%AE%B5%20%28Segment%29%E6%9D%A5%E8%A1%A8%E7%A4%BA%E8%BF%99%E4%BA%9B%E4%B8%8D%E5%90%8C%E7%9A%84%E9%83%A8%E5%88%86%EF%BC%8C%E6%AF%8F%E4%B8%AA%E6%AE%B5%E5%85%B6%E5%AE%9E%E5%B0%B1%E6%98%AF%E4%B8%80%E4%B8%AA%E5%B0%8F%E7%9A%84Hashtable%EF%BC%8C%E5%AE%83%E4%BB%AC%E6%9C%89%E8%87%AA%E5%B7%B1%E7%9A%84%E9%94%81%E3%80%82)



## 自旋锁与偏向锁：

- 自旋锁只能用于多核cup，如果是单核将无限制处于自旋状态。
- [偏向锁是啥](https://www.jianshu.com/p/36eedeb3f912)
- [自旋锁是啥](https://www.cnblogs.com/cxuanBlog/p/11679883.html)











