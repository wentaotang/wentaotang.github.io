---
layout: post
title:  CountDownLatch在java中是什么
categories:
- Java并发
tags:
- Java锁
---

[原文链接-What is CountDownLatch in Java - Concurrency Example Tutorial](http://javarevisited.blogspot.com/2012/07/countdownlatch-example-in-java.html)

## CountDownLatch是什么

CountDownLatch在java中是一种同步器，它可以让一个线程在开始执行前等待一个或多个线程。这是一个非常重要的需求，在java服务器端核心应用中经常被用到。而CountDownLatch内置该功能极大的简化了开发工作。CountDownLatch 及其他并发组件比如CyclicBarrier, Semaphore, ConcurrentHashMap and BlockingQueue 都是在java 5中的java.util.concurrent包中引入介绍的。在这篇java并发教程中，我们将了解到CountDownLatch 在java中是干什么的？CountDownLatch 是怎么工作的？以及学习一些CountDownLatch的例子，最后就是CountDownLatch有哪些功能点值得关注。你也可以通过wait 和 notify机制实现CountDownLatch 同样的功能，但是在开始尝试的时候都是棘手的，而且代码量也很大。而通过CountDownLatch实现仅仅只需要几行代码。CountDownLatch允许主线程等待的线程个数是可变的。它可以等待1个或N个线程，而代码实现上没有多少变化的。关键在于：如果你理解了什么是CountDownLatch，CountDownLatch干了些什么，CountDownLatch是怎么工作的，那么你在java应用程序中何时需要使用CountDownLatch并不困难。

## CountDownLatch在java中是如何工作的

我们现在知道了CountdownLatch是什么，是时候知道它的锁工作原理了，主线程将一直等待直到门开，主线程将等待CountDownLatch创建时指定的N个线程。任意一个线程，一般是应用程序的主线程将调用CountDownLatch的await()方法，调用的线程将一直等待直到指定的count值=0或是调用线程被其他的线程中断。所有其他线程要求count计数下降通过调用CountDownLatch的countDown()方法，一旦count=0主线程就开始工作，也就是等待中的线程开始运行。CountDownLatch的一个缺点就是count值到达0后，就不能再重用了。但是不用担心，java并发集合提供另一个叫做CyclicBarrier能满足count重用的需求。

## CountDownLatch 示例

在这部分，我们将通过一个完整的例子来使用CountDownLatch，在下面的`CountDownLath示例`中，java程序要求在应用处理任何请求前，先执行CacheService, AlertService,ValidationService三个服务。这里我们通过CountDownLatch来构建实现。

```java

import java.util.Date;
import java.util.concurrent.CountDownLatch;
import java.util.logging.Level;
import java.util.logging.Logger;

/**
 * Java program to demonstrate How to use CountDownLatch in Java. CountDownLatch is
 * useful if you want to start main processing thread once its dependency is completed as illustrated in this CountDownLatch Example
 * 
 * @author Javin Paul
 */
public class CountDownLatchDemo {

    public static void main(String args[]) {
       final CountDownLatch latch = new CountDownLatch(3);
       Thread cacheService = new Thread(new Service("CacheService", 1000, latch));
       Thread alertService = new Thread(new Service("AlertService", 1000, latch));
       Thread validationService = new Thread(new Service("ValidationService", 1000, latch));
      
       cacheService.start(); //separate thread will initialize CacheService
       alertService.start(); //another thread for AlertService initialization
       validationService.start();
      
       // application should not start processing any thread until all service is up
       // and ready to do there job.
       // Countdown latch is idle choice here, main thread will start with count 3
       // and wait until count reaches zero. each thread once up and read will do
       // a count down. this will ensure that main thread is not started processing
       // until all services is up.
      
       //count is 3 since we have 3 Threads (Services)
      
       try{
            latch.await();  //main thread is waiting on CountDownLatch to finish
            System.out.println("All services are up, Application is starting now");
       }catch(InterruptedException ie){
           ie.printStackTrace();
       }
      
    }
  
}

/**
 * Service class which will be executed by Thread using CountDownLatch synchronizer.
 */
class Service implements Runnable{
    private final String name;
    private final int timeToStart;
    private final CountDownLatch latch;
  
    public Service(String name, int timeToStart, CountDownLatch latch){
        this.name = name;
        this.timeToStart = timeToStart;
        this.latch = latch;
    }
  
    @Override
    public void run() {
        try {
            Thread.sleep(timeToStart);
        } catch (InterruptedException ex) {
            Logger.getLogger(Service.class.getName()).log(Level.SEVERE, null, ex);
        }
        System.out.println( name + " is Up");
        latch.countDown(); //reduce count of CountDownLatch by 1
    }
  
}

Output:
ValidationService is Up
AlertService is Up
CacheService is Up
All services are up, Application is starting now

```

通过这个示例的输出结果，你能看到应用没有开始直到所有的service服务各个都完成。

## 我们何时需要使用CountDownLatch

当类似上面那种要求main线程wait直到一个或N个线程完成。使用CountDownLatch的经典例子是基于面向服务架构的任何服务器端程序。多个服务由多个线程提供，直到所有的serivces服务完成后，应用才启动，就像我们示例中的那样。

### CountDownLatch 注意问题
几个要点关于CountDownLatch值得我么记住：  

- 你不能重复使用CountDownLatch 的count值一旦它到达0，这个也是CountDownLatch和CyclicBarrier的主要区别，而且在java核心概念的面试和多线程面试中经常被问到。
- 主线程在门栓（Latch）处的等待通过CountDownLatch的await()方法来实现，而其他线程通过调用CountDownLatch的countDown()方法通知主线程它已完成。


上面通过一个真实的示例向您展示什么是CountDownLatch，CountDownLatch是如何工作的，CountDownLatch是一个非常有用的java并发工具，如果你掌握了何时使用CountDownLatch和怎么使用CountDownLatch，你将能够在java通信中减少大量的复杂的并发控制代码。  


本博客其他的java并发例子：

- [When to use ThreadLocal variable in Java](http://javarevisited.blogspot.sg/2012/05/how-to-use-threadlocal-in-java-benefits.html)
- [Difference between Runnable and Thread in Java](http://javarevisited.blogspot.sg/2012/01/difference-thread-vs-runnable-interface.html)
- [How to stop Thread in Java](http://javarevisited.blogspot.sg/2011/10/how-to-stop-thread-java-example.html)
- [How to fix deadlock in Java](http://javarevisited.blogspot.sg/2010/10/what-is-deadlock-in-java-how-to-fix-it.html)
- [How to find race conditions in Java](http://javarevisited.blogspot.sg/2012/02/what-is-race-condition-in.html)
- [How to write Thread-safe code in Java](http://javarevisited.blogspot.sg/2012/01/how-to-write-thread-safe-code-in-java.html)


