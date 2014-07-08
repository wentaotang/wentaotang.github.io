---
layout: post
title:  为什么wait,notify,notifyAll方法定义在Object而不是Thread类中
categories:
- Java线程
tags:
- 线程
---

原文链接:[Why wait, notify and notifyAll is defined in Object Class and not on Thread class in Java](http://javarevisited.blogspot.sg/2012/02/why-wait-notify-and-notifyall-is.html)

 
为什么wait,notify,notifyAll方法被定义在Object类而不是Thread类中是一个著名的核心Java面试题，该问题在面试2年，4年甚至高级别位置的Java开发中经常出现。这个问题的妙处在于能反映出面试者对wait,notify原理机制的了解，通过这个问题能看出wait和notify的特性，并确保他是否完全理解wait,notify。类似于[为什么Java不支持多继承](http://javarevisited.blogspot.com/2011/07/why-multiple-inheritances-are-not.html)，[为什么String是不可变类](http://javarevisited.blogspot.com/2010/10/why-string-is-immutable-in-java.html),这里可能有多个答案关于为什么wait和notify被定义在Object类，每个人都可以辩解这些理由。

在我所有的面试经历中我发现大部分2-3年的Java程序员对wait和notify还是比较迷惑的，当你要求他用wait和notify来编写代码的时候他们就比较纠结了。因此如果你正打算去面试，确保你对wait和notify的机制比较了解以便你在使用wait和notify实现生产消费者模型或阻塞队列时感觉比较自在一些。顺便说一下，这篇文章是延续我早期wait和notify的相关的文章，例如[Why Wait and notify requires to be called from Synchronized block or method](http://javarevisited.blogspot.com/2011/05/wait-notify-and-notifyall-in-java.html)和[Difference between wait, sleep and yield method in Java](http://javarevisited.blogspot.com/2011/12/difference-between-wait-sleep-yield.html),如果你没读过这写文章，你会发现这些文章非常有趣。


## 为什么wait，notify，notifyAll方法定义在Object中的理由

这里有一些想法关于为什么它们不应包含在Thread中，我想这个对我有点意义