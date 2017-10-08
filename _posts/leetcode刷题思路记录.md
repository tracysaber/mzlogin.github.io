---
layout: post
title: leetcode刷题思路
categories: [leetcode]
description: 记录解题方法
keywords: leetcode
---

## leetcode
### 18 四数之和
给定一个数组求 其中和为target的四元组（不能重复）
方法：先判断数组元素不足4个的，直接返回空的List
再用Arrays.sort进行排序。然后用两个变量for循环分别代表四元组d前两个数字，i遍历第1个数字至倒数第4个数字
j遍历i+1至倒数第3个数字，再用一个while循环处理后两个数组，k和l分别从j+1和最后1个数组相向逼近，如果有符合条件
的加入结果List中，并判断一下重复。
