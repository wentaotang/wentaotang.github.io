---
layout: post
title:  CountDownLatch 译文
categories:
- 锁
tags:
- java锁
---

java.util.concurrent.CountDownLatch 是一个允许一个或多个线程等待直到给定的一组操作完成的并发结构。   

CountDownLatch  根据初始设定值的count进行初始化，count值每调用`countDown()` 方法就减1，等待的线程直到count=0，才能调用`await()` 方法，调用`await()`阻止线程直到count值=0  

下面是一个简单的示例，在Decrementer 类的CountDownLatch 上调用`countDown()`方法三次后，等待中的Waiter通过`await()`方法被释放了。  

```java

CountDownLatch latch = new CountDownLatch(3);

Waiter      waiter      = new Waiter(latch);
Decrementer decrementer = new Decrementer(latch);

new Thread(waiter)     .start();
new Thread(decrementer).start();

Thread.sleep(4000);

```

```java

public class Waiter implements Runnable{

    CountDownLatch latch = null;

    public Waiter(CountDownLatch latch) {
        this.latch = latch;
    }

    public void run() {
        try {
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Waiter Released");
    }
}

```

```java

public class Decrementer implements Runnable {

    CountDownLatch latch = null;

    public Decrementer(CountDownLatch latch) {
        this.latch = latch;
    }

    public void run() {

        try {
            Thread.sleep(1000);
            this.latch.countDown();
            System.out.println("第一个任务完成");

            Thread.sleep(1000);
            this.latch.countDown();
            System.out.println("第二个任务完成");

            Thread.sleep(1000);
            this.latch.countDown();
            System.out.println("第三个任务完成");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```   
