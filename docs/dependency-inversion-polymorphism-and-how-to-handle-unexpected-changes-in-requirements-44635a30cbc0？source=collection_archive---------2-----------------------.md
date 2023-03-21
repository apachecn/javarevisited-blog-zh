# 依赖倒置、多态，以及如何处理需求中的意外变化

> 原文：<https://medium.com/javarevisited/dependency-inversion-polymorphism-and-how-to-handle-unexpected-changes-in-requirements-44635a30cbc0?source=collection_archive---------2----------------------->

在本文中，我们将看到如何对需求中的意外特性和变化做出反应。

![](img/3b561a2863daebcd726dbe03e7e85008.png)

布拉德·斯达克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 1.概观

在[我们的上一篇文章](/javarevisited/explaining-the-open-closed-principle-to-the-rubber-duck-with-a-hands-on-exercise-68a8d73ecc91)中，我们实现了一个简单的 [FizzBuzz](https://www.java67.com/2015/10/how-to-solve-fizzbuzz-in-java.html) 应用程序，同时关注[开闭原理](https://javarevisited.blogspot.com/2015/07/strategy-design-pattern-and-open-closed-principle-java-example.html)和可测试性。此外，我们使用了封装和…