---
title: OS课程实验
date: 2021-04-12 14:37:33
tags: 大二自学
categories: 自学内容或课程作业
---





# OS课程实验



<!--more-->



## 相关Linux操作

- `Ctrl+Alt+T`打开终端









## 实验相关内容：

### 哲学家就餐：

- 复现哲学家就餐问题
- 连续运行30次以上不死锁
- 问题：五个哲学家（五个线程，五个线程是一样的），五只筷子（互斥访问），两个基本状态（思考，进餐）
- 目标：一个哲学家拿到左右的筷子而不死锁。

### 解法:

1. 奇数先拿坐标,偶数先拿右边:

```c
#include <pthread.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

pthread_t philosopher[5];
pthread_mutex_t chopstick[5];

void random_sleep(void)
{
    int t = rand() % 3;
    sleep(t + 1);
}

void eat(int n)
{
    do
    {
        printf("Philosopher %d is thinking\n", n);
        random_sleep();

        printf("Philosopher %d is going to eat\n", n);
        if (n % 2 != 0)
        {
            pthread_mutex_lock(&chopstick[n]);
            pthread_mutex_lock(&chopstick[(n + 1) % 5]);
            printf("Philosopher %d is eating\n", n);
            pthread_mutex_unlock(&chopstick[n]);
            pthread_mutex_unlock(&chopstick[(n + 1) % 5]);
            printf("Philosopher %d finished eating\n", n);
        }
        else
        {
            pthread_mutex_lock(&chopstick[(n + 1) % 5]);
            pthread_mutex_lock(&chopstick[n]);
            printf("Philosopher %d is eating\n", n);
            pthread_mutex_unlock(&chopstick[(n + 1) % 5]);
            pthread_mutex_unlock(&chopstick[n]);
            printf("Philosopher %d finished eating\n", n);
        }

        random_sleep();
    } while (1);
}

int main(void)
{
    srand((unsigned)time(NULL));
    int i;
    for (i = 0; i < 5; i++)
    {
        pthread_mutex_init(&chopstick[i], NULL);
    }

    for (i = 0; i < 5; i++)
    {
        pthread_create(&philosopher[i], NULL, (void *)eat, (void *)i);
    }

    for (i = 0; i < 5; i++)
    {
        pthread_join(philosopher[i], NULL);
    }

    for (i = 0; i < 5; i++)
    {
        pthread_mutex_destroy(&chopstick[i]);
    }

    return 0;
}
```



2. 设置一个门槛 , 最多四个人抢筷子嗷!!!

```c
#include <pthread.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <semaphore.h>

pthread_t philosopher[5];
pthread_mutex_t chopstick[5];
sem_t count;

void random_sleep(void)
{
	int t = rand() % 3;
	sleep(t + 1);
}

void eat(int n)
{
	do
	{
		printf("Philosopher %d is thinking\n", n);
		random_sleep();

		printf("Philosopher %d is going to eat\n", n);
		sem_wait(&count);
		pthread_mutex_lock(&chopstick[n]);
		pthread_mutex_lock(&chopstick[(n + 1) % 5]);
		printf("Philosopher %d is eating\n", n);
		pthread_mutex_unlock(&chopstick[n]);
		pthread_mutex_unlock(&chopstick[(n + 1) % 5]);
		printf("Philosopher %d finished eating\n", n);
		sem_post(&count);

		random_sleep();
	} while (1);
}

int main(int argc, char const *argv[])
{
	/* code */
	//初始化信号量
	sem_init(&count, 0, 4); //初始化一个值为1的信号量

	srand((unsigned)time(NULL));
	int i;
	for (i = 0; i < 5; i++)
	{
		pthread_mutex_init(&chopstick[i], NULL);
	}

	for (i = 0; i < 5; i++)
	{
		pthread_create(&philosopher[i], NULL, (void *)eat, (void *)i);
	}

	for (i = 0; i < 5; i++)
	{
		pthread_join(philosopher[i], NULL);
	}

	for (i = 0; i < 5; i++)
	{
		pthread_mutex_destroy(&chopstick[i]);
	}

	return 0;

}
```



































## 操作设置：



### 线程相关：





1. pthread_create

```c
int pthread_create(pthread_t *restrict tidp,
				const pthread_attr_t *restrict attr,
				void *(*start_rtn)(void *), 
				void *restrict arg);

```

