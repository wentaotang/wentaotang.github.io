---
layout: post
title:  java序列化
categories:
- Java序列化
tags:
- Java序列化
---

原文[链接](http://javarevisited.blogspot.sg/2014/05/why-use-serialversionuid-inside-serializable-class-in-java.html#more)   

###为什么在要在实现了序列化了类中使用SerialVersionUID   


序列化和序列化ID让很多java程序员一直很困惑，我经常看见一些类似这样的问题：  

1.什么是SerialVersionUID？   
2.如果我不声明SerialVersionUID在实现了Serialization的类中会有什么问题？


SerialVersionUID:可以理解为class文件的一个版本号

如果我不显示的声明一个SerialVersionUID，那么java自身的序列化机制也会为我们默认生成一个SerialVersionUID，   
这个机制对类的修改时非常敏感的，包括对类属性的修改，属性访问修饰符的修改，以及类实现的接口的修改，   甚至是不同的编译器都会导致生成不一样的SerialVersionUID   
这样就最终会造成反序列化失败，因此隐式的序列化ID是非常危险的，推荐显示的生成序列化ID
