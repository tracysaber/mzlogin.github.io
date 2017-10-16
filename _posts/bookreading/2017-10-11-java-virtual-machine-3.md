---
layout: post
title: 阅读《深入理解JAVA虚拟机》3
categories: BookReading
description: 阅读笔记
keywords: 读书
---
深入理解java虚拟机 读书笔记 3

# 第三章 垃圾收集器与内存分配策略
## 判断对象已死的方法

### 1. 引用计数算法（Reference Counting）
每当有一个地方引用它时计数器加1；当引用失效时，计数器值减1；任何时刻，计数器为0的对象是不能被使用的。 缺点在于不能处理**对象之间循环引用**的问题。

### 2. 可达性分析算法（Reachability Analysis）
通过一系列称为“GC Roots”的对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径称为引用链（Reference Chain），当一个对象到GC Roots没有任何引用链相连（即GC Roots到这个对象不可达），则证明这个对象是不可用的。GC Roots包括以下几类
* 虚拟机栈中引用的对象
* 方法区中类静态属性引用的对象
* 方法区中常量引用的对象
* 本地方法栈中JNI引用的对象
