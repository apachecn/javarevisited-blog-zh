# Synchronized VS ReentrantLock，如何选择？

> 原文：<https://medium.com/javarevisited/synchronized-vs-reentrantlock-how-to-choose-cfb5306080e7?source=collection_archive---------1----------------------->

在多线程场景中，我们经常使用 Synchronized 和 Reentrantlock。你知道它们之间的区别吗？

![](img/85e5261c711e371cdcdc1ad9d4d866cc.png)

当我们遇到多线程场景时，我们自然会考虑线程安全。这个时候我们会考虑使用锁来保证代码同步。Java 中有两种锁定方法。一种是使用同步关键字 [synchronized](https://javarevisited.blogspot.com/2020/04/difference-between-atomic-volatile-and-synchronized-in-java-multi-threading.html) ，也叫…