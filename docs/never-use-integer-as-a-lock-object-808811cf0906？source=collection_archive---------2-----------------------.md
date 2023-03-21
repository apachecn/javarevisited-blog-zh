# 永远不要使用整数作为锁对象

> 原文：<https://medium.com/javarevisited/never-use-integer-as-a-lock-object-808811cf0906?source=collection_archive---------2----------------------->

也许你已经很熟悉并发编程了，但是在使用 synchronized 的时候还是要小心。

![](img/595c84f0e3e433fa2a2f697ac0ec0eda.png)

在一些并发场景中，我们经常使用 [synchronized](https://javarevisited.blogspot.com/2020/04/difference-between-atomic-volatile-and-synchronized-in-java-multi-threading.html) 来保证线程安全。对于 Java 开发人员来说，使用 synchronized 很简单，不用考虑锁定和解锁。这些具体的细节由 [JVM](/javarevisited/7-best-courses-to-learn-jvm-garbage-collection-and-performance-tuning-for-experienced-java-331705180686) 封装，对开发者是透明的。