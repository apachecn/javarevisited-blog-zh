# 延迟加载、聚合和 CQRS

> 原文：<https://medium.com/javarevisited/lazy-loading-aggregate-and-cqrs-10feea6d391?source=collection_archive---------0----------------------->

## 理解这些模式来处理应用程序的复杂性。

![](img/8eefe626a63c7176a69ce02391b5a0e9.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在本文中，我将通过一个例子来解释何时使用每种模式，何时不使用。从头到尾都是同一个例子，一步一步地解释。

# 变得太大的聚合