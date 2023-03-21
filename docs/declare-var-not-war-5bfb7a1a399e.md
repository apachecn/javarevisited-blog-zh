# 局部变量类型推断:声明 var，而不是 war

> 原文：<https://medium.com/javarevisited/declare-var-not-war-5bfb7a1a399e?source=collection_archive---------3----------------------->

> 在本文中，我将尝试使用保留类型名 var 来解释 Java 10 的新特性局部变量类型推断。

![](img/e5bfb71c8374d6bc34fcb8095e164217.png)

照片由[卡斯帕·卡米尔·鲁宾](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Java 变化很快，随着新的 6 个月发布周期的到来，我们将在每个版本中尝试新的特性。Java 10 中新增了一个[特性](https://openjdk.java.net/jeps/286) ***局部变量类型推理*** 。它的基本目的是减少样板代码，提高用初始化器声明局部变量时的可读性。

> 程序必须写给人们阅读，顺便提一下，也必须写给机器执行~ **哈罗德·艾贝尔森**

我们用一个例子来理解这个。以下代码是在没有使用[局部变量类型推理](https://javarevisited.blogspot.com/2018/03/finally-java-10-has-var-to-declare-local-variables.html)的情况下编写的。

根据 Java 10 局部变量类型推理进行重构后。

在上面重构的代码中，编译器可以通过查看 RHS 声明来推断声明的类型本身。

这些只是一些例子，帮助你理解这个特性以及我们如何使用局部变量类型推理。

现在让我们了解一下局部变量类型推理在哪里可以使用，在哪里不可以使用。

## 在那里它可以被使用

1.  局部变量初始值设定项
2.  增强型`for`循环中的索引
3.  局部变量在传统的`for`循环中声明
4.  [try-with-resources 语句的资源变量](https://javarevisited.blogspot.com/2011/09/arm-automatic-resource-management-in.html)
5.  隐式类型 [lambda 表达式](/javarevisited/7-best-java-tutorials-and-books-to-learn-lambda-expression-and-stream-api-and-other-features-3083e6038e14?source=---------14------------------)的形参。(Java [11](https://openjdk.java.net/jeps/323) 新增支持)

下面的代码片段展示了一些有效的例子。

## 在不能使用的地方

1.  菲尔茨
2.  方法参数
3.  方法返回类型
4.  没有任何初始化的局部变量声明
5.  无法使用空值进行初始化

我试图在下面的代码片段中解释，如果以不受支持的方式使用它，会出现什么样的编译器错误。

## 有什么好处？

如果你问我个人，我认为开发者应该明智地使用它。众所周知，每当一个新功能出现时，你肯定会兴奋不已，想要尝试一下。但是你必须明白，作为开发人员，我们阅读代码的次数比我们编写代码的次数要多。

因为这关系到可读性，所以有些人会喜欢它，有些人会讨厌它。因此，如果在代码审查期间，有人说他们无法获得声明的类型`var` ,那么这意味着其他人不太清楚，所以也许切换回我们显式声明类型的老式方法并不是那么糟糕。同样，在某些情况下，声明的类型非常明显，那么你可以跳过显式声明的类型，使用*变量*。

## 结论

在本文中，我们已经介绍了什么是局部变量类型推断【Java 10 的新特性,并举例说明了它在哪些地方可以使用，在哪些地方不被支持。可以进一步阅读 OpenJDK 团队准备的这些关于局部变量类型推理 ***的[FAQ](https://openjdk.java.net/projects/amber/LVTIFAQ.html)。***

## 支持我

如果你喜欢你刚刚读到的，你可以给我买杯咖啡。

[![](img/883994fb2cde2cb170202b796dac9bb8.png)](https://www.buymeacoffee.com/meashish)

## 进一步阅读

可以继续看我之前的一些文章。

<https://ashishtechmill.com/10-books-every-java-developer-must-read>  <https://ashishtechmill.com/containerizing-spring-boot-application-with-jib> 