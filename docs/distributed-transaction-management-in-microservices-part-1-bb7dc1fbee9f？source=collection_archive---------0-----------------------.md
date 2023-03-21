# 微服务中的分布式事务管理—第 1 部分

> 原文：<https://medium.com/javarevisited/distributed-transaction-management-in-microservices-part-1-bb7dc1fbee9f?source=collection_archive---------0----------------------->

[![](img/3aab6c4621f11f9ae2a9e3016a4843c7.png)](https://javarevisited.blogspot.com/2021/09/microservices-design-patterns-principles.html)

图片来源:[https://miro . medium . com/max/690/1 * zba 4 hre 9 xkf 4 fzips 2 mnfq . png](https://miro.medium.com/max/690/1*ZbA4HrE9XKF4FziPs2MNfQ.png)

大家好。在本文中，我们将了解跨[微服务](/javarevisited/10-best-java-microservices-courses-with-spring-boot-and-spring-cloud-6d04556bdfed)的分布式事务管理。

## 什么是交易？

事务不过是一系列必须成功执行的操作。即使其中一个操作失败，也必须回滚整个步骤，以便离开…