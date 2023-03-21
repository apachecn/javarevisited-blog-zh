# 微服务中的分布式事务管理—第 2 部分— Saga 模式

> 原文：<https://medium.com/javarevisited/distributed-transaction-management-in-microservices-part-2-saga-pattern-53808a55e641?source=collection_archive---------0----------------------->

大家好。这篇文章是上一篇文章的延续

[](https://dineshchandgr.medium.com/distributed-transaction-management-in-microservices-part-1-bb7dc1fbee9f) [## 微服务中的分布式事务管理—第 1 部分

### 大家好。在本文中，我们将了解跨微服务的分布式事务管理。

dineshchandgr.medium.com](https://dineshchandgr.medium.com/distributed-transaction-management-in-microservices-part-1-bb7dc1fbee9f) 

在本文中，我们将了解到 [**Saga 模式**](https://javarevisited.blogspot.com/2021/09/microservices-design-patterns-principles.html) **，这是一种异步模式**，它在每个微服务中执行一系列事务并发布…