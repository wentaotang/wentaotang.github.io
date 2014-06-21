---
layout: post
title:  String、StringBuffer、StringBuilder的区别
categories:
- String
tags:
- String
---

原文链接:[String vs StringBuffer vs StringBuilder in Java](http://javarevisited.blogspot.com/2011/07/string-vs-stringbuffer-vs-stringbuilder.html)


## String、StringBuffer、StringBuilder的区别

String是Java中最重要的类之一，Java初学者都是从使用著名的System.out.println()在控制台打印字符串状态开始的。许多初级Java程序员并不知道String对象是一个final类且是不可变的，每次修改String对象都会产生一个新的String对象。那么如何操作String对象才不会产生过多的String临时变量垃圾呢？StringBuffer和StringBuilder就是这个问题的答案。StringBuffer从jdk1.0开始就有了，属于老资历了，但是StringBuilder是到jdk5才新增的，类似相同的实现还有Enum、Generics(泛型)、 varargs methods(可变参数的方法)和自动装箱都是伴随jdk1.5新增进来的。无论你从事哪种类型的应用开发，你会发现String类型都是被大量的使用。如果你分析你的应用程序你会发现大量的字符串类型的垃圾，因为程序中创建了很多的临时字符串变量。在这篇教程中，我们将了解什么是String，String对象有哪些重要的属性？StringBuffer是什么？什么时候使用StringBuffer和StringBuilder？StringBuilder在哪些情况下可以替换StringBuffer？StringBuffer和StringBuilder的区别在Java核心问题经常被问到.那我们就先从String开始吧。

## String 对象

在我们看String和StringBuffer或StringBuilder的区别之前，我们先来看看String的一些基本属性。

1. **String是不可变对象：** String被设计成不可变的对象，具体原因你可以查看[这篇文章](http://javarevisited.blogspot.sg/2010/10/why-string-is-immutable-in-java.html).String的不可变性提供了很多好处，例如String对象的哈希值可以被缓存，让HashMap获取key的hash值更快，另一个理由就是[为什么HashMap中的key一般都是String类型的值](http://javarevisited.blogspot.com/2011/02/how-hashmap-works-in-java.html).因为
String是final类型的，所以它可以被安全的共享在多个线程之间且不需要做额外的同步。

2. 当我们使用双引号括起来的字符串，类似字符串"abcd"被称为字符串字面量，字符串字面量在字符串常量池中创建。当你比较2个字符串字面量使用“==” 它将返回TRUE，因为它们实际上是同一个字符串实例，任何对象比较使用等于操作符是不好的做法，你应该使用equals方法来比较相等。
3. “+”操作符是重载的用来连接2个字符串，内部的“+”操作符是通过StringBuffer或StringBuilder来实现。
4. 字符串其实是通过字符数组来实现的，使用UTF-16来格式化表示。String的subString可会造成内存泄露，因为一些字符数组在原字符串和SubString之间是共享的，这样就会造成原字符串无法进行垃圾回收。具体可以参考这篇文章[How SubString works in java](http://javarevisited.blogspot.sg/2011/10/how-substring-in-java-works.html)来详细了解。
5. String重写了equals()和hashcode()方法，如果2个字符串包含完全相同的字符顺序和字符大小写，它们就被认为是相等的。如果你想忽略大小写比较可以考虑使用equalsIgnoreCase()方法，可以看看[how to correctly override equals method in Javay](http://javarevisited.blogspot.com/2011/02/how-to-write-equals-method-in-java.html)来了解更多关于equals方法的最佳实践。另一个值得注意的就是；等于方法必须与String的compareTo方法一致，因为SortedSet、SortMap等是使用compareTo
来比较字符串的。
6. toString()方法提供了字符串表示的任何对象和对象中声明的对象，推荐其他的对象实现toString方法来提供字符串表示。
7. 你可以通过char数组、byte数组、String、StringBuffer、StringBuilder来创建字符串，String提供这些类型相应的构造函数来定义字符串。


## String的问题

String的一个最大优势不可变性如果没有正确的使用也会变成最大的问题。很多时候我们创建一个字符串并在它上面执行许多操作，例如转大小写，substring，连接其他字符串等等。因为String是一个不可变类，每次你一个新字符串被创建的同时旧字符串就被丢弃了，由此在堆中造成了许多临时垃圾。如果String是通过字符串直接量创建，它们就被保留在字符串池中。为了解决这个问题Java提供了StringBuffer和StringBuilder2个类，StringBuffer是一个比较老的类了，而StringBuilder是相对比较新的，在jdk1.5的时候引入进来的。

## String和StringBuffer的区别

String和StringBuffer最大的区别就是：String是不可变类，而StringBuffer是可变的类，意味着一旦你创建了一个StringBuffer对象，对它的任何修改都不会创建任何其他新对象。这个可变的属性让StringBuffer成为处理String对象的理想选择，你可以通过toString()方法把StringBuffer转换成String对象。String和StringBuffer的区别是一个经常被问到问题，而且在电话面试或第一轮面试中经常被提及。现在就还包括了StringBuilder，问一些String、StringBuffer、StringBuilder的区别。因此我们得好好准备下了，下一节中我们来看看StringBuffer和StringBuffer的区别。

## StringBuffer和StringBuilder的区别

StringBuffer非常适合处理可变的的字符串，但是它的一个缺点就是它所有的public方法都是同步的，保存了线程安全但是速度慢了。在jdk5提供了一个类似的类StringBuilder，它基本上是copy了StringBuffer只是它不是同步的。在大多数的情况下尽量尝试使用StringBuilder类而不是StringBuffer。你也可以使用“+”操作符来连接两个字符串，因为“+”操作符内部实际上是使用StringBuffer或StringBuilder的append()方法。如果你仔细对比下StringBuffer和StringBuilder你会发现它们完全相似，所有的API方法也都是类似的。另一方面，String和StringBuffer完全不同，它们的API方法也完全不同，StringBuilder和String也是类似的。

## 总结

1. String是不可变对象，而StringBuilder和StringBuffer是可变对象
2. StringBuffer是同步的，而StringBuilder不是同步的，速度比StringBuffer快。
3. "+"操作符的连接操作内部是使用StringBuffer或StringBuilder实现的。
4. 如果不可变使用String，如果你需要变且需要线程安全使用StringBuffer，如果你要求可变且不需要线程安全请使用StringBuilder。


上面就是关于String、StringBuilder、StringBuffer的讨论。这些区别能帮你避免常见的编码错误。从jdk5起，你可以使用“+”操作符或StringBuilder来连接两个字符串。

