---
layout: post
title: ArrayBlockingQueue学习
categories:
- juc
tags:
- juc
---


我们先来学习jdk对ArrayBlockingQueue的注释描述部分。   

1. 该类支持有界的阻塞队列，队列元素的顺序是FIFO(First in First Out),head元素是在队列中呆的时间最长的元素，tail元素是队列中呆的时间最短的元素。新的元素都是在队列的尾部(tail)插入.而获取操作则是在队列的头部(head)进行。

2. ArrayBlockingQueue是一个经典的有界缓冲区，固定大小的数组，插入元素的是生产者，取出元素的是消费者。类一旦被创建，队列的容量大小是不能被改变的。尝试向一个满了的队列新增(put)元素会导致操作阻塞，同理，当向一个空的队列取(take)元素的时候也会阻塞当前操作。

3. 该类对等待中的生产者线程和消费者线程支持有序的等待策略。默认情况下，顺序是不保证的。不过，一个包含boolean值的ArrayBlockingQueue的构造方法支持线程访问的有序访问。公平策略降低了吞吐量，不过减少了可变性及避免了线程饥饿。

我们先通过ArrayBlockingQueue的源码来学习学习。

```java


```

