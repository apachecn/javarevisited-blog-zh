# Java 中的同步(this)与同步(class)

> 原文：<https://medium.com/javarevisited/synchronized-this-vs-synchronized-class-in-java-d521556b1f?source=collection_archive---------0----------------------->

![](img/5fdc6141a945220da502bdbf597b279f.png)

Johnny Wang 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 概念

[Synchronized](https://www.java67.com/2013/01/difference-between-synchronized-block-vs-method-java-example.html) 是 Java 中的一个关键字，使用锁机制来实现同步。

锁定机构具有以下两个特征。

**互斥**:即同一时间只允许一个线程持有一个对象锁，这个特性用来实现…