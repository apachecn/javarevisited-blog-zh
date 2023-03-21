# Spring Boot 和 MongoDB:搜索和分页

> 原文：<https://medium.com/javarevisited/spring-boot-mongodb-searching-and-pagination-1a6c1802024a?source=collection_archive---------0----------------------->

## 使用 MongoTemplate 在 Spring Boot 应用程序中实现搜索和分页

![](img/d36076f1817d281bdd287cf071975729.png)

类为我们提供了与数据库交互的特性，并以线程安全的方式提供了创建、更新、删除和查询 MongoDB 文档的操作。`MongoTemplate`类实现了接口`MongoOperations`。你可以找到方法…