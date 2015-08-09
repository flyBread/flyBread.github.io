---
layout: post
title: "Concurrence-12-Java's Volatile Keyword"
description: "翻译 梳理基础的东西"
category: 翻译 java 并发编程 多线程
tags: [翻译 并发编程 多线程]
---
#### Java's Volatile Keyword
<br/>
##### java的volatile关键字
<br/>

文章的地址：[翻译文章的源地址](http://tutorials.jenkov.com/java-concurrency/volatile.html)
<br/>
Java的volatile关键字被用来标记Java变量为"被存储在主存中"。更确切地说这意味着，
每一个volatile变量的读都是从计算机内存中读取，而不是从CPU缓存，每个写入volatile变量将被写入内存，
而不只是CPU缓存。     

事实上，自从java5以来，volatile保证的不只是volatile变量不只是从内存中读和写向内存，下面的内容也会
详细的加以解释。   

####Java挥发保证变量的可见性
java的volatile关键字保证变量的变动对线程的可见性，这个听起来比较抽象，我们慢慢的解释。   

在多线程应用程序中的线程运行在非volatile变量，出于性能方面的原因，每个线程都可以从主内存中的变量复制到
一个CPU缓存，而对它们进行操作。如果您的计算机包含多个CPU，每个线程都可以在不同的CPU上运行。这意味着，
每个线程可以将变量复制到不同的cpu的CPU缓存。此图说明了这种情况：     

![](http://tutorials.jenkov.com/images/java-concurrency/java-volatile-1.png)  

没有volatile变量，就不能够保证JVM从内存中读入cpu缓存中，或者把数据从CPU缓存写回内存，案例解释：   
假设多个线程访问一个共享的变量，这个变量有如下声明的counter变量：   

```
public class SharedObject {
    public int counter = 0;
}
```  

线程1可以读取共享变量counter的值0 并把它写入了CPU缓存，并且把值增加到1，但是没有把变动的值写回内存，线程2
在去读取同样的共享变量counter值的时候，从内存中还是0，然后把它读入CPU缓存，然后增加一，也没有把数据缓存写
回内存。线程1和线程2现在几乎是同步。真正共享计数器变量的值应该是2，但是两个线程在CPU缓存中的变量值都是1，
而且内存里面的值是0,一团糟啊，即使线程把他们CPU缓存中的关于共享变量的值写回内存，那也是错误的。      

通过声明counter是volatile变量，java虚拟机就能够保证，变量的每次读取都是从内存中读取的，所有的写操作都会
写到内存中，下面就是声明counter为volatile变量的代码：    

```
public class SharedObject {
    public volatile int counter = 0;
}
```     

某种条件下，简单的声明一个变量为volatile可能并不够保证多线程访问的是最新的写入的值，下面会有一些情况的介绍。    

在某些情况下，两个线程针对同一个变量的读和写，简单的声明volatile变量并不足以应该这种情况，例如线程1可能把
counter的值0读入了cpu1的寄存器里面，同时或者在这之后，线程2也把值0读入了cpu2的寄存器里面。两个线程都是
同时从内存中直接的读取counter的值，现在两个变量全部增加一，并且把值写回到内存中，他们会把他们寄存器版本写
回counter，两个都把1写回内存中。但是两次增加后，counter的值应该是2。    

关于多线程的一个线程，不能够看到变量的最新的值因为这个值还没有被另外更新的线程写入内存里面，叫做可见性的问题
一个线程的更新对另一个线程不可见的。



 
















