---
layout: post
title:  CyclicBarrier在java中是什么
categories:
- Java并发
tags:
- Java锁
---

[原文链接 What is CyclicBarrier Example in Java 5 – Concurrency Tutorial](http://javarevisited.blogspot.com/2012/07/cyclicbarrier-example-java-5-concurrency-tutorial.html)

## CyclicBarrier 在Java中是什么

CyclicBarrier是在Java5的java.util.Concurrent包中被引入的一个同步计数器，伴随的其他的同步并发组件有 Counting Semaphore, BlockingQueue, ConcurrentHashMap 等。CyclicBarrier跟我们后面介绍的CountDownLatch是相似的，CyclicBarrier允许多个线程在屏障处相互等待。CountDownLatch和CyclicBarrier的区别在多线程面试中经常被问到。CyclicBarrier是并发程序的一个再常见不过的要求，因为它可以用来执行最后一部分任务一旦单个任务度完成了。所有的线程在屏障处相互等待，CyclicBarrier初始化的时候指定等待线程的个数，线程通过调用CyclicBarrier的await()方法相互等待，直到所有的线程都调用await()方法。一般来说调用await()方法就是告诉线程你要在屏障处等待。虽说await()方法是一个阻塞方法。但是也能被超时中断，或是被其他线程主动中断。在这篇并发教程中，我们将了解CyclicBarrier是什么，例子就是3个线程相互等待以进行下一步处理的CyclicBarrier示例。


## CountDownLatch和CyclicBarrier的区别

CountDownLatch的count不能重用，而CyclicBarrier的cout值是可以重用的。  
这意味着CountDownLatch适合处理一次性事件，而CyclicBarrier适合处理重复性发生的事件和复杂问题并行计算等场景。如果你对Java的线程和并发感兴趣，你可以看看我相关帖子[何时使用Volatile](http://javarevisited.blogspot.sg/2011/06/volatile-keyword-java-example-tutorial.html)和[Java同步是如何工作的](http://javarevisited.blogspot.sg/2011/04/synchronization-in-java-synchronized.html)

## CyclicBarrier 示例

下面是一个CyclicBarrier的简单示例，3个线程为了能够顺利通过屏障，3个线程需要调用await()方法，简而言之，他们不会继续处理直到3个线程都达到屏障点，一旦三个线程都到达屏障点，屏障将放开，每个线程都在屏障点继续执行各自的任务。

```java

import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;
import java.util.logging.Level;
import java.util.logging.Logger;

/**
 * Java program to demonstrate how to use CyclicBarrier in Java. CyclicBarrier is a
 * new Concurrency Utility added in Java 5 Concurrent package.
 *
 * @author Javin Paul
 */
public class CyclicBarrierExample {

    //Runnable task for each thread
    private static class Task implements Runnable {

        private CyclicBarrier barrier;

        public Task(CyclicBarrier barrier) {
            this.barrier = barrier;
        }

        @Override
        public void run() {
            try {
                System.out.println(Thread.currentThread().getName() + " is waiting on barrier");
                barrier.await();
                System.out.println(Thread.currentThread().getName() + " has crossed the barrier");
            } catch (InterruptedException ex) {
                Logger.getLogger(CyclicBarrierExample.class.getName()).log(Level.SEVERE, null, ex);
            } catch (BrokenBarrierException ex) {
                Logger.getLogger(CyclicBarrierExample.class.getName()).log(Level.SEVERE, null, ex);
            }
        }
    }

    public static void main(String args[]) {

        //creating CyclicBarrier with 3 parties i.e. 3 Threads needs to call await()
        final CyclicBarrier cb = new CyclicBarrier(3, new Runnable(){
            @Override
            public void run(){
                //This task will be executed once all thread reaches barrier
                System.out.println("All parties are arrived at barrier, lets play");
            }
        });

        //starting each of thread
        Thread t1 = new Thread(new Task(cb), "Thread 1");
        Thread t2 = new Thread(new Task(cb), "Thread 2");
        Thread t3 = new Thread(new Task(cb), "Thread 3");

        t1.start();
        t2.start();
        t3.start();
      
    }
}

Output:
Thread 1 is waiting on barrier
Thread 3 is waiting on barrier
Thread 2 is waiting on barrier
All parties are arrived at barrier, lets play
Thread 3 has crossed the barrier
Thread 1 has crossed the barrier
Thread 2 has crossed the barrier

```

## 何时使用CyclicBarrier

鉴于CyclicBarrier的性质，它可以非常方便的实现类似Java7中的Fork-join映射 化简类型的功能，将大任务拆分成小块，为了完成任务你需要收集各个小任务的输出结果等等。例如，为了计算印度的人口，你需要有4个线程分别计算东、西、南、北四个区域的人口，他们之间需要互相等待，一旦最后一个线程完成了任务，主线程才能把各个线程的结果相加并打印出来。在这些情况下你可以使用CyclicBarrier。

- 实现多玩家游戏，游戏不能开始直到所有的玩家都加入游戏
- 执行冗长的计算，将任务分解为较小的任务。大体上实现了map reduce技术

### CyclicBarrier的关键点

> * 一旦所有的线程到达屏障点，在创建CyclicBarrier时提供一个Runnable参数执行自己自己的任务当所有的线程都到达屏障点的时候。
> * 如果CyclicBarrier初始化定义了3个线程，意味着3个线程需要在屏障处调用await（）方法
> * 线程将在屏障点处被阻塞，直到所有的线程都到达屏障点或线程被中断或await超时
> * 如果另一个线程中断了正在等待中的线程，程序将抛出BrokernBarrierException异常

```java

java.util.concurrent.BrokenBarrierException
        at java.util.concurrent.CyclicBarrier.dowait(CyclicBarrier.java:172)
        at java.util.concurrent.CyclicBarrier.await(CyclicBarrier.java:327)
```

> * CyclicBarrier的rest()方法重置了屏障的初始化值，其他等待中的线程或是还没到达的线程都将被中止，并抛出java.util.concurrent.BrokenBarrierException.


上面就是我介绍的CyclicBarrier，何时使用CyclicBarrier，CyclicBarrier的简单示例，我们也了解了CyclicBarrier和CountDownLatch的区别，当你有一些想法，我们可以在并发代码中使用CyclicBarrier。