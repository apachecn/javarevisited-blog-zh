# HDFS 读写

> 原文：<https://medium.com/javarevisited/hdfs-write-read-cfa17a1a6100?source=collection_archive---------0----------------------->

在 hadoop 分布式世界中，需要了解的最重要和最基本的事情之一是 HDFS 写/读操作是如何执行的。要了解 hadoop 生态系统中不同组件的工作方式，了解基本的 hdfs 概念非常重要。在进入 hdfs 写/读之前，让我们列出其中涉及的主要参与者

客户端:与群集交互，启动写入和读取

NameNode:负责编排和委托的主守护进程

DataNodes:从属守护进程，处理实际的数据存储