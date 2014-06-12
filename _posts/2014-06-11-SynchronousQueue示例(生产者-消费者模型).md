---
layout: post
title:  SynchronousQueue示例 生产者-消费者模型
categories:
- Java并发
tags:
- Java锁
---

原文链接:[SynchronousQueue Example in Java - Produer Consumer Solution](http://javarevisited.blogspot.sg/2014/06/synchronousqueue-example-in-java.html#more)


SynchronousQueue是BlockingQueue的一个特殊类型，它的插入操作必须等待直到另外一个线程执行了删除操作，反之亦然。当你调用在SynchronousQueue上调用put()方法的时候它就阻塞直到另一个线程取出该队列的元素。类似的，如果一个线程想要删除一个元素，如果当前队为空，那么线程将会被阻塞直到另一个线程在队列中插入一个元素。你可以把SynchronousQueue想象成持着奥林匹克火炬奔跑的运动员，他们带着火炬奔跑并传递给在另一端等待的运动员。注意这个类的命名，你就能理解为什么取名叫SynchronousQueue，它把数据同步传递到其他线程。SynchronousQueue将等待另一方取走数据而不是插入数据后直接返回。如果你熟悉CSP和ADA，你知道同步队列类似于会和通道。他们非常适合于`传递设计`，为了处理相关的信息、任务、事件等，在一个线程中运行的对象必须与同步到另一个线程中。在早先的多线程教程中，我们知道可以通过wait和notify，BlockQueue来实现生产者-消费者模型，在这篇教程中，我们将知道使用SynchronousQueue也可以实现生产者-消费者模型。SynchronousQueue对生成线程和消费线程支持公平的顺序访问。默认情况下，顺序访问是不保证的，然而，通过SynchronousQueue的构造方法赋予一个true值就可以保证线程的顺序访问了。

## 使用SynchronousQueue 实现生产者-消费者模型

![示例](http://wentaotang.github.io/images/SynchronousQueue_in_Java.jpg)

就像我先前说的，在任何语言中没有比生产者-消费者模型更能让我们理解线程间的通信了.在生产者-消费者模型中，一个线程扮演生产者生产事件或任务，其他的线程扮演消费者。共享缓存扮演传输数据的介质，难点在于解决生产者-消费者的边界问题。缓存如果满了，生产者必须等待。同理。如果缓存空了，消费者也必须等待。在这篇教程中，我们将使用SynchronousQueue来解决同样的问题，不过它的容量为0.


### SynchronousQueue中的要点

1.  SynchronousQueue阻塞直到一个线程尝试向SynchronousQueue添加值，另一个线程准备取。
2.  SynchronousQueue容量为0
3.  SynchronousQueue通常被来实现直接传递的排队策略，一个线程传递给等待的线程，如果允许的话，创建新的线程，否则拒绝任务。
4. SynchronousQueue不允许添加Null元素，否则报NullPointException
5. 不像其他容器集合的方法，SynchronousQueue扮演一个空集合
6. 你不能访问SynchronousQueue队列的头部元素，因为仅仅当你尝试删除的时候，元素在存在。同样，你也不能插入元素，除非有其他的线程正在尝试删除元素。
7. SynchronousQueue上不能执行遍历访问。
8. SynchronousQueue带boolean值的构造方法，当为true时允许线程公平的访问。




