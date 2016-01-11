---
layout: post
title: "Hadoop — 安装及配置"
description: ""
category: Hadoop
tags: [Hadoop]
---
{% include JB/setup %}


安装前准备
------------

#### 安装Hadoop之前需确保JDK,以及SSH安装ok.

- jdk需要1.6版本及以上.

- hadoop需要通过ssh来启动Slave列表中各台主机的守护进程.Hadoop并没有区分集群模式和伪分布式模式,所以,hadoop会采用与集群模式相同的处理方式,
即按次序启动文件conf/slave中记载的主机上的进程.只不过在伪分布式中Slave为localhost.所以,即使在伪分布式下,ssh也是必须的.



运行配置
---------

下载hadoop压缩包,解压至指定目录.

#### Hadoop角色定义

hadoop分别从三个角度将主机划分成两种角色:

- 基本: Master和Slave

- HDFS角度: NameNode和DataNode

- MapReduce角度: JobTracker和TaskTracker

#### Hadoop三种运行方式

- 单机模式

- 伪分布式模式

- 集群模式

单机模式无需配置,在这种方式下,Hadoop被认为是一个单独的Java进程,此方式经常用来调试.

伪分布式模式的hadoop相当于只有一个节点的集群,在这个集群中,这个节点既是Master,也是Slave;既是NameNode,也是DataNode;既是JobTracker,也是TaskTracker.

着重说明伪分布式模式的配置:

**_conf/Hadoop-env.sh_**

*指定JDK安装位置*

    export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_20.jdk/Contents/Home

**_conf/core-site.xml_**

*核心配置文件,配置hdfs的地址及端口号*

    <configuration>
        <property>
            <name>fs.default.name</name>
            <value>hdfs://localhost:9000/</value>
        </property>
    </configuration>

**_conf/hdfs-site.xml_**

*配置备份方式,默认为3.单机中改为1*

    <configuration>
    	<property>
    		<name>dfs.replication</name>
    		<value>1</value>
    	</property>
    </configuration>

**_conf/mapred-site.xml_**

*配置JobTracker的地址及端口*

    <configuration>
    	<property>
    		<name>mapred.job.tracker</name>
    		<value>localhost:9001</value>
    	</property>
    </configuration>

Hadoop启动
---------

- 启动Hadoop之前需格式化Hadoop的文件系统HDFS.

`bin/hadoop namenode -format`

- 启动Hadoop

`bin/start-all.sh`

- 验证Hadoop是否安装成功

[http://localhost:50030](http://localhost:50030)  MapReduce的web页面

[http://localhost:50070](http://localhost:50070) HDFS的web页面

页面显示正常即安装成功.

--------------------

*vivian.yu*

--------------------