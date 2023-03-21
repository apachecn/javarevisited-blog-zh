# 使用 Java 模块创建自己的虚拟机

> 原文：<https://medium.com/javarevisited/create-your-own-vm-using-java-modules-a48bc5ed2222?source=collection_archive---------4----------------------->

在这篇文章中，让我们实际探索一下 Java 模块的概念。最后，让我们使用 java 模块及其 VM 创建一个小应用程序。

![](img/616d01a3c674bcdf2a1e36aca821ba8f.png)

由[丹尼尔·韦德罗](https://unsplash.com/@dwiadro?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Java modules 是作为 Java 9 的一部分引入的伟大特性之一。它用更强的封装来改进你的代码，它能够减少虚拟机的大小，改善执行时间，等等。