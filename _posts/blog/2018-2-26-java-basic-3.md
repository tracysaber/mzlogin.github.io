---
layout: post
title: java面试基础内容3
categories: Blog
description: 没有靠谱描述
keywords: java
---
java面试会考察的基础知识点3

# 语言知识
* 面向对象的知识
面向对象的主要特征包括抽象、继承、封装和*多态*。
1. 继承。
* class 子类 extends 父类。
* Java不支持多重继承，一个子类最多只能有一个父类，但是可以实现多个接口达到多重继承的目的。
* 子类只能继承父类的非私有成员变量和方法。
* 成员变量重名时子类会覆盖父类的变量。
* 相同的函数子类会重写父类的方法。

synchronized与Lock有什么异同
1. 用法不一样。synchronized既可以加载方法上，也可以加在特定代码块中，括号中表示需要加锁的对象。而Lock需要显示地指定起始位置和终止位置。synchrozied是托管给JVM去操作的，而Lock的锁定是通过自己编写代码实现的，有着更精确的线程语义。
2. 性能不一样。在JDK5中增加了Lock接口的实现类ReentrantLock。和synchronized有着相同的并发性和内存语义，在资源竞争比较激烈的情况下ReentrantLock性能比较好，反之synchronized比较占优势。
3. 锁机制不一样。synchronized获取锁和释放都是在块结构中，当获取多个锁时，必须以相反的顺序释放，而且是自动解锁。Lock则需要开发人员手动去释放锁，并且必须在finally中释放。
