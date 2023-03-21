# 学习之旅— —源代码分析

> 原文：<https://medium.com/javarevisited/netty-learning-journey-source-code-analysis-netty-d4b4c9196577?source=collection_archive---------7----------------------->

## 线程本地分配机制和 PooledByteBuf 线程级对象池原理分析

![](img/b87857bb2568352004ad51f376b5f4e5.png)

瑞兰德·迪恩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

上一章讲 Netty 内存分配时，没有考虑本地线程缓存，也就是 Netty 分配内存时，首先尝试从线程本地缓存申请，如果…