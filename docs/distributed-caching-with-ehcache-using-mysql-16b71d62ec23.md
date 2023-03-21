# 使用 MySQL 的 Ehcache 分布式缓存

> 原文：<https://medium.com/javarevisited/distributed-caching-with-ehcache-using-mysql-16b71d62ec23?source=collection_archive---------2----------------------->

![](img/3acd3fa685ed770dfc1a4ab7fa455cb8.png)

## 不要！新技术或额外的计算资源

# 问题陈述

我认为 Ehcache 是任何人为了实现服务器端缓存都可以采用的最流行和最简单的框架。如果我们有一个单独的吊舱系统，它会工作得非常好。一旦我们进入一个需要支持高可用性和自动扩展的系统…