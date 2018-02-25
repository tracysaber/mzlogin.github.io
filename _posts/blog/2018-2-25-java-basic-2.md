---
layout: post
title: java面试基础内容
categories: Blog
description: 没有靠谱描述
keywords: java
---
java面试会考察的基础知识点

# 语言知识
* 多线程问题
在Java语言中，线程有四种状态，*运行*，*就绪*，*挂起*，*结束*

多线程同步的实现方法有哪些
1. synchronized关键字
2. wait()方法和notify()方法
3. Lock
* lock()。以阻塞的方式获取锁。等待直到获取锁。
* tryLock(long timeout,TimeUnit unit)。如果获取到了锁返回True，否则等待相应的时间单元，期间如果得到锁就返回True，否则到时就返回False。


