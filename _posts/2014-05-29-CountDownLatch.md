---
layout: post
title:  CountDownLatch 
categories:
- Java并发
tags:
- Java锁
---

CountDownLatch，在我初次接触这个类的时候，确实是有些迷茫，自己也在网上找了很多的资料，但始终是处于一种似懂非懂的状态。

CountDownLatch的javadoc：   

/**
 * A synchronization aid that allows one or more threads to wait until
 * a set of operations being performed in other threads completes.
 */   

大意是允许`一个或多个`线程等待直到其他N个线程的一系列操作完成。感觉还是有些别扭。

在看完徐明明的这篇blog：
http://xumingming.sinaapp.com/215/countdownlatch-vs-cyclicbarrier/
后，我感觉讲到点子上了，侧重点不一样。

那我们就以代码来展示下CountDownLatch的用法:  

适用场景：通俗的讲就是说我们在做某件事的时候，它的前提条件必须是成立的，，否则这事没法做.只能等待。等待前提条件成立，再执行。   
下面我们来模拟下去医院看病的情形吧。   
大致流程是这样的：   

 1. 排队(帝都人比较多，大家懂的) 
 2. 挂号
 3. 通知看病（等待叫号）
 
我参考了一个[blog](http://javarevisited.blogspot.com/2012/07/countdownlatch-example-in-java.html)的代码（可能需要翻墙），我稍微修改了下。   

```java
import java.util.concurrent.CountDownLatch;

public class CountDownLatchDemo {
	
	public static void main(String[] args) throws InterruptedException {
		//定义看病的3个前提条件
		final CountDownLatch latch = new CountDownLatch(3);
		Thread watting = new Thread(new Service("排队", 5000,latch));
		Thread register = new Thread(new Service("挂号", 1000,latch));
		Thread notice = new Thread(new Service("通知看病",5000, latch));
	
		watting.start(); 
		//watting.join()
		register.start(); 
		//register.join();
		notice.start();
	    //notice.join();  
       try{
            //main thread is waiting on CountDownLatch to finish
            latch.await();  
            //看病任务
            System.out.println("终于轮到我看病了，我容易吗");
       }catch(InterruptedException ie){
           ie.printStackTrace();
       }
	}
}

class Service implements Runnable{
    private final String name;
    private final int time;  //耗时
    private final CountDownLatch latch;
  
    public Service(String name, int timeToStart, CountDownLatch latch){
        this.name = name;
        this.time = timeToStart;
        this.latch = latch;
    }
  
    @Override
    public void run() {
        try {
            Thread.sleep(time);
        } catch (InterruptedException ex) {
            ex.printStackTrace();
        }
        System.out.println( name);
        //reduce count of CountDownLatch by 1
        latch.countDown(); 
    }
}
```   

运行结果，大家一定觉得有歧义，其实是我这个例子没有举好，
因为这个3个条件在现实生活中是有依赖关系的，如果大家硬要按流程来办事的话，可以使用join来完成。取消注释就可以了。但是我举这个例子主要是让大家更好的理解CountDownLatch。就是要干某件事的时候，若干前提条件一定要满足(前提条件之间最好没有依赖关系)，这个时候就比较适合采用CounDownLatch来完成。






