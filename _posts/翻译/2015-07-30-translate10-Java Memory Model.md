---
layout: post
title: "Concurrence-10-Java Memory Model"
description: "翻译 梳理基础的东西"
category: 翻译 java 并发编程 多线程
tags: [翻译 并发编程 多线程]
---
#### Java Memory Model
<br/>
##### Java 内存模型
<br/>

文章的地址：[翻译文章的源地址](http://tutorials.jenkov.com/java-concurrency/java-memory-model.html)
<br/>

Java内存模型说明了Java虚拟机是如何使用计算机内存(RAM)的。Java虚拟机是对整个电脑的模型化，
该模型自然的就包括内存模型-就是Java的内存模型。   

如果想要设计出来运行良好的多线程程序，理解java内存模型就是非常必要的。java内存模型说明了多个
线程是如何得到被其他线程写入的共享值，当必要的时候又是如何同步协作的访问共享变量。   

最初的Java内存模型是不完善的，所以Java 1.5中的Java内存模型进行了修订。此版本的Java内存模型
现在仍在使用Java 8中。    

java内存模型的内在
在JVM中，java内存模型内在的被分为了线程栈和内存堆，下面这张图可以从逻辑的角度说明java内存模型：

![](http://tutorials.jenkov.com/images/java-concurrency/java-memory-model-1.png)    

在JVM中运行的每一个线程都有自己的线程栈，线程栈中包含到达当前执行的代码线程必须调用的方法，我常
把这个称之为调用栈，随着线程执行代码，调用栈随之发生变化。    

线程栈中也会包含所有被执行方法（调用栈中的所有的方法）的局部变量，一个线程只能够访问它自己的线程
栈，局部变量只对创建它的那个线程可见，对其他的线程是不可见的。




































