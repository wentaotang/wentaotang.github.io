---
layout: post
title: 实战OutOfMemory-学习
categories:
- jvm
tags:
- jvm
---


在回答这个问题前，我们先看看JVM的内存被划分成了哪些个区域

> * 方法区
> * java堆
> * 虚拟机栈
> * 本地方法栈
> * 程序计数器

根据java虚拟机规范，除程序计数器外，其他的区域都有抛出OutOfMemoryError的可能。

###java堆溢出

java堆是一个共享的内存区域，用来存放对象实例和数组，我们只要不断的创建对象，当对象容量达到堆的最大限制后就会抛出内存溢出。   

```java  
List<MyClass> list=new ArrayList<MyClass>();
while(true){
    MyClass my=new MyClass();
    list.add(my);
}
```

运行时添加如下jvm参数  -Xmx5m -Xms5m -XX:+HeapDumpOnOutOfMemoryError

###虚拟机栈和本地方法栈溢出    

虚拟机栈和本地方法栈都是描述的java方法执行的内存模型，每个方法执行的时候都会创建一个栈帧(stack Frame)用于存储局部变量表，操作栈，动态链接，方法出口等信息。  

```java
int statckLength=1;

@Test
public void test2(){
    statckLeak();
}

public void stackLeak(){
    stackLength++;
    stackLeak();
}
```


运行时添加如下jvm参数： -Xss512k -XX:+HeapDumpOnOutOfMemoryError   


###运行时常量池溢出   

如果要想运行时常量池中添加内容，最简单的方法就是使用String.intern()这个方法。该方法的作用就是如果常量池中已经包含一个等于此字符串的String，则返回常量池中的这个String对象，否则将这个String对象添加到常量池中并返回此String对象的引用。    

```java
List<String> list=new ArrayList<String>();
int i=0;
while(true){
    list.add(String.valueOf(i++).intern());
}
```

因为运行时常量池是隶属于方法区，在sun的Hotspot虚拟机机上也俗称perm space(永久区)，所以在运行的时候我们需要添加如下jvm参数：   
-XX:PermSize=2m -XX:MaxPermSize=2m -XX:+HeapDumpOnOutOfMemoryError

在控制台上，我们将看到如下输出：  
```java
java.lang.OutOfMemoryError: PermGen space
```

###本机直接内存溢出   

DirectMemory容量可直接通过-XX:MaxDirectMemeorySize指定，如果不指定，则默认与java堆的最大值(-Xmx指定)一样。 

```java
@Test
    public void test4() throws NoSuchFieldException, SecurityException, IllegalArgumentException, IllegalAccessException {
        Field theUnsafeInstance = Unsafe.class.getDeclaredField("theUnsafe");
        theUnsafeInstance.setAccessible(true);
        Unsafe unsafe=(Unsafe) theUnsafeInstance.get(Unsafe.class);
        while (true) {
            unsafe.allocateMemory(1024 * 1024);
        }
    }
```

运行是添加如下jvm参数：-XX:MaxDirectMemorySize=512k 




## 参考  
1. [深入理解JVM虚拟机]

