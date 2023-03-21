# 如何用 Java 实现 LRU 缓存

> 原文：<https://medium.com/javarevisited/how-to-implement-lru-cache-in-java-f82923cc3214?source=collection_archive---------1----------------------->

## 用 Java HashMap 和双向链表实现 LRU 缓存

> 最初发表于[](https://asyncq.com/how-to-implement-lru-cache-in-java)

**[![](img/6d17ebac82624e3cfc8ebfcfacf4b3e9.png)](https://javarevisited.blogspot.com/2020/05/fibonacci-series-in-java-8-with.html)**

## **介绍**

*   **缓存是一个[数据结构](/javarevisited/50-data-structure-and-algorithms-interview-questions-for-programmers-b4b1ac61f5b0)，其目的是在定义的时间段内存储一个对象。**
*   **此定义的周期逻辑可以基于对象的使用频率或…**