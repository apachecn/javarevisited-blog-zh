# 如何有效地聚集石英工作

> 原文：<https://medium.com/javarevisited/how-to-cluster-effectively-quartz-jobs-9b097f5e1191?source=collection_archive---------1----------------------->

# 语境

一段时间前，我写了一篇关于如何在集群模式下将 [Quartz 与 Spring 一起使用的文章，但是由于 Quartz 的**主动-被动集群模式**，其中只有一个节点(容器、VM，这里放入您的基础设施类型)负责处理 Quartz 的传入“唤醒”信号，我们在项目中遇到了性能问题，因为只有一个繁重的任务消耗了所有节点资源，导致了崩溃。](/@rafaelfaita/spring-boot-using-quartz-in-mode-cluster-e1d71e4af4b9)

这样，我们选择使用**石英**和**队列**创建一个混合解决方案来解决这个问题。