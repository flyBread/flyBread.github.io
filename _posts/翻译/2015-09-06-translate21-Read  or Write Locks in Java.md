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

如果一个线程读取某一个资源，只要没有线程在写这个资源，或者没有线程拥有对这个资源
写的请求，就没有问题。这样可以通过上调写访问请求的优先级，表示写入请求比读请求
更重要。此外，如果读取是最常发生操作，我们就不提高写的优先级，因为可能
发生饥饿。请求写权限的线程将阻塞，直到所有的读权限的线程释放了ReadWriteLock锁
了。如果不断地授予新的读取访问线程，等待写入访问权限的线程将永远的阻止
，导致饥饿。如果没有当前已锁定读写锁的线程，或者为了写资源而请求读写锁的线程
，这个时候线程才能够被授予读取资源的权限。    

一个线程只有在没有线程读取资源，并且没有线程向这个资源写入，才能够授予写的权限。
除非我们关心请求写操作的线程之间的公平性，那么无论多少线程请求写的权限，都没有关
系(针对线程获得写权限)。    


带着上述简单的规则，我们实现一个如下的读写锁：    

```
public class ReadWriteLock{

  private int readers       = 0;
  private int writers       = 0;
  private int writeRequests = 0;

  public synchronized void lockRead() throws InterruptedException{
    while(writers > 0 || writeRequests > 0){
      wait();
    }
    readers++;
  }

  public synchronized void unlockRead(){
    readers--;
    notifyAll();
  }

  public synchronized void lockWrite() throws InterruptedException{
    writeRequests++;

    while(readers > 0 || writers > 0){
      wait();
    }
    writeRequests--;
    writers++;
  }

  public synchronized void unlockWrite() throws InterruptedException{
    writers--;
    notifyAll();
  }
}
```

这个读写锁拥有两个锁方法和两个释放锁的方法。一对锁与释放的方法是针对读权限的，
另一对是针对锁权限的。   

读权限的规则在lockRead方法中体现，所有的线程都能够得到读资源的权限，除非
这个时候已经有一个拥有写权限的线程，或者多个请求写权限的线程。    

写权限的规则在lockWrite方法中体现，一个线程能够得到写的权限，首先请求写的权限，
这个时候 writeRequests++ ，然后它会检查它能不能得到写的权限，如果这个时候
没有线程拥有读取资源的权限，并且也没有线程拥有写这个资源的权限，它就能够
得到写资源的权限。至于有多少个线程请求这个资源的写权限，没有关系。    

值得注意的是unlockRead 和 unlockWrite方法的调用，调用的是notifyAll方法
而不是notify方法，为了解释为什么，首先我们考虑下面的场景：   

在ReadWriteLock锁里面，现在有线程等待着读取资源的权限，还有线程等待着写资源
的权限，如果一个线程被notify方法唤醒，请求读取资源的权限，因为这个时候有写资源
的请求，然而，没有等待着写权限的线程被唤醒，所以什么也没有发生，也就是说这个
信号被“丢失”，什么也没有发生，没有哪一个线程获得写或者读取资源的权限。但是
调用notifyAll方法，所有等待着的线程都会被唤醒，检查状态看看能不能得到自己
希望的权限。    

调用notifyAll方法，还有其他的一个好处，当多个线程都在等待读取资源的时候，
而这个时候又没有线程读取写的权限，unlockWrite方法的调用，所有的线程都能够
得到读取资源的权限，而不是一个一个的唤醒。























