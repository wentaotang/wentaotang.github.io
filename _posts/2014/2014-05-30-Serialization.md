---
layout: post
title:  java序列化
categories:
- Serialization
tags:
- Serialization
---

原文[链接](http://javarevisited.blogspot.sg/2014/05/why-use-serialversionuid-inside-serializable-class-in-java.html#more)   

###为什么在要在实现了序列化了类中使用SerialVersionUID   


序列化和序列化ID让很多java程序员一直很困惑，我经常看见一些类似这样的问题：
1.什么是SerialVersionUID？
2.如果我不声明SerialVersionUID在实现了Serialization的类中会有什么问题？