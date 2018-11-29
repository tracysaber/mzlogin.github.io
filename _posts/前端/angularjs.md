---
layout: post
title: angular echarts ngx-echarts采坑实录
categories: 前端
description: 没有
keywords: js angular echarts
---

# angular echarts ngx-echarts使用记录
项目中要使用地图，自然而然想到echarts的map，不过使用过程中遇到大坑，网上没有找到合适的解答，后来找到一个可行的解决方案。
echarts是百度的一个图表展示的开源工具，方便好用并且十分强大。

## 纯js使用方法

## angularjs项目中使用echarts
在angular/cli中使用echarts时，纯js的方法不太适用，因此我们可以选择针对angular的echarts工具。常见的有echarts-ng2和ngx-echarts，后者使用更为方便、强大。
### 安装
```shell
# if you use npm
npm install echarts --save
npm install ngx-echarts --save
```
把这两种工具安装到项目中去。