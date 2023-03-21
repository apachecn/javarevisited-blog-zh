# CQRS &事件采购 III:框架

> 原文：<https://medium.com/javarevisited/cqrs-event-sourcing-iii-the-framework-c9025a31738a?source=collection_archive---------1----------------------->

![](img/5eab001a2e2ced58b478187abb38dac5.png)

劳拉·奥克尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

让我们看看应用程序的工作方式，特别是事件存储和事件总线。

这个实现是在[科特林](/javarevisited/top-5-courses-to-learn-kotlin-in-2020-dfc3fa7706d8)中完成的，但是同样的设计可以用不同的语言重写。完整的代码示例可在 [Github](https://github.com/min-maxx/CQRS_Kotlin.git) 上获得。

1.  [简介](https://towardsdev.com/cqrs-event-sourcing-i-introduction-2af1458d6ac8?sk=3f4f0adae3ae71ab49e08aba3d569d13)
2.  [用例实现](/@min-maxx/cqrs-event-sourcing-ii-use-case-implementation-e87e96a4e223)
3.  **CQRS 框架**