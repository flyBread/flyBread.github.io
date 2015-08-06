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
Java的volatile关键字被用来用于Java变量标记为"被存储在主存中"。更确切地说这意味着，每一个volatile变量的读将从计算机内存中读取，而不是从CPU缓存，每个写入volatile变量将被写入内存，而不只是CPU缓存。


 
















