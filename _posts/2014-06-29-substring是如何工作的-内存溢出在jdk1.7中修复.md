---
layout: post
title:  substring是如何工作的-内存溢出在jdk1.7中修复
categories:
- String
tags:
- String
---

## subString是如何工作的-  内存溢出问题在jdk1.7中修复

substring方法是String类最常用的方法之一，在Java字符串面试中经常被问到。例如，substring是如何工作的？或者有时候问substring是如何造成内存溢出的？ 为了回答这些问题，你需要知道它是如何实现的。最近我一个朋友在钻研Java面试中关于String的tostring方法，他使用substring很长一段时间了，当然我们也在用，令他感到奇怪的是面试官对substring的痴迷及深入分析它的实现层。尽管String是一个特殊的类，有很多关于String的面试问题，例如[为什么char Array比String更适合存储密码](http://javarevisited.blogspot.com/2012/03/why-character-array-is-better-than.html)。在这种情况下，substring方法站在了舞台的中央了。而我们大多数人只是使用substring方法然后就忘了。不是每个程序员都会深入代码查看它究竟是如何工作的。

**Update** ：这个问题确实是个bug，[http://bugs.sun.com/view_bug.do?bug_id=6294060](http://bugs.sun.com/view_bug.do?bug_id=6294060),这个问题在Java7中被修复。现在替换原先的共享原始character array，substring方法新建一个String对象copy原来的值。简而言之，substring方法仅保存它需要的数据。感谢Yves Gillet指出这个问题。由于我的一些读者指出java.lang.String类在jdk1.7中有一些变化，并且substring(offset,count)被移除了。这样每个String实例就可以节省一些字节，但是不共享原始数组使substring在指定时间前线性执行。不管怎么说，这是值得的移除String相关的内存溢出。说了这么多，如果你还没有升级到jdk1.7还是使用jdk1.6，这件事值得你注意下。

问题开始于正常聊天，面试官问"你使用过substring方法吗"？我朋友骄傲的回答说“是，使用过很多次”，面试官面带微笑的说“好，非常好“，
