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

![map关系图](/images/java/MapClassHierarchy.jpg)

1. Set 表示数学意义上的集合概念，其最主要的特点是在集合中的元素不能重复。因此存入Set中的每个元素都必须定义equals()方法来确保对象的唯一性。
* \[interface\]  **SortedSet**  保证其中的key都是有序的，（要求插入的数据类型都实现了 **Comparable**  接口） -->子接口  **NavigableSet**  提供了一些根据某个key寻找它前后key的方法。
* 实现类， **TreeSet** 是 **NavigableSet** 的直接实现子类，能够保证key的有序排列。

2. List就是链表，保存了对象进入的顺序，所以能对列表中的每一个元素的插入和删除位置进行精确的控制。
* **LinkedList** 的实现是 双向链表。也是实现了List接口中的方法，通过比较索引值的位置来实现get等方法。
当我们调用get(int location)时，首先会比较“location”和“双向链表长度的1/2”；若前者大，则从链表头开始往后查找，直到location位置；否则，从链表末尾开始先前查找，直到location位置。
* **Vector** ：线程安全，但速度慢。
　　　　  默认初始容量为10，加载因子为1，扩容增量：原容量的 1倍（可以设置）
* **ArrayList** ：线程不安全，查询速度快。
　　      默认初始容量为10，加载因子为1，扩容增量：原容量的 0.5倍+1
* **Vector** 和 **ArrayList** 都是用数组实现的，在内存中的地址空间连续。

3. Queue 队列，

4. Map提供了从键映射到值的数据结构。用于保存键值对，值可以重复，但键必须唯一。

* **HashMap**  初始容量是16，加载因子是0.75，每次扩容的大小是之前的两倍。

* **HashTable** 初始容量是11，加载因子是0.75，扩容增量：原数组长度+1。

* **WeakHashMap** 中的key是弱引用，只要key不再被外部引用，就会被回收。

## 迭代器
迭代器(Iterator)是一个对象，提供了一种访问一个容器对象中的各个元素。可以不需要了解容器底层的结构，就可以实现对容器的遍历。被称为轻量级的容器。
1. 用容器的iterator()方法返回一个Iterator，然后通过Iterator的next()方法返回第一个元素。
2. 使用Iterator的hasNext()方法判断容器内是否还有元素，如果有通过next()获取下一个元素。
3. 可以通过remove()删除迭代器返回的元素。

### **Iterator** 与 **ListIterator** 的区别， **Iterator** 只能正向遍历集合， **ListIterator** 继承自 **Iterator** ，专门针对List，可以从两个方向来遍历List。