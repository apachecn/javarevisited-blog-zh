# 为什么我们需要数据库连接池？-每个程序员都必须知道

> 原文：<https://medium.com/javarevisited/why-do-we-need-a-database-connection-pool-every-programmer-must-know-9f90e7c8e5af?source=collection_archive---------0----------------------->

大家好。在本文中，我们将研究**数据库连接**及其**生命周期**。然后我们将看看 [**连接池**](https://javarevisited.blogspot.com/2012/06/jdbc-database-connection-pool-in-spring.html) ，它的内部，以及我们为什么需要使用它。然后我们将看看**设计模式**在哪里放置连接池。然后我们将查看数据库连接池中可能出现的**性能问题**，并通过查看 Java 中使用的常见**连接池框架来结束本文。让我们开始吧。**