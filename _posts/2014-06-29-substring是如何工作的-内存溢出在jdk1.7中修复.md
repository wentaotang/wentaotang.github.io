---
layout: post
title:  substring是如何工作的-内存溢出在jdk1.7中修复
categories:
- String
tags:
- String
---

原文链接：[How SubString method works in Java - Memory Leak Fixed in JDK 1.7](http://javarevisited.blogspot.com/2011/10/how-substring-in-java-works.html)

## subString是如何工作的-  内存溢出问题在jdk1.7中修复

substring方法是String类最常用的方法之一，在Java字符串面试中经常被问到。例如，substring是如何工作的？或者有时候问substring是如何造成内存溢出的？ 为了回答这些问题，你需要知道它是如何实现的。最近我一个朋友在钻研Java面试中关于String的substring方法，他使用substring很长一段时间了，当然我们也在用，令他感到奇怪的是面试官对substring的痴迷及深入分析它的实现层。尽管String是一个特殊的类，有很多关于String的面试问题，例如[为什么char Array比String更适合存储密码](http://javarevisited.blogspot.com/2012/03/why-character-array-is-better-than.html)。在这种情况下，substring方法站在了舞台的中央了。而我们大多数人只是使用substring方法然后就忘了。不是每个程序员都会深入代码查看它究竟是如何工作的。

**Update** ：这个问题确实是个bug，[http://bugs.sun.com/view_bug.do?bug_id=6294060](http://bugs.sun.com/view_bug.do?bug_id=6294060),这个问题在Java7中被修复。现在替换原先的共享原始character array，substring方法新建一个String对象copy原来的值。简而言之，substring方法仅保存它需要的数据。感谢Yves Gillet指出这个问题。由于我的一些读者指出java.lang.String类在jdk1.7中有一些变化，并且substring(offset,count)被移除了。这样每个String实例就可以节省一些字节，但是不共享原始数组使substring在指定时间前线性执行。不管怎么说，这是值得的移除String相关的内存溢出。说了这么多，如果你还没有升级到jdk1.7还是使用jdk1.6，这件事值得你注意下。

问题开始于正常聊天，面试官问"你使用过substring方法吗"？我朋友骄傲的回答说“是，使用过很多次”，面试官面带微笑的说“好，非常好“.接下来的问题就是你能解释下substring干了些什么吗?,我朋友得到了一个展示他才华的机会，他说substring方法是用来获取String字符串的一部分字符串，定义在java.lang.String类中，是一个重载的方法，一个版本的substring方法仅包含一个beginIndex的参数，返回从beginIndex到结尾的字符串。而其他就带有2个参数，一个是beginIndex，一个是endIndex,返回从beginIndex到endIndex-1间的字符串。他强调指出当你调用substring方法的时候将返回一个新的String对象，因为String是不可变量类。

接下来的问题是，如果beginIndex等于字符串的长度的时候会发生什么呢？它将不会抛出IndexOutOfBoundException异常而是返回空字符串。同样的情况，如果beginIndex=endIndex=字符串的长度，也是返回空字符串。当beginIndex是负数的时候，或beginIdex大于endIndex，或beginIndex大于字符串长度的时候，将返回StringIndexBoundException异常。

到目前为止一切顺利，我朋友很高兴，面试似乎也不错，知道面试官问他，你知道substring是如何工作的吗？许多Java面试者在这里败了下来，因为他们没有看过java.lang.String的代码，不知道substring方式是如何实现的。如果你看过String类的substring方法，你会指出它们是调用String(int offset,int count,char value[])构造方法创建了一个新的字符串对象。这里最有趣地方就是value[],它代表的是原先的字符串，因此，这里有什么问题呢？

万一你还没指出它的问题，如果原始的字符串非常长，大概有1GB的大小，无论要截取的子字符串是多么小，它将持有1GB的数组，这也将阻止原字符串无法进行垃圾回收，万一字符串没有任何关联对象了，很明显这会导致内存泄露，内存将保存这个对象即使这个对象已没有用了，这就是为什么substring能产生内存溢出问题。

很显然，面试官下一个问题就是你会怎么处理这个问题？

- 方法一：创建新的字符串： new String(str.substring(2));
- 方法二：使用intern() : str.substring(2).intern();