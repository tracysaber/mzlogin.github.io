---
layout: post
title: oozie使用过程踩坑记录
categories: Blog
description: 没有靠谱描述
keywords: 自嗨
---
oozie 使用情况记录

# Oozie使用问题记录
Oozie运行时使用的job.properties和workflow.xml的地址需要指明清楚，如果是放在hdfs上需要提前上传并在相应的配置项中指明对应的路径。
Oozie安装完成后测试有可能出现如下错误
![图片1](/images/posts/oozie/1.png)

原因是因为oozie各节点的时间没有同步，需要对于所有的节点进行一次同步
通过*oozie job –oozie http://namenode:11000/oozie -config /path/to/job.properties –run*可以在命令行里启动相应的作业。一些正常的作业可以比较容易地进行，比如执行shell脚本或者跑一个mapreduce的程序。
提交spark作业有几点需要注意的地方，需要指定master的运行模式，本地运行local模式的配置比较少，要运行在yarn上的话，需要在workflow.xml中增加SPARK_HOME的信息。
```xml
<global>
   <configuration>
       <property>
          <name>oozie.launcher.yarn.app.mapreduce.am.env</name>
          <value>SPARK_HOME=/home/hdfs/spark-2.1.1-bin-hadoop2.7</value>
       </property>
    </configuration>
</global>
```
在spark的具体属性中，<jar>元素要给出在hdfs上的具体路径。
一些可以灵活配置经常改动的配置项可以都放在job.properties中，比如master，可以指定为local也可以在yarn-cluster上运行。
以下两个配置项是在spark2.0版本及以上一定要注意配置的，oozie.libpath指向hdfs上上传的库文件位置。版本低于spark2.0的需要在这个文件夹下上传spark-assembly.jar文件，但如果高于2.0则需要把spark/jars目录下的所有jar文件都上传到相应位置。
> oozie.use.system.libpath=true
oozie.libpath=${nameNode}/user/${user.name}/share/lib/spark

最后需要注意的是sparkopts这个配置项，按照Oozie官网对于spark action的要求，一下三项都要配置。
>--conf spark.yarn.history    Server.address=http://namenode:18088 --conf spark.eventLog.dir=hdfs://namenode:9000/user    /hdfs/applicationHistory --conf spark.eventLog.enabled=true

