---
layout: post
title: 《JAVA并发编程的艺术》
categories: BookReading
description: 阅读笔记
keywords: 读书
---
JAVA并发编程的艺术 读书笔记

# 第一章 并发编程的挑战
## 上下文切换
CPU通过时间片分配算法来循环执行任务，当前任务执行一个时间片后会切换到下一个任务。但是，在切换前会保存上一个任务的状态，以便下次切换回这个任务时，可以再加载这个任务的状态。所以任务从保存到再加载的过程就是一次上下文切换。
