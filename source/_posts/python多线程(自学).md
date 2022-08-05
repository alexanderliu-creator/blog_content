---
title: Python多线程
date: 2021-02-20 16:05:21
tags: 
	Python


---



# Python多线程运用

---



<!--more-->





#  Threading

- 一些基本常用的功能：
  - .active_count()   （有几个线程存在）
  - .start()   （开始运行某个线程）
  - threading.Thread(target = ... ， name = ...)   （创建一个新的线程）
    - 一般来说还要定义一个函数，就是你要干什么，然后把具体的函数传入到线程中，初始化创建线程的时候，赋值给target
  - .enumerate()    (显示线程具体的信息)
  - .current_thread   (现在正在运行的线程)



- join功能：
  
  - .join()   等待上面的线程运行完了，join()指令下面的命令才会执行嗷！
  
  其实本质上不是这样理解的哦，join()指令是让主函数main()等待它的执行完成





- queue功能：

  - 原理: 多线程内的return函数失效了，一般数据先暂存于queue中，等待程序线程全部执行完毕后，再打印输出

  - 首先多线程的数据如果像保存一般都是用queue来保存的

  - 先初始化每个线程，让他们执行特定功能，然后一定要用join()，等所有线程的程序都运行完了，再去读取存在queue中的数据，然后最后打印输出

  - 基本函数：

    - ```python
      def multithreading():
          q = Queue()
          threads = []
          data = [[1,2,3],[3,4,5],[4,4,4],[5,5,5]]
          for i in range(4):
              t = threading.Thread(target=job,args=(data[i],q))
              t.start()
              threads.append(t)
          # 相当于添加了四个线程
          for thread in threads:
              thread.join()
          results=[]
          
          for _ in range(4):
              results.append(q.get())
          print(results)
      ```

    - 同时，每个函数的运行结果，要用q.put(result)捕捉，线程运行完了之后，再去用q.get()来进行提取。



- lock功能：
  - 如果多个线程同时处理一个变量，那么就会直接爆炸，不知道处理的啥，这个时候就应该锁死一个线程，当这个线程运行结束之后，再去运行另一个线程，以避免冲突！
  - lock = threading.Lock()
  - global lock引入这个锁之后，在线程运行之前和之后，可以用lock.acquire()和lock.release()来指定线程的锁死和结束

