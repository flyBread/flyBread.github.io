---
layout: post
title: "Concurrence-15-Thread Deadlock"
description: "翻译 梳理基础的东西"
category: 翻译 java 并发编程 多线程
tags: [翻译 并发编程 多线程]
---
#### Thread Deadlock
<br/>
##### 线程通信
<br/>

文章的地址：[翻译文章的源地址](http://tutorials.jenkov.com/java-concurrency/deadlock.html)
<br/>

##### 线程死锁

死锁就是多个线程等待着获取锁而被阻塞，而其他线程处于死锁状态下又保存着这些锁。当不同的线程在同一时间，征用
相同的锁，但是获取锁的顺序不同，就有可能出现死锁。    

例如，线程1有锁A,现在去锁B，线程2已经拥有锁B，但是现在去锁A，这种情况下死锁就会发生，线程1不会得到锁B，线程2
也就是永远不会得到锁A，并且它们也不知道死锁的发生，只是一直的等待对方的锁，A和B，永远的等待下去，这种情况就是
死锁。     

情景表示如下：   

```
Thread 1  locks A, waits for B
Thread 2  locks B, waits for A
```   

下面是TreeNode类,一个在不同情况下调用同步方法的的示例:   

```
public class TreeNode {
 
  TreeNode parent   = null;  
  List     children = new ArrayList();

  public synchronized void addChild(TreeNode child){
    if(!this.children.contains(child)) {
      this.children.add(child);
      child.setParentOnly(this);
    }
  }
  
  public synchronized void addChildOnly(TreeNode child){
    if(!this.children.contains(child){
      this.children.add(child);
    }
  }
  
  public synchronized void setParent(TreeNode parent){
    this.parent = parent;
    parent.addChildOnly(this);
  }

  public synchronized void setParentOnly(TreeNode parent){
    this.parent = parent;
  }
}
```  

如果一个线程调用parent.addChild(child)的同时，另外一个线程2调用child.setParent(parent),在相同的parent和
child实例上，一个死锁的情况就会发生，下面是示意的代码：    

```
Thread 1: parent.addChild(child); //locks parent
          --> child.setParentOnly(parent);

Thread 2: child.setParent(parent); //locks child
          --> parent.addChildOnly()
``` 

首先线程1调用 parent.addChild(child),因为addChild 方法是一个同步的方法，所以线程1 锁住了parent这个对象

线程2调用  child.setParent(parent),因为setParent 方法是一个同步的方法，所以线程2 锁住了child这个对象

现在 孩子节点和父亲节点全部被两个不同的线程加锁了，下一步，线程1试图调用 child.setParentOnly()方法，但是
孩子节点被线程2锁着，所以线程1阻塞了。线程2试图调用 parent.addChildOnly()方法，但是父节点被线程1锁着，然后
线程2也被阻塞了。这样两个线程都阻塞了，并且都在等待着对方拥有的锁。   

备注：在上面的描述中，两个线程必须同时调用parent.addChild(child) 和 child.setParent(parent),并且是两个
相同的parent和child节点，死锁才可能发生。上面的代码可能执行很长的时间突然间就死锁了。    

要想死锁的发生，线程真的需要同时的去抢占对方的锁。例如，如果线程1比线程2早运行一小会，那么它就会直接的占有
A和B两个锁，线程2就会在试图锁B的时候被阻塞。这样就不会有死锁的发生。因为线程调度是无法预测的，所以没有办法
来预测一个死锁的发生。





























