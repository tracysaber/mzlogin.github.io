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

下面的四个加载器是Tomcat自己定义的类加载器。其中WebApp类加载器和Jsp类加载器通常会存在多个实例，每一个Web应用程序对应一个WebApp类加载器。每一个Jsp文件对应一个Jsp类加载器。
对于Tomcat的6.x版本，只有指定了tomcat/conf/catalina.properties配置文件的server.loader和share.loader项后才会建立这两个类加载器的实例，否则都会用CommonClassLoader的实例代替，而默认的配置文件中没有设置这两项，所以顺理成章地把这三个目录合并到/lib目录中去。
## OSGi:灵活的类加载器架构
OSGi(Open Service Gataway Initiative)是OSGi联盟制定的一个基于Java语言的动态模块化规范。最著名的应用案例就是Eclipse IDE。
每个模块（称为Bundle）与普通的Java类库区别不大，两者一般都以JAR格式进行封装，并且内部存储的都是Java Package和Class。但是一个Bundle可以声明它所依赖的Java Package，也可以声明它允许导出发布的Java Package。
