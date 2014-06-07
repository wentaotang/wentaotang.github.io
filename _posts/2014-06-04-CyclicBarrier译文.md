---
layout: post
title:  CyclicBarrier 译文
categories:
- Java并发
tags:
- Java锁
---

[原文地址](http://tutorials.jenkov.com/java-util-concurrent/cyclicbarrier.html)
 

> * 创建一个CyclicBarrier
> * 在屏障处(barrier)等待
> * CyclicBarrier 行为
> * CyclicBarrier 示例

java.util.concurrent.CyclicBarrier 的同步机制是通过一些算法同步多个线程前进。换句话说就是，在所有的线程没有到达屏障前，所有的线程必须相互等待。下面是一个简单的示例图：  
![图解](http://wentaotang.github.io/images/cyclic-barrier.png)

通过调用`CyclicBarrier`的`await()`方法让多个线程相互等待，一旦N个线程都到达屏障处的时候，所有的等待的线程都被唤醒继续执行。  

## 创建一个CyclicBarrier

当你创建一个CyclicBarrier你要指定当等待线程达到多少个的时候，就释放它们。下面教你如何创建一个CyclicBarrier。  

```java  
CyclicBarrier barrier = new CyclicBarrier(2);
```

## 在屏障处等待
下面是线程在屏障处等待  

```java
barrier.await();
```  

你也可以为等待的线程指定一个等待的时长，当超过指定的等待时长时，该线程就会被释放，即使还没达到设定的N个等待线程。  
现实例子(方便理解)：大家进出城门的时候，官府出了个规定：  

- 城外等待进城的人数到达50人的时候才打开城门。  
- 城门每个小时开一次，即使没有到达50人也开。

```java
barrier.await(1, TimeUnit.HOURS);
```

这些等待的线程会一直在屏障处等待，直到:   

- 最后一个线程达到
- 他线程调用该线程的`interrupt()` 方法
- 另一个等待的线程被中断
- 另一个等待的线程等待超时
- CyclicBarrier.reset() 被一些外部线程调用   


## CyclicBarrier 行为

CyclicBarrier 支持屏障行为(暂这么叫吧)，当最后一个线程到达的时候，该屏障行为就执行。  
我们可以通过CyclicBarrier的构造函数来运行`屏障行为`   

```java   
Runnable      barrierAction = ... ;
CyclicBarrier barrier       = new CyclicBarrier(2, barrierAction);
```   

## CyclicBarrier 示例
```java    
Runnable barrier1Action = new Runnable() {
    public void run() {
        System.out.println("BarrierAction 1 executed ");
    }
};
Runnable barrier2Action = new Runnable() {
    public void run() {
        System.out.println("BarrierAction 2 executed ");
    }
};

CyclicBarrier barrier1 = new CyclicBarrier(2, barrier1Action);
CyclicBarrier barrier2 = new CyclicBarrier(2, barrier2Action);

CyclicBarrierRunnable barrierRunnable1 =
        new CyclicBarrierRunnable(barrier1, barrier2);

CyclicBarrierRunnable barrierRunnable2 =
        new CyclicBarrierRunnable(barrier1, barrier2);

new Thread(barrierRunnable1).start();
new Thread(barrierRunnable2).start();
```
下面是CyclicBarrierRunnable 类：    

```java   
public class CyclicBarrierRunnable implements Runnable{
    CyclicBarrier barrier1 = null;
    CyclicBarrier barrier2 = null;
    public CyclicBarrierRunnable(CyclicBarrier barrier1,CyclicBarrier barrier2) {
        this.barrier1 = barrier1;
        this.barrier2 = barrier2;
    }

    public void run() {
        try {
            Thread.sleep(1000);
            System.out.println(Thread.currentThread().getName() +
                                " waiting at barrier 1");
            this.barrier1.await();

            Thread.sleep(1000);
            System.out.println(Thread.currentThread().getName() +
                                " waiting at barrier 2");
            this.barrier2.await();

            System.out.println(Thread.currentThread().getName() +
                                " done!");

        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (BrokenBarrierException e) {
            e.printStackTrace();
        }
    }
}
```

下面是控制台的输出，有可能Thread-1 和Thread-0 的顺序是不一样的   

Thread-1 waiting at barrier 1  
Thread-0 waiting at barrier 1  
BarrierAction 1 executed   
Thread-0 waiting at barrier 2  
Thread-1 waiting at barrier 2  
BarrierAction 2 executed   
Thread-0 done!  
Thread-1 done!  







 







