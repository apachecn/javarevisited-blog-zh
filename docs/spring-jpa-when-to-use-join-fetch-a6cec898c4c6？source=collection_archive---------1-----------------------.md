# Spring JPA:何时使用“Join Fetch”

> 原文：<https://medium.com/javarevisited/spring-jpa-when-to-use-join-fetch-a6cec898c4c6?source=collection_archive---------1----------------------->

## 避免 N+1 次查询并保留检索逻辑

[![](img/009b44a4c7bdfb9d1b227f05241a2401.png)](https://medium.com/javarevisited/top-5-books-to-learn-hibernate-for-java-developers-b2cb4b16ccd6?source=---------14------------------)

照片由 [Sam Pak](https://unsplash.com/@melocokr?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

刚接触 [JPA / Hibernate](/javarevisited/top-5-books-to-learn-hibernate-for-java-developers-b2cb4b16ccd6?source=---------14------------------) 的开发人员遇到的第一个也是最常见的问题之一就是`n+1 queries`。是因为通常负责`n+1 queries`的代码看起来并不怪异但是看起来很简单。这使得其他开发人员意识不到性能问题，只有在数据库中有足够的数据/记录时才发现它。

它可能看起来像这样简单:`person.getAddresses();`

在本文中，我将讨论一个简单但常见的场景，其中使用“Join Fetch”是有益的。我还将包括两个集成测试来比较性能。但是首先，让我们阅读文档中的定义。

## Hibernate 文档

> *" A****" fetch "****join 允许使用单次选择来初始化值的关联或集合及其父对象。这在集合的情况下特别有用。*

这里有一个简单的场景来演示上面的定义:

## 领域实体/模型:

> *有* ***人*** *实体有* ***集合*******地址****

*现在我们已经准备好了我们的域，创建一个`PersonRepository`，然后查询来自`Person`实体和`display each address line`的所有记录。让我们从导致`[n+1 queries](https://www.java67.com/2016/02/top-20-hibernate-interview-questions.html)`的错误方法开始。*

# *N+1 个查询结果*

*下面是一个集成测试，演示了导致`n+1 queries`的代码。*

> *根据集成测试的要点，我有意删除了导入和其他注释——这样我们就可以专注于重要的部分。*

## *运行上述集成测试的结果:*

*`121515718 nanoseconds spent executing **51 JDBC statements**;`*

*检索了`50`条记录，执行了`51`个查询，花费了大约`121 ms`*

*这可能看起来微不足道，因为我们只有 50 条记录，但这清楚地展示了`n+1 queries`场景。现在让我们尝试使用“Join Fetch ”,同时保留这种显示所有地址行的方式，并使用相同的记录比较结果。*

# *连接提取结果*

*下面是`PersonRepository`的代码，它有一个利用 JPQL 来使用“连接提取”的`retrieveAll`方法。*

*集成测试与上一个几乎相似，唯一的不同是我们现在使用我们刚刚创建的`retrieveAll`方法。*

> *根据集成测试的要点，我有意删除了导入和其他注释——这样我们就可以专注于重要的部分。*

## *运行上述集成测试的结果:*

*`15411157 nanoseconds spent executing 1 JDBC statements`*

*检索了`50`条记录，执行了`1`个查询，花费了大约`15 ms`*

# *结论*

*`N+1 Queries`:执行`51`查询，取约`121 ms`
`JOIN FETCH`:执行`1`查询，取约`15 ms`*

*上述比较虽然可能会有所不同，并且仅限于`50`条记录，但在性能上存在巨大差距，并且可能会有与记录数量成正比的指数差异。*

*使用 Join Fetch 是解决 n+1 查询的一种方法，同时保留了检索逻辑。有鉴于此，还有其他方法，如预测等。*

*本文的完整源代码可以在 [Github](https://github.com/emyasa/medium-articles/tree/master/java-persistence-api) 上找到。*

*我希望你喜欢读这篇文章。*

*编码快乐！*