---
layout: post
title: 阅读《深入理解JAVA虚拟机》6
categories: BookReading
description: 阅读笔记
keywords: 读书
---
深入理解java虚拟机 读书笔记 6

# 第六章 类文件结构
## 概述
java的程序文件最终是被转化成了与操作系统和机器指令集无关的、平台中立的格式作为程序编译后的存储格式。

## 无关性的基石
各种不同的平台的虚拟机与所有平台都统一使用的程序存储格式-字节码(ByteCode)是构成无关性的基石，虚拟机同时有着另一个中立特性-语言无关性。除了java语言以外，Clojure,Groovy,Scala,Jython等语言都可以运行在java虚拟机上。
Java虚拟机不和包括Java在内的任何语言绑定，它只与“Class文件”这种特定的二进制文件格式所关联，Class文件