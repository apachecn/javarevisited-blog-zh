# JUnit 参数化测试

> 原文：<https://medium.com/javarevisited/junit-parameterized-tests-5be86a936ca0?source=collection_archive---------2----------------------->

## 一个测试就能统治所有人！

![](img/72f402003e72d73134b6d104c5e51126.png)

# 参数化测试？

从 JUnit4 开始，我们现在可以运行参数化测试，但是这意味着什么呢？这意味着我们可以用不同的值多次运行一个测试，很棒，不是吗？

# 如何？

是的，那很好，但是我怎么做呢？
这么简单:

*   用**@ run with(parametered . class)**注释您的测试类
*   创建用 **@Parameters** 注释的方法，该方法返回对象的 Iterable 作为测试数据集。
*   创建一个构造函数，它将相当于数据集的一行的内容作为参数。或者创建等同于一个参数的实例变量，并用 **@Parameter(X)** 对它们进行注释，其中 X 是数据集中某一行的参数索引。
*   创建一个将使用您的[实例变量](https://javarevisited.blogspot.com/2012/02/difference-between-instance-class-and.html)的测试用例。

好的，但是具体地我能有一个例子吗？

# 例如

## 要测试的类

让我们来看一个简单的类，它将 calcul 的字符串作为参数并返回结果。

## 经典 Junit 测验

我以一种经典的方式编写了这个测试类来测试我的每一个案例

如你所见，我对每个测试用例都有一个测试，这里我的类很简单，没有那么多行代码，但是如果我的类更复杂，会有更多的代码，多余的，不是吗？

## 参数化的

这是相同的测试，但是参数化了，您可以看到在 How-to 部分建立的所有以前的语句

*   跑步者
*   数据集
*   参数
*   测试谁使用它

因此，通过一次测试，我测试了我之前的 5 个测试用例。

感谢您的阅读，本文中的示例在 [Junit4](/javarevisited/top-10-courses-to-learn-eclipse-junit-and-mockito-for-java-developers-4de1e8d62b96) 中，如果您想要使用 [Junit5](/javarevisited/5-courses-to-learn-junit-and-mockito-in-2019-best-of-lot-f217d8b93688?source=---------20------------------) 的示例，我会写另一篇关于它的文章。