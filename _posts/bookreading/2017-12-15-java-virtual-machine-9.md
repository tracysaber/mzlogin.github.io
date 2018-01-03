---
layout: post
title: 阅读《深入理解JAVA虚拟机》9
categories: BookReading
description: 阅读笔记
keywords: 读书
---
深入理解java虚拟机 读书笔记 9

# 第九章 类加载及执行子系统的案例与实战
## Tomcat 正统的类加载架构
主流的Java Web服务器，都实现了自己定义的类加载器
一个功能健全的Web服务器需要解决如下几个问题。
* 部署在同一个服务器上的两个Web应用程序所使用的Java类库可以实现相互隔离。
* 同样在一个服务器上的应用可以共享Java类库。
* 服务器自身的安全不能受到部署的应用的影响。
* 要支持HotSwap功能。支持JSP生成类的热替换。
由此一个单独的ClassPath无法满足需求，因此一般的服务器提供好几个ClassPath路径供用户存放第三方类库，这些路径都以"lib"或"classes"命名。通常每一个目录都会有一个相应的自定义类加载器去加载放置在里面的Java类库。Tomcat中有三组目录可以存放Java类库（/common,/server,/shared）还可以加上Web应用自身的目录/WEB-INF，一共四组。
* /common 可以被Tomcat和所有的Web应用共享
* /server 只能被Tomcat自身使用
* /shared 可以被所有的Web应用程序共同使用，但是Tomcat不可以
* /WEB-INF 仅仅能被应用程序自身使用，对Tomcat和别的应用程序都不可见

![图片1](/images/bookreading/jvm9/1.png)
