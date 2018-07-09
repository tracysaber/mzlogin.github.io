---
layout: post
title: java基础-多线程等
categories: Java基础
description: 没有靠谱描述
keywords: java
---
java面试会考察的基础知识点2

# 语言知识
## 多线程问题
在Java语言中，线程有四种状态，*运行*，*就绪*，*挂起*，*结束*。

多线程的实现方法。
1. 继承Thread类，重写run()方法
2. 实现Runable接口，并实现run()方法
3. 实现Callable接口，重写call()方法
* Callable可以在任务结束后提供一个返回值，Runable没有这个功能。
* Callable的call()方法可以抛出异常
* 运行Callable可以拿到一个Future对象，表示异步计算的结果，它提供了检查计算是否完成的方法。由于线程属于异步计算模型，因此无法从别的线程中得到函数的返回值，这样就可以用Future来监视目标线程调用call()方法的情况，

多线程同步的实现方法有哪些
1. synchronized关键字
2. wait()方法和notify()方法
3. Lock
* lock()。以阻塞的方式获取锁。等待直到获取锁。
* tryLock(long timeout,TimeUnit unit)。如果获取到了锁返回True，否则等待相应的时间单元，期间如果得到锁就返回True，否则到时就返回False。
* lockInterruptibly()。主要区别在于如果获取不到锁，当前线程就会处于休眠的状态。

synchronized与Lock有什么异同
1. 用法不一样。synchronized既可以加载方法上，也可以加在特定代码块中，括号中表示需要加锁的对象。而Lock需要显示地指定起始位置和终止位置。synchrozied是托管给JVM去操作的，而Lock的锁定是通过自己编写代码实现的，有着更精确的线程语义。
2. 性能不一样。在JDK5中增加了Lock接口的实现类ReentrantLock。和synchronized有着相同的并发性和内存语义，在资源竞争比较激烈的情况下ReentrantLock性能比较好，反之synchronized比较占优势。
3. 锁机制不一样。synchronized获取锁和释放都是在块结构中，当获取多个锁时，必须以相反的顺序释放，而且是自动解锁。Lock则需要开发人员手动去释放锁，并且必须在finally中释放。
