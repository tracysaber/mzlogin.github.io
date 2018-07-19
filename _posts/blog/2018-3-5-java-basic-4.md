---
layout: post
title: java基础-容器等
categories: Java基础
description: 没有靠谱描述
keywords: java
---
java面试会考察的基础知识点4

# 语言知识
## Java Collections
包括了List,Queue,Set,Stack,Map等数据结构。

![集合关系图](/images/java/java-collection-hierarchy.jpeg)

1. Set表示数学意义上的集合概念，其最主要的特点是在集合中的元素不能重复。因此存入Set中的每个元素都必须定义equals()方法来确保对象的唯一性。有两个具体实现，HashSet和TreeSet(元素有序)。
2. List就是链表，保存了对象进入的顺序，所以能对列表中的每一个元素的插入和删除位置进行精确的控制。LinkedList,ArrayList,Vector都实现了这个接口。
3. Map提供了从键映射到值的数据结构。用于保存键值对，值可以重复，但键必须唯一。

## 迭代器
迭代器(Iterator)是一个对象，提供了一种访问一个容器对象中的各个元素。可以不需要了解容器底层的结构，就可以实现对容器的遍历。被称为轻量级的容器。
1. 用容器的iterator()方法返回一个Iterator，然后通过Iterator的next()方法返回第一个元素。
2. 