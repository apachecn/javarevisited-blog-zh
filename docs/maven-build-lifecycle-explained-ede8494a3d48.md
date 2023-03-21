# Maven 构建生命周期解释

> 原文：<https://medium.com/javarevisited/maven-build-lifecycle-explained-ede8494a3d48?source=collection_archive---------0----------------------->

通过这篇文章，让我们详细了解 maven 的生命周期。从本文中，您将了解 maven 如何基于生命周期工作，以及如何用您自己的实现覆盖生命周期阶段。

![](img/1d15b49f5e7787d08549290ce7a45204.png)

[切佩·尼科利](https://unsplash.com/@nicoli_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在 Maven 中，有三个内置构建生命周期，它们是默认的、干净的和现场的。每一个 maven 操作都属于这些 maven 生命周期中的任何一个。这些的分类…