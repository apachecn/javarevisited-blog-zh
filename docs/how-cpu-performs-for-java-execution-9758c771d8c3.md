# CPU 如何执行 Java

> 原文：<https://medium.com/javarevisited/how-cpu-performs-for-java-execution-9758c771d8c3?source=collection_archive---------1----------------------->

在这篇文章中，我们讨论了 CPU 如何响应每一个 Java 操作。CPU 如何以优化的方式处理不同的操作。在本文中，您将清楚地了解 CPU、CPU 缓存、L1、L2 和 L3 缓存等术语，以及它们如何帮助执行 CPU 操作。

![](img/6b099ea38b41f849281db58a46e1e9d6.png)

照片由[胡佛·董](https://unsplash.com/@lozt?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在阅读本文之前，希望您了解 Java 基准测试工具 JMH。关于这篇文章…