---
layout: post
title: "Hadoop — MapReduce工作机制"
description: ""
category: Hadoop
tags: [Hadoop, MapReduce]
---
{% include JB/setup %}


MapReduce任务执行流程
---------

一个Map/Reduce 作业（job） 通常会把输入的数据（input file）切分为若干独立的数据块（splits），然后由 map任务（task）以完全并行的方式处理它们.<br/>
Map/Reduce框架会对map的输出做一个 Shuffle 操作，Shuffle 操作的后的结果会输入给reduce任务.<br/>
整个Map/Reduce框架负责任务的调度和监控，以及重新执行已经失败的任务.

Map/Reduce计算集群由一个单独的JobTracker（master） 和每个集群节点一个 TaskTracker（slave）共同组成.<br/>
JobTracker负责调度构成一个作业的所有任务，这些任务会被分派到不同的TaskTracker上去执行，JobTracker会监控它们的执行、重新执行已经失败的任务.<br/>
而TaskTracker仅负责执行由JobTracker指派的任务。

<p><img src="/include/img/MapReduce作业执行流程图.jpg" alt="MapReduce工作执行流程图" height='500px' width='600px'/></p>

#####主要独立模块:

- 客户端(Client) : 编写MapReduce代码,配置作业,提交作业

- JobTracker : 初始化作业,分配作业,与TaskTracker通信,协调整个作业的执行

- TaskTracker : 保持与JobTracker的通信,在分配的数据片段上执行Map或Reduce任务,

- HDFS : 保存作业数据,配置信息,保存作业结果

<br/>

#####具体工作流程解析:

1. 提交作业

    用户在提交作业之前,需要做些必要的配置,比如:程序代码,Map和Reduce的接口配置,输入输出路径的配置,
    其他类型的设置如InputFormat(),OutputFormat(),作业名称,主函数中的Output的key和value类型.

    配置完作业的所有内容后即可运行作业了,**_步骤1_**.

    首先JobTracker调用getNewJobId()从JobTracker初获取当前作业的ID,**_步骤2_**;

    同时检查作业的相关路径;计算作业的输入划分,并将划分写入Job.split文件中;将运行所需的资源如:作业Jar文件\配置文件和计算所得的输入划分信息
    复制到作业对应的HDFS上,**_步骤3_**;

    调用JobTracker对象的submitJob()方法来真正提交作业,告诉JobTracker作业准备执行,**_步骤4_**.

2. 初始化作业 :

    JobTracker会把作业放入一个内部队列中，交由Job Scheduler进行调度，并对其进行初始化,**_见步骤5_**.

    为了创建任务运行列表，作业调度器首先从共享文件系统中获取Job已计算好的InputSplit的信息,**_见步骤6_**.
    然后为每个InputSplit创建一个Map任务,Reduce任务的数量由JobConf的mapred.reduce.tasks属性决定

3. 分配任务 :

    JobTracke和TaskTracker之间的通信和任务分配是通过心跳机制完成.TaskTracker作为一个单独的JVM执行一个简单的循环,主要实现每隔一段时间向JobTracker发送心跳:
    告诉JobTracker是否存活,是否准备执行新任务.JobTracker接收到心跳信息,如果有待分配任务,就会为TaskTracker分配一个任务,并将分配信息封装在心跳通信的返回值中返回给
    TaskTracker,**_步骤7_**. JobTacker为TaskTracker分配Map任务时,为减少网络带宽,会考虑将Map数据本地化.取离TaskTracker map任务最近的划分文件分配给TaskTracker
    ,最好是在TaskTracker本地,(TaskTracker往往运行在HDFS的DataNode中)

4. 执行任务 :

    TaskTracker申请到新任务后就要在本地运行任务了.首先将任务本地化(将任务所需的数据,配置信息,程序代码从HDFS复制到TaskTracker本地)**_步骤8_**.

    调用launchTaskForJob()方法真正启动任务,先为任务创建本地目录,然后启动TaskRunner,**_步骤9_**.

    TaskRunner再启动新的child java虚拟机来运行每个任务,**_步骤10_**.最后将运算结果收集到缓存文件系统中.


------------------------------------