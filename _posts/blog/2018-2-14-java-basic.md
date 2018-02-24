---
layout: post
title: java面试基础内容
categories: Blog
description: 没有靠谱描述
keywords: java
---
java面试会考察的基础知识点

# 语言知识
* Java是否存在内存泄漏的问题

```shell
回答：存在，如下几种情况都有可能发生内存泄漏
1.静态集合类。比如HashMap这类数据结构，如果这些容器是静态的，由于他们的生命周期和程序一致，所以容器内的对象在容器的生命周期结束之前不会被释放。
2.各种连接。比如数据库、网络等等。如果访问数据库的过程中对Connection、Statement等不显式关闭，将会造成大量的对象无法被回收，从而引起内存泄漏。
3.


```

