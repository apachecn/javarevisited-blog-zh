# 在 Spring Boot 应用中使用 Redis、MYSQL 提高 API 性能

> 原文：<https://medium.com/javarevisited/improve-api-performance-using-redis-mysql-in-spring-boot-app-c8bb089691af?source=collection_archive---------0----------------------->

## 做出更快的响应并减少对昂贵的数据库查询的依赖。

![](img/761772f831bd7ef7bcdcd33fb4bf8f66.png)

图片来源——维基百科

更常见的是，在 API 设计期间，每个 API 可能会在从数据库或外部 API 调用中查询数据后作出响应。有时有些表中的数据不经常改变，这意味着这些表不是…