# RT-Thread 物联网操作系统线程创建与管理

## 概述

RT-Thread是一个面向物联网的实时操作系统，它提供了丰富的线程管理功能，以支持多任务处理和实时调度。

## 线程创建

线程是RT-Thread中任务调度的基本单位，有`动态创建`和`静态创建`两种创建方式。

### 定义线程栈
线程栈是线程执行时的局部变量存储区，需要预先定义好：
```c
#define THREAD_STACK_SIZE 2048 // 定义线程栈大小
static rt_uint8_t thread_stack[THREAD_STACK_SIZE];
```

### 定义动态线程函数

线程函数是线程执行的入口点，需要实现具体的线程逻辑：

```c
static void thread_function(void *parameter)
{
    // 线程执行的代码
}
```

### 动态创建线程

使用`rt_thread_create`函数动态创建线程：

```c
rt_thread_t thread;
thread = rt_thread_create(
    "thread_name",    // 线程名称
    thread_function,  // 线程函数
    RT_NULL,          // 线程函数参数
    THREAD_STACK_SIZE, // 线程栈大小
    7,                // 线程优先级，7为最高
    10);              // 超时时间
if (thread != RT_NULL)
    rt_thread_startup(thread);
```
### 动态创建线程简单示例

```c
#include <rtthread.h>

#define DBG_TAG "main"
#define DBG_LVL DBG_LOG
#include <rtdbg.h>

rt_uint32_t test_stack[512];

void thread1t(void *param)
{
    while(1)
    {
		//线程所做的操作循环打印Hello World
        rt_kprintf("Hello World\n");
        rt_thread_mdelay(1000);
    }
}

int main(void)
{
	//动态创建线程，用thread1来接收创建线程的结果
    rt_thread_t thread1 = rt_thread_create("thread", thread1t, NULL, 1000, 1, 10);

    if (thread1 != RT_NULL)//判断线程创建是否成功
    {
        if (rt_thread_startup(thread1) == RT_EOK)//启动线程，同时判断线程是否成功启动
        {
            rt_kprintf("Thread1 started successfully.\n");
        }
        else
        {
            rt_kprintf("Failed to start Thread1.\n");
        }
    }
    else
    {
        rt_kprintf("Failed to create Thread1.\n");
    }

    return 0;
}

```

运行结果

![screenshot_image.png](https://oss-club.rt-thread.org/uploads/20240723/4af9bde339d5464991a91342518f64f9.png)

### 静态创建线程示例
```c
#include <rtthread.h>
#define DBG_TAG "main"
#define DBG_LVL DBG_LOG
#include <rtdbg.h>

/* 定义线程栈和控制块 */
static rt_uint32_t thread1_stack[512];
static struct rt_thread thread1;
static char thread1_name[RT_NAME_MAX] = "thread";

/* 线程函数 */
static void thread1_entry(void *param)
{
    while (1)
    {
        /* 线程所做的操作循环打印Hello World */
        rt_kprintf("Hello World\n");
        rt_thread_mdelay(1000); // 线程休眠1000毫秒
    }
}

/* 线程初始化函数 */
int thread1_init(void)
{
    /* 创建线程1 */
    rt_thread_init(&thread1, thread1_name, thread1_entry, RT_NULL,
                   thread1_stack, sizeof(thread1_stack),
                   1, 10);
    
    /* 启动线程 */
    if (rt_thread_startup(&thread1) == RT_EOK)
    {
        rt_kprintf("Thread1 started successfully.\n");
    }
    else
    {
        rt_kprintf("Failed to start Thread1.\n");
    }

    return 0;
}

/* 程序入口 */
int main(void)
{
    /* 调用线程初始化函数 */
    thread1_init();

    /* 主线程循环 */
    while (1)
    {
        rt_thread_mdelay(1000); // 主线程休眠1秒
    }

    return 0;
}
```

## 线程管理

### 线程同步

线程同步是多线程编程中的重要概念，RT-Thread提供了多种同步机制：

- **互斥锁（Mutex）**：确保同一时间只有一个线程可以访问共享资源。
- **信号量（Semaphore）**：用于控制对共享资源的访问数量。

### 线程通信

线程间通信是多线程程序中常见的需求，RT-Thread提供了以下通信机制：

- **消息队列（Mail Queue）**：线程间可以发送消息。
- **信号量（Semaphore）**：也可以用于线程间的同步和通信。

### 线程调度

RT-Thread支持基于优先级的抢占式调度策略，确保高优先级的线程能够及时得到处理。


## 心得体会

通过今天的学习，我对RT-Thread的线程创建和管理工作有了更深入的理解。线程是实现多任务并发执行的关键，而良好的线程管理能够提高系统的稳定性和效率。以及对git工具以及markdown文档写作有了更深的理解。

