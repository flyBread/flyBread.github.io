---
layout: post
title: "Concurrence-11-Java Synchronized Blocks"
description: "翻译 梳理基础的东西"
category: 翻译 java 并发编程 多线程
tags: [翻译 并发编程 多线程]
---
#### Java Synchronized Blocks
<br/>
##### java同步代码块
<br/>

文章的地址：[翻译文章的源地址](http://tutorials.jenkov.com/java-concurrency/synchronized.html)
<br/>

把一个方法或者一段代码标记为同步，就叫做java同步的代码块，java 同步代码块可以用来避免竞争条件
的发生。   

####java同步关键字
在java中是使用关键字 synchronized 来标记同步代码块。在java中一个同步块是在某一个对象上的同步。
所有同步在同一个对象上的代码块，在某一个时刻只能有一个线程能够在其中执行代码，所有的其他的企图
进入代码块的线程都会被阻塞，知道同步代码块里面的线程离开同步快。   

关键字 synchronized 可以用来标记四种类型的代码块：    
1.实例方法    
2.静态方法   
3.实例方法中的代码块   
4.静态方法中的代码块    

这些块是不同的对象的同步。你需要哪种类型的同步块取决于具体情况。   

####实例方法的同步
实例方法的同步代码如下：    

```
 public synchronized void add(int value){
      this.count += value;
  }
```  


备注：关键字在方法声明中，这样告诉java这个方法是同步的。     
java中一个同步的实例方法，是在拥有这个方法实例上的同步。因此每一个实例都有同步在不同对象上的同步
方法，这里的不同对象指的拥有各自方法的实例。同步方法中只能有一个线程执行，如果多个线程存在，一个
线程在某一个时刻只能一个实例中的一个同步方法中执行代码，一个线程对应一个实例。    

#### 静态方法的同步

同步静态方法的方法就就像是同步实例方法中那样使用 synchronized关键字。下面就是例子：   

```
  public static synchronized void add(int value){
      count += value;
  }
```

这里synchronized关键字，就是告诉java这个是一个同步的方法。    

静态同步方法是同步在静态方法所属于的类对象上面，因为在JVM中每一个类只有一个Class对象，那么在相同
的类中，只有一个线程运行在同步的静态方法中。    

如果静态方法位于不同的类中，那么每一个类中就可以有一个线程在同步静态方法中运行。每一个线程对应
一个类，不管它调用的是哪一个静态同步方法。    

#### 实例方法中的静态代码块
我们可能没有必要同步整个方法，有时候我们只需要同步方法的一部分，方法中的同步代码块就是做这个的。
下面就是一个示例：   

```
  public void add(int value){
    synchronized(this){
       this.count += value;   
    }
  }
```

上面的示例就是使用同步代码来使一段代码同步，这段的代码在执行的时候就像在同步方法中一样。  
备注：java同步代码块在括号里面使用了一个对象，在这个示例中，我们使用的是this，代表调用这个
add方法的实例，这个synchronized关键字括号里面使用的对象被称之为监视对象，也就是说这个同步
代码块同步在这个监视对象上，实例方法的同步就是使用该实例方法归属的对象作为监视对象的。    

在同一个监视对象上，同步代码快内只能有一个线程执行。  

接下来的例子里面，两个方法同时同步在它们被调用的实例上，因此他们同步在相同的对象上。

```
public class MyClass {
    public synchronized void log1(String msg1, String msg2){
       log.writeln(msg1);
       log.writeln(msg2);
    }
    public void log2(String msg1, String msg2){
       synchronized(this){
          log.writeln(msg1);
          log.writeln(msg2);
       }
    }
  }
```  

因此在这个例子中，只能有一个线程在两个代码块中的其中一个里面跑。   

如果第二个代码块同步在其他的监视对象而不是this上，那么在某一个时刻每一个方法里面都可以有一个线程
在跑。   

####静态方法里面的同步代码块
下面是两个静态方法的例子，这些方法都是同步在静态方法归属的类的类对象上的：    

```
 public class MyClass {
    public static synchronized void log1(String msg1, String msg2){
       log.writeln(msg1);
       log.writeln(msg2);
    }
    public static void log2(String msg1, String msg2){
       synchronized(MyClass.class){
          log.writeln(msg1);
          log.writeln(msg2);  
       }
    }
  }
```    

在任意时刻只能有一个线程跑在两个同步方法中的一个内。    

如果第二个同步代码块没有同步在MyClass.class 类对象上，那么在某一个时刻，每一个方法内就有可能
有一个线程在跑。   

#### java同步的示例









 
















