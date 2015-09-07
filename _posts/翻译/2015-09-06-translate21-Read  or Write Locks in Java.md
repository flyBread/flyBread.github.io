---
layout: post
title: "Read/Write Locks in Java"
description: "翻译 梳理基础的东西"
category: 翻译 java 并发编程 多线程
tags: [翻译 并发编程 多线程]
---
#### Read / Write Locks in Java
<br/>
##### java中的读锁/写锁
<br/>

文章的地址：[翻译文章的源地址](http://tutorials.jenkov.com/java-concurrency/read-write-locks.html)
<br/>

读写锁比文章 Locks in java 中描述的Lock实现更加的复杂。设想你有一个应用，需要读
或者写某些资源，但是写的操作比读要少，两个线程读取相同的资源的时候，不会引发问题。
所以多个线程在同一个时间访问资源没有问题。但是一个线程想要写入某些资源，在相同的
时间内不能够有线程读或者写这个资源。为了解决这个允许多个线程读，单个线程写的问题，
我们需要读写锁。   

java5中在java.util.concurrent包中，有读写锁的实现，但是我们了解他们实现背后的
原理还是很有用的。   


#### 读写锁的java实现

首先我们总写一下，资源读写权限的状态：   

读权限： 如果没有线程在写，并且没有线程获得写的权限      
写权限： 如果没有线程在读或者写     




























