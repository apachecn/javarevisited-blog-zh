# 使用@FunctionalInterface 注释的最佳实践

> 原文：<https://medium.com/javarevisited/best-practice-of-using-functionalinterface-annotation-b302f9b40594?source=collection_archive---------8----------------------->

![](img/1c2a3feaf22bb844be03f0148bbffbab.png)

Lambda 表达式和函数式编程被认为是 Java 8 的基石。

> 什么是 Lambda 表达式？

简单地传递一个逻辑块，就像一个匿名方法。

> **什么是功能界面？**

简单地说，函数接口就是具有单一抽象方法的接口。

功能接口被用作[λ表达式](/javarevisited/7-best-java-tutorials-and-books-to-learn-lambda-expression-and-stream-api-and-other-features-3083e6038e14?source=---------14------------------)的基础，这就是功能接口如何在技术上与λ表达式相关联。

> **编译器如何制作函数接口**

Java 编译器足够聪明，可以将任何具有单一抽象方法的接口作为函数接口。

反过来，如果一个类用注释明确标记，并且包含多个抽象方法或者根本不包含抽象方法，那么编译器会将其检测为错误。

> **应用@** 功能界面**注释**的最佳实践

正如你在上面的描述中看到的，在开发中确实没有必要明确地注释。那么我们为什么要明确地这么做呢？？？

> **为什么我们需要显式添加@** FunctionalInterface？？？

现在，让我们假设你正在和一个团队一起做一个项目，如果你没有使用注释，甚至你不知道注释，另一个开发人员可能会把你用一个抽象方法创建的任何接口当作一个函数接口。如果您稍后修改代码以具有一些其他抽象方法，代码将确实中断，并且它将不能作为功能接口工作。

现在我们清楚地知道为什么我们需要显式注释。因此，其他开发人员可以清楚地知道他们可以为哪些接口安全地应用 lambdas…