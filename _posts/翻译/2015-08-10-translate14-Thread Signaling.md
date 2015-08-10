---
layout: post
title: "Concurrence-14-Thread Signaling"
description: "翻译 梳理基础的东西"
category: 翻译 java 并发编程 多线程
tags: [翻译 并发编程 多线程]
---
#### Thread Signaling
<br/>
##### 线程通信
<br/>

文章的地址：[翻译文章的源地址](http://tutorials.jenkov.com/java-concurrency/thread-signaling.html)
<br/>

线程通信的目的是使线程能够发送信号给对方。此外，线程也能使线程结构其他线程发送的信号。
例如，一个线程B可能等待线程A的信号：数据已准备好处理。

##### 基于共享变量的通信

线程之间彼此之间发送信号的一个简单的方法是:把信号的值设置在在某些共享对象变量上。线程A设置成员布尔变量 
hasDataToProcess 为true，线程B可能会读这个成员变量 ，两者都在同步代码块中。下面就是一个简单的例子，说明
一个对象能够包含一个信号量，并且提供校验和设置的方法。   

```
public class MySignal{

  protected boolean hasDataToProcess = false;

  public synchronized boolean hasDataToProcess(){
    return this.hasDataToProcess;
  }

  public synchronized void setHasDataToProcess(boolean hasData){
    this.hasDataToProcess = hasData;  
  }

}
```

线程A和线程B为了能够相互通信协同工作，必须包含一个引用针对一个共享的MySignal实例的引用。如果线程A和B的
引用指向了不同的MySignal的实例，他们就不会发现彼此的信号。要处理的数据可以位于一个共享缓冲区，
与MySignal实例相隔离。     

##### 忙等待  

处理数据的线程B等待这数据准备好，可以处理数据。换句话，就是线程B等待线程A的信号：调用hasDataToProcess
方法返回true，下面就是线程B跑着的循环，等待着信号的到来。     

```
protected MySignal sharedSignal = ...

...

while(!sharedSignal.hasDataToProcess()){
  //do nothing... busy waiting
}
```   
循环会一直的运行，直到hasDataToProcess方法返回true，这个就叫做忙等待，线程当等待的时候，一直处于忙碌的状态。
















