---
layout: post
title: java基础-多线程，并发等
categories: Java基础
description: 没有靠谱描述
keywords: java
---
java面试会考察的基础知识点2

# 语言知识
## 多线程问题
在Java语言中，线程有四种状态，*运行*，*就绪*，*挂起*，*结束*。

### 多线程的实现方法。
1. 继承Thread类，重写run()方法
2. 实现Runable接口，并实现run()方法
3. 实现Callable接口，重写call()方法
* Callable可以在任务结束后提供一个返回值，Runable没有这个功能。
* Callable的call()方法可以抛出异常
* 运行Callable可以拿到一个Future对象，表示异步计算的结果，它提供了检查计算是否完成的方法。由于线程属于异步计算模型，因此无法从别的线程中得到函数的返回值，这样就可以用Future来监视目标线程调用call()方法的情况，

### 多线程同步的实现方法有哪些
1. synchronized关键字
2. wait()方法和notify()方法
3. Lock
* lock()。以阻塞的方式获取锁。等待直到获取锁。
* tryLock(long timeout,TimeUnit unit)。如果获取到了锁返回True，否则等待相应的时间单元，期间如果得到锁就返回True，否则到时就返回False。
* lockInterruptibly()。主要区别在于如果获取不到锁，当前线程就会处于休眠的状态。

### synchronized与Lock有什么异同
1. 用法不一样。synchronized既可以加载方法上，也可以加在特定代码块中，括号中表示需要加锁的对象。而Lock需要显示地指定起始位置和终止位置。synchrozied是托管给JVM去操作的，而Lock的锁定是通过自己编写代码实现的，有着更精确的线程语义。
2. 性能不一样。在JDK5中增加了Lock接口的实现类ReentrantLock。和synchronized有着相同的并发性和内存语义，在资源竞争比较激烈的情况下ReentrantLock性能比较好，反之synchronized比较占优势。
3. 锁机制不一样。synchronized获取锁和释放都是在块结构中，当获取多个锁时，必须以相反的顺序释放，而且是自动解锁。Lock则需要开发人员手动去释放锁，并且必须在finally中释放。

### 乐观锁和悲观锁
**悲观锁** ：假定会发生并发冲突，屏蔽一切可能违反数据完整性的操作。
**乐观锁** ：假设不会发生并发冲突，只在提交操作时检查是否违反数据完整性。乐观锁不能解决脏读的问题。

独占锁是一种悲观锁，Java中synchronized就是一种独占锁。有着比较大的开销。
CAS(Compare and Swap)是一种乐观锁，每次不加锁，而是假设没有冲突去完成某个操作。如果失败就重试，直到成功。
CAS中存在着三个值，一个是内存位置的值V，一个是旧的预期值A，一个是新值B。指令执行时，当且仅当V符合旧预期值A时，处理器用新值B更新V的值，否则它就不执行更新，但是无论是否更新了V的值，都会返回V的旧值，上述的处理过程是一个原子操作。