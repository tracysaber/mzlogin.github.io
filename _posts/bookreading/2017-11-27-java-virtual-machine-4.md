---
layout: post
title: 阅读《深入理解JAVA虚拟机》4
categories: BookReading
description: 阅读笔记
keywords: 读书
---
深入理解java虚拟机 读书笔记 4

# 第四章 虚拟机性能监控与故障处理工具
## 概述
给一个系统定位问题的时候，知识、经验是关键基础，数据是依据，工具是运用知识处理数据的手段。这里的数据包括：运行日志、异常堆栈、GC 日志等。

## JDK的命令行工具
在bin目录下有很多命令行工具，大多数都是jdk/lib/tools.jar类库的一层薄包装。
|   名称   |   主要作用   |
|   :---:  |   :-----:    |
|   jps    |  jvm process status tool,显示指定系统内所有的hotspot虚拟机进程|
|   jstat  |  jvm statistics monitoring tool,用于收集hotspot虚拟机各方面的运行数据|
|   jinfo　|  configuration info for java，显示虚拟机配置信息
|   jmap　 |  memory map for java,生成虚拟机的内存转储快照（heapdump文件）|
|   jhat　 |  jvm heap dump browser，用于分析heapmap文件，它会建立一个http/html服务器让用户可以在浏览器上查看分析结果|
|   jstack |　stack trace for java ,显示虚拟机的线程快照|