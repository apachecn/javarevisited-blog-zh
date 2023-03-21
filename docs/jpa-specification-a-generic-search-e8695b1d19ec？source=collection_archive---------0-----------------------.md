# JPA 规范:通用搜索

> 原文：<https://medium.com/javarevisited/jpa-specification-a-generic-search-e8695b1d19ec?source=collection_archive---------0----------------------->

## 了解如何创建简单而通用的搜索

![](img/4bc4f4027dee8e60ba156f60ebdbdd54.png)

照片由[is AAC benhessed](https://unsplash.com/@isaacbenhesed?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在本文中，我们将讨论 JPA 规范的一个常见用例。

首先，我们将从使用 JPA 规范会简化我们整体实现的场景开始。接下来，我们将看看如何在我们的代码中使用它。最后，我们将探索它的实现。

# 方案

**一个常见的用例是，当我们需要基于一些列来查找记录时，我们在运行时定义这些列。**

例如，让我们假设我们正在处理一个图书馆管理系统。想象一下，要求使用关键字搜索书籍，而不明确指出它是否来自`title`、`genre`、`author`、*、*等等。此外，在这个需求中，我们需要对我们的大多数域应用这种搜索，而不仅仅是`Book`。

有了这个需求，现在让我们看看我们的抽象解决方案。

# **解决方案**

在深入研究它的实现之前，让我们看一下这个解决方案的整体以及我们将如何使用它。

首先，让我们快速创建一个数据集。为简单起见，我们将使用 XML:

有了这个数据集，让我们尝试使用`“Spring”`作为关键字来搜索书籍。

为了演示我们的解决方案，现在让我们创建一个简单的集成测试:

从断言中我们可以看到，搜索`“Spring”`作为关键字，成功检索到 2 本书:[春天在行动](/javarevisited/5-advanced-spring-framework-books-experienced-java-developers-should-read-in-2020-best-of-lot-2a786fc5ad31?source=---------6-----------------------)和 [*春天在行动*](/hackernoon/top-5-spring-boot-and-spring-cloud-books-for-java-developers-75df155dcedc?source=---------23------------------) 。

此外，值得强调的是`BookRepository`需要扩展`JpaSpecificationExecutor`，以拥有`findAll(searchSpecification)`方法。

现在很清楚，`GenericSearchSpecification` **使我们能够将这种搜索应用到我们的任何领域**。现在让我们讨论它的实现。

# 履行

在前一节中，我们已经看到了名为`GenericSearchSpecification`的类的使用。现在让我们来处理它的实现。

(如果你想看整个项目，我已经包括了下面的链接。)
以下是仅必要部分的片段:

正如我们从片段中看到的，我们创建了实现`Specification`接口的`GenericSearchSpecification`类。它有两个属性:一个列表`columns`和一个`search`字符串。

接下来，我们从`Specification`接口实现了`toPredicate`方法。此外，我们创建了一个`buildExpression`方法来处理域嵌套属性的搜索，比如`“author.name”`。

# 结论

在本教程中，我们探索了 JPA 规范的一个常见用例，以及解决方案及其实现。

本文的完整源代码可以在 [Github](https://github.com/emyasa/medium-articles/tree/master/java-persistence-api) 上获得。

如果您正在寻找另一篇与 [JPA](https://javarevisited.blogspot.com/2018/01/top-5-hibernate-and-jpa-courses-for-java-programmers-learn-online.html) 相关的有趣文章，请随意查看:

[](/javarevisited/spring-jpa-when-to-use-join-fetch-a6cec898c4c6) [## Spring JPA:何时使用“Join Fetch”

### 避免 N+1 次查询并保留检索逻辑

medium.com](/javarevisited/spring-jpa-when-to-use-join-fetch-a6cec898c4c6)