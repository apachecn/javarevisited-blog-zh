# Java 原子数据类型:面试参考

> 原文：<https://medium.com/javarevisited/java-atomic-datatype-interview-reference-a463632d0d1a?source=collection_archive---------5----------------------->

作为 java 并发的一部分，它引入了一些新的数据类型用于并发处理。这些被称为原子数据类型。在本文中，我们将探索这些数据类型。

![](img/5f0ca82e9ffbed830c80d741a4c291dd.png)

[Zoltan·塔斯](https://unsplash.com/@zoltantasi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 什么是原子数据类型？

在 Java 中，java.util.concurrent.atomic 包下包含的类有 **AtomicBoolean、AtomicInteger、AtomicLong、AtomicReference** …