- 处于pthread.h

 tidp：指向线程ID的指针，当函数成功返回时将存储所创建的子线程ID

 attr：用于定制各种不同的线程属性（一般直接传入空指针NULL，采用默认线程属性）

 start_rtn：线程的启动例程函数指针，新创建的线程执行该函数代码

 arg：向线程的启动例程函数传递信息的参数

- 这里自己使用完之后给个总结：

  - pthread_create这个东西是用于线程的初始化，这里传的是你声明的pthread_t的指针.
  - 第二个默认为NULL不用管
  - 第三个是一个函数,就是你要跑的函数,注意,是函数,函数中用到的参数是由最后一个参数给的
  - 第四个是你要传入函数的参数

- 例子:

  ```c
  for (i = 0; i < 5; i++)
      {
          pthread_create(&philosopher[i], NULL, (void *)eat, (void *)i);
      }
  ```







2. pthread_exit

```c
void pthread_exit(void *rval_ptr);
```

- 处于pthread.h

主线程用于等待子线程的结束，实现多线程之间的调用和交互。调用该函数的父线程将一直被阻塞，直到指定的子线程终止. 

- 这个是线程的自我kill,和你在c的函数代码里面的exit是一样的嗷!!!





3. pthread_join

   - 若线程从启动例程返回，rval_ptr将包含返回码

   - 若线程被取消，由rval_ptr指定的内存单元就置为PTHREAD_CANCELED

   - 若线程由于pthread_exit终止，rval_ptr即pthread_exit的参数

   参数

    thread：需要等待的子线程ID

    rval_ptr：若不关心线程返回值，可直接将该参数设置为空指针NULL

   •若线程从启动例程返回，rval_ptr将包含返回码

   •若线程被取消，由rval_ptr指定的内存单元就置为PTHREAD_CANCELED

   •若线程由于pthread_exit终止，rval_ptr即pthread_exit的参数

   返回值

    成功返回0，否则返回错误编号

- 这个和Java一样,主线程等待子线程运行完成后再结束退出
- 例子: 

```c
for (i = 0; i < 5; i++)
	{
		pthread_join(philosopher[i], NULL);
	}
//philosopher是你初始化的进程
```








4. lpthread_self函数可以让调用线程获取自己的线程ID

   `pthread_t pthread_self();（pthread.h）`

   - 这个东西这次没用上,问题不大



5. create之后,子线程自动成立并且开始跑. Join的话,父线程会等待子线程结束再结束,此外,注意释放资源嗷!!!







### 互斥量:

- 互斥量的声明:
  
- `pthread_mutex_t mlock = PTHREAD_MUTEX_INITIALIZE`
  
- 初始化：`int pthread_mutex_init(pthread_mutex_t *mutex,const pthread_mutexattr_t *attr);`

- 销毁：`int pthread_mutex_destroy(pthread_mutex_t *mutex);`

- 加锁和解锁：

  `int pthread_mutex_lock(pthread_mutex_t *mutex);`

  `Int pthread_mutex_unlock(pthread_mutex_t *mutex);`

- 尝试加锁：

  `int pthread_mutex_trylock(pthread_mutex_t *mutex);`

  补充说明: `pthread_mutex_trylock() 在成功完成之后会返回零。其他任何返回值都表示出现了错误。如果出现以下任一情况，该函数将失败并返回对应的值。`

  调用该函数时，若互斥量未加锁，则锁住该互斥量，返回0；若互斥量已加锁，则函数直接返回失败（不会阻塞调用线程）

- 实例：

  ```c
  //…
  pthread_mutex_t mutex;
  pthread_mutex_init(&mutex, NULL);
  pthread_mutex_lock(&mutex);
  //do something
  pthread_mutex_unlock(&mutex);
  pthread_mutex_destroy(&mutex);
  //…
  ```









### 信号量：

![image-20210412145241195](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210412145241195.png)

![image-20210412145311824](https://gitee.com/alexs-rabbit//picture/raw/master/image-20210412145311824.png)

- 示例：

```c
sem_t sem;
sem_init(&sem, 0, 1);//初始化一个值为1的信号量
sem_wait(&sem);//获取信号量
//do somthing
sem_post(&sem);//释放信号量
sem_destroy(&sem);//销毁一个无名信号量
```

- 这个信号量就是我们口中的资源型信号量,它可以赋予初值的嗷!!!
- 上面两个经常混着使用嗷.