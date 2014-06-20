---
layout: post
title:  String vs StringBuffer vs StringBuilder
categories:
- String
tags:
- String
---

原文链接:[String vs StringBuffer vs StringBuilder in Java](http://javarevisited.blogspot.com/2011/07/string-vs-stringbuffer-vs-stringbuilder.html)


## String、StringBuffer、StringBuilder的区别

String是Java中最重要的类之一，Java初学者都是从使用著名的System.out.println()在控制台打印字符串状态开始的。许多初级Java程序员并不知道String对象是一个final类且是不可变的，每次修改String对象都会产生一个新的String对象。那么如何操作String对象才不会产生过多的String临时变量垃圾呢？StringBuffer和StringBuilder就是这个问题的答案。StringBuffer从jdk1.0开始就有了，属于老资历了，但是StringBuilder是到jdk5才新增的，类似相同的实现还有Enum、Generics(泛型)、 varargs methods(可变参数的方法)和自动装箱都是伴随jdk1.5新增进来的。无论你从事哪种类型的应用开发，你会发现String类型都是被大量的使用。如果你分析你的应用程序你会发现大量的字符串类型的垃圾，因为程序中创建了很多的临时字符串变量。在这篇教程中，我们将了解什么是String，String对象有哪些重要的属性？StringBuffer是什么？什么时候使用StringBuffer和StringBuilder？StringBuilder在哪些情况下可以替换StringBuffer？StringBuffer和StringBuilder的区别在Java核心问题经常被问到.那我们就先从String开始吧。

