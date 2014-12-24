---
layout: post
title: Java Integer 使用注意事项
categories:
- jdk
tags:
- jdk
---

**Integer**类是一个使用频率比较高的类，在使用的过程中，我们应该注意些什么呢？  

>**尽量使用Integer.valueof(int),而不是new Integer(int)**

答案是：缓存，我们先来看看如下的代码？

```java
 Integer a=Integer.valueOf(127);
 Integer b=Integer.valueOf(127);
 Integer c=new Integer(127);

 System.out.println(a==b);  //true
 System.out.println(a==c); // false
```

根据我们一般的认识，变量a和变量b都是Integer对象，a==b？ 这不科学啊，那我们先来一探究竟，上源码

```java
public static Integer valueOf(int i) {
        assert IntegerCache.high >= 127;
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```

if代码告诉我们，如果我们传进去的值是介于IntegerCache.low和IntegerCache.high之间的话，返回值实际上是从IntegerCache中直接返回来的，那么就能理解为什么上面示例代码中的变量`a==b`了，只有当超出这个返回的的时候才会通过new Integer(int) 的方式返回。那么问题来了，这个范围是多少呢？ 那我们只有再看看Integer的内部类IntegerCache类了。

```java
private static class IntegerCache {
static final int low = -128;
static final int high;
static final Integer cache[];

static {
    // high value may be configured by property
    int h = 127;
    String integerCacheHighPropValue =
        sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
if (integerCacheHighPropValue != null) {
    int i = parseInt(integerCacheHighPropValue);
    i = Math.max(i, 127);
    // Maximum array size is Integer.MAX_VALUE
    h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
}
high = h;

cache = new Integer[(high - low) + 1];
int j = low;
for(int k = 0; k < cache.length; k++)
    cache[k] = new Integer(j++);
}
}
```

通过代码我们知道该范围的最小值就是-128，最大值默认是127，所以一般默认情况下，low-high的范围就是[-128-127],通过以上的代码我们知道，最小值是不可改变的，但是这个最大值是可以改变的，我们只需要在编译的时候新增系统属性 `java.lang.Integer.IntegerCache.high=<size>`即可，如下所示：  
![jvm参数](http://wentaotang.qiniudn.com/jvm-D.png)  
或者新增JVM参数：  
`-XX:AutoBoxCacheMax=<size>`  


**参考文章：**  

1.http://stackoverflow.com/questions/15052216/how-large-is-the-integer-cache 
2.http://stackoverflow.com/questions/3934291/extending-java-integer-cahce




