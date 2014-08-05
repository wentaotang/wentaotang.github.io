---
layout: post
title:  ArrayList和Vector的区别
categories:
- Java集合
tags:
- ArrayList Vector
---

原文地址:[http://javarevisited.blogspot.com/2011/09/difference-vector-vs-arraylist-in-java.html](http://javarevisited.blogspot.com/2011/09/difference-vector-vs-arraylist-in-java.html)

ArrayList和Vector是在Java集合包中经常被用到的类，它们之间的区别也经常在第一轮面试或电话面试中经常被问到。虽然在我看来是一个非常简单的问题，知道什么时候该使用Vector而不是ArrayList是很重要的当你在做项目的时候。在这篇文章中我们将理解ArrayList和Vector的区别并尝试理解这些概念背后的区别。最终目标就是像熟悉你自己一样熟悉的区分ArrayList和Vector之间的区别。顺便说一下，从Java5开始新增了一个实现了List接口的叫做CopynOnWriteArrayList，CopynOnWriteArrayList跟Vector和ArrayList类似但是并发访问性能比Vector好。


在开始看Vector和ArrayList的区别之前，让我们先看看它们之间的相似点，为什么在某些情况下ArrayList可以替换Vector。

1. Vector和ArrayList都是基于内部数组实现下标访问
2. ArrayList和Vector记录着元素插入的顺序，意味着如果你遍历ArrayList或是Vector你得到的数据的顺序将是你插入数据时的顺序。
3. 基于ArrayList和Vector的Iterator和ListIterator访问都是快速失败的。
4. ArrayList和Vector允许插入Null值和重复的值

## Vector Vs ArrayList

现在让我们来看看Vector和ArrayList有哪些关键区别，这将决定什么时候该使用Vector而不是ArrayList，反之亦然。区别主要是基于同步、线程安全、速度、迭代列表、导航等属性考虑。

1. **同步和线程安全**
    首要的区别就是：Vector是线程安全的，ArrayList不是。就是说Vector拥有的类似结构修改的方法add()、remove()都是synchronized的，它可以在多线程并发条件下安全的使用。而ArrayList的方法都不是同步的所以不适合在多线程并发的条件下使用。为什么ArrayList不能在多个线程之间共享这个问题经常在线程面试中出现。
2. **速度和性能**
    ArrayList一直比Vector快。因为Vector是同步的，线程安全的同步成本让它变慢。另一方面，ArrayList不是同步的，速度更快，在单线程环境下，显而易见我们应该选择ArrayList。在下列情况下你也可以在多线程环境下使用ArrayList：如果多个线程只是从ArrayList读取值，或是通过Collections.unmodifiableList(list)修饰过的List。
3. **容量**
    每当Vector越过设定的阀值，通过capacityIncrement指定属性值来调整容量，而ArrayList则是通过ensureCapacity()方法来调整容量。
4. **枚举和迭代器**
    Vector可以通过elements()方法返回枚举类型的，该方法不是快速失败的，而ArrayList的Iterator和ListIterator则是快速失败的，在我的文章[Iteratro和Enumeration的区别](http://javarevisited.blogspot.sg/2010/10/what-is-difference-between-enumeration.html)详细讨论了它们之间的不同。你也看看看一看   
5. **遗留**
    另一点值得记住的就是Vector在JDK1.0中就出现了，最初它并不是集合工具类的一部分，实在后面版本的重构中才实现了List接口，才变成了集合中的一部分.

考虑到Vector和ArrayList的这些特点，我的结论就是无论何时你应该使用ArrayList避免使用Vector除非你没有别的选择。如果你有多个线程读写只有少个线程写，你应该仔细考虑CopyOnWriteArrayList而不是Vetcor，因为CopyOnWriteArrayList能提供线程安全且对性能影响不大。


     相关的文章:  
    [Difference between String, StringBuffer and StringBuilder in Java](http://javarevisited.blogspot.com/2011/07/string-vs-stringbuffer-vs-stringbuilder.html)   
    [Difference between Comparator and Comparable with Example](http://javarevisited.blogspot.com/2011/06/comparator-and-comparable-in-java.html)

    [Difference between hashtable and hashmap in java](http://javarevisited.blogspot.com/2010/10/difference-between-hashmap-and.html)  



