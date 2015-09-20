---
layout: post
title: "Concurrence-23-Semaphores"
description: "翻译 梳理基础的东西"
category: 翻译 java 并发编程 多线程
tags: [翻译 并发编程 多线程]
---
#### Semaphores
<br/>
##### 信号量
<br/>

[文章的源地址](http://tutorials.jenkov.com/java-concurrency/semaphores.html)
<br/>
大意翻译，逐字逐句的翻译太累了，并且只为了翻译而翻译，不是看这篇文章的本意，
故采取大意翻译，当然首先还是得一句一句得看懂得，毕竟追求得就是看懂文章。   

信号量，也是一种并发的机制，可以被用来在线程之间发送信号，避免信号丢失；也
可以像锁一样保卫临界区的代码。java5以后，在java.util.concurrent包中有
Semaphore的实现，我们看一下它的实现原理。   

#### 简单的信号量
代码如下：   

```
public class Semaphore {
  private boolean signal = false;

  public synchronized void take() {
    this.signal = true;
    this.notify();
  }

  public synchronized void release() throws InterruptedException{
    while(!this.signal) wait();
    this.signal = false;
  }

}
```

take方法中发送一个信号，被储存在Semaphore上。  
release方法等待着一个信号。当接收到一个信号的时候，信号的标志位会被重新的
清洗，然后release方法会退出。     

使用信号量，可以避免信号的丢失。你可以使用take方法代替notify，使用release
代替wait，如果调用take方法在release方法之前，那么线程在调用release的时候
将会知道take已经被调用了，因为信号被存储在signal变量里面。wait和notify方
法不能够做到这一点。     

take 和 release 方法的成名比较的古怪。    

#### 使用信号量通信

两个线程之间使用Semaphore通信：

```
Semaphore semaphore = new Semaphore();

SendingThread sender = new SendingThread(semaphore);

ReceivingThread receiver = new ReceivingThread(semaphore);

receiver.start();
sender.start();
```

```
public class SendingThread {
  Semaphore semaphore = null;

  public SendingThread(Semaphore semaphore){
    this.semaphore = semaphore;
  }

  public void run(){
    while(true){
      //do something, then signal
      this.semaphore.take();

    }
  }
}
```

```
public class RecevingThread {
  Semaphore semaphore = null;

  public ReceivingThread(Semaphore semaphore){
    this.semaphore = semaphore;
  }

  public void run(){
    while(true){
      this.semaphore.release();
      //receive signal, then do something...
    }
  }
}
```
#### 计数信号量

上面Semaphore
























