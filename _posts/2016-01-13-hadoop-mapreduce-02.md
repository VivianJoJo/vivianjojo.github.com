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

<p><img src="/include/img/MapReduce作业执行流程图.jpg" alt="MapReduce工作执行流程图" height='500px' width='600px'/></p>

#####主要独立模块:

- 客户端(Client) : 编写MapReduce代码,配置作业,提交作业

- JobTracker : 初始化作业,分配作业,与TaskTracker通信,协调整个作业的执行

- TaskTracker : 保持与JobTracker的通信,在分配的数据片段上执行Map或Reduce任务,

- HDFS : 保存作业数据,配置信息,保存作业结果

<br/>

#####具体工作流程解析:

1. 提交作业

需求配置: 1. 程序代码; 2. Map和Reduce接口的配置; 3. 输入输出路径配置; 4. 其他类型配置

2. 初始化作业 :

3. 分配任务 :

4. 执行任务 :

5. 更新任务执行进度和状态 :

6. 完成作业 :

------------------------------------