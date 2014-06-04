---
layout: post
title:  CycliBarrier 
categories:
- 锁
tags:
- java锁
---

[原文地址](http://tutorials.jenkov.com/java-util-concurrent/cyclicbarrier.html)
 

> * 创建一个CyclicBarrier
> * 在屏障处(barrier)等待
> * CyclicBarrier 行为
> * CyclicBarrier 示例

java.util.concurrent.CyclicBarrier 的同步机制是通过一些算法同步多个线程前进。换句话说就是，在所有的线程没有到达屏障前，所有的线程必须相互等待。下面是一个简单的示例图：  
![图解](http://wentaotang.github.io/images/cyclic-barrier.png)




 







