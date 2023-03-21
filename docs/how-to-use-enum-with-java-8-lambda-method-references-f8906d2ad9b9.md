# 如何将 Enum 与 Java 8 Lambda 和方法引用一起使用

> 原文：<https://medium.com/javarevisited/how-to-use-enum-with-java-8-lambda-method-references-f8906d2ad9b9?source=collection_archive---------1----------------------->

> 最初发表于[https://asyncq.com](https://asyncq.com/how-to-use-enum-with-java-8-lambda-method-references)

# 介绍

[λ](/javarevisited/8-best-lambdas-stream-and-functional-programming-courses-for-java-developers-3d1836a97a1d)和[方法引用](https://javarevisited.blogspot.com/2017/03/what-is-method-references-in-java-8-example.html)是 Java 8 发布时添加的最酷的特性之一，自发布以来，开发人员喜欢并开始在他们的 POC 或后来的生产项目中使用它。在本文中，我们将看到，借助于 enum、 [lambda、](https://javarevisited.blogspot.com/2014/02/10-example-of-lambda-expressions-in-java8.html)以及后来的[方法引用](https://javarevisited.blogspot.com/2017/08/how-to-convert-lambda-expression-to-method-reference-in-java8-example.html#axzz5gKl3DykI)，我们如何使我们的代码松散耦合，更易于维护和修改。

# Lambda 表达式和枚举