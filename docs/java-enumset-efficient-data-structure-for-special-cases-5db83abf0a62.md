# Java enum set——适用于特殊情况的高效数据结构

> 原文：<https://medium.com/javarevisited/java-enumset-efficient-data-structure-for-special-cases-5db83abf0a62?source=collection_archive---------0----------------------->

## 效率越高，局限性越大。

![](img/df5f6a8099ba0353be02f1cc42dce457.png)

每个使用 Java 超过几天的人都知道 [Set 接口](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Set.html)和它最常见的实现 [HashSet](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/HashSet.html) 。

还有一些更专门化的实现，比如排序的[树集](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/TreeSet.html)。但是到底什么是[枚举集](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/EnumSet.html)？这是最…