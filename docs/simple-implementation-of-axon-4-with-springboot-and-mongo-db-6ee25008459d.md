# 用 SpringBoot 和 Mongo DB 简单实现 Axon 4

> 原文：<https://medium.com/javarevisited/simple-implementation-of-axon-4-with-springboot-and-mongo-db-6ee25008459d?source=collection_archive---------1----------------------->

在本教程中，我们将学习实现一个简单的 SpringBoot 应用程序，它实现了 CQRS 原理。

在这个用例中，我们将使用 Axon 4 框架。为了存储事件，我们将使用 [Mongo DB](/javarevisited/5-best-mongodb-courses-to-learn-nosql-for-beginners-in-2020-42df5af5496c?source=---------13----------------------------) ，而不是存储库来存储和查询数据，我们将使用静态映射。

## 概念

CQRS —命令查询分离。简而言之，要么对数据执行操作，要么查询并返回数据。