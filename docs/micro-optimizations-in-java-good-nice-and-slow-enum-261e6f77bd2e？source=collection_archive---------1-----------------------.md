# Java 中的微优化。好，好，慢枚举

> 原文：<https://medium.com/javarevisited/micro-optimizations-in-java-good-nice-and-slow-enum-261e6f77bd2e?source=collection_archive---------1----------------------->

![](img/381a90f512cb59bb6ad9c187f7e5f412.png)

枚举是每个比“Hello World”大的应用程序的关键部分。我们在任何地方都使用它们。它们实际上非常有用:它们限制输入，允许您通过引用比较值，提供编译时检查，并使代码更容易阅读。然而，使用枚举有一些性能缺点。我们将在本帖中回顾它们。