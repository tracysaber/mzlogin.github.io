---
layout: post
title: 阅读《深入理解JAVA虚拟机》7
categories: BookReading
description: 阅读笔记
keywords: 读书
---
深入理解java虚拟机 读书笔记 7

# 第七章 虚拟机类加载机制
## 概述
虚拟机把描述类的数据从Class文件加载到内存，并对数据进行校验、转换解析和初始化，最终形成可以被虚拟机直接使用的Java类型。在Java语言里，类型的加载、连接和初始化过程都是在程序运行期间完成的，Java里天生可以动态拓展的语言特性就是依赖运行期动态加载和动态连接这个特点实现的。例如，如果编写一个面向接口的应用程序，可以等到运行时
再指定其实际的实现类。从最基础的Applet、jsp到相对复杂的OSGi技术，都是使用了Java语言运行期间类加载的特性。
## 类加载的时机
类的整个生命周期包括：加载(Loading)、验证(Verification)、准备(Preparation)、解析(Resolution)、初始化(Initialization)、使用(Using)和卸载(Unloading)7个阶段。验证、准备、解析3个部分统称为连接(Linking)。
![图片1](/images/bookreading/jvm7/1.png)