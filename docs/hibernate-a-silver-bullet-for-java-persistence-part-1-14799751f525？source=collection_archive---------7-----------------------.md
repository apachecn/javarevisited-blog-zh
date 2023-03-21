# hibernate——Java 持久性的银弹(第 1 部分)

> 原文：<https://medium.com/javarevisited/hibernate-a-silver-bullet-for-java-persistence-part-1-14799751f525?source=collection_archive---------7----------------------->

> 关于职业选手有一场长期的辩论。和缺点。JPA 或 Hibernate 的。(在本文中，我将交替使用这些术语)。一些作者声称 JPA 拿走了对 SQL 的控制。他们说，由于 Hibernate 作为一种工具，不能 100%执行所需的“数据库任务”，用户迟早会遇到问题。他们解决这个问题的方法是…不使用 JPA！

![](img/889c3f96d7fa57d0de04fc5f0d97b3dc.png)

[布拉德·巴摩尔](https://unsplash.com/@bradbarmore?utm_source=medium&utm_medium=referral)在[太空望远镜](https://unsplash.com?utm_source=medium&utm_medium=referral)上拍摄的照片

在本系列中，我将展示 Hibernate 的出色之处。当达到极限时，应使用**互补**工具。

简而言之，如果我一年去几次徒步旅行，我不会烧伤我的车。因为否则我每天早上都要徒步去我的办公室！

# Hibernate 最擅长什么？

*   **ORM** 。地图。因为我们用 Java 编程，所以我们想使用对象。我们想要类型安全和智能感知。
*   对象检索和持久化。我们可以很容易地通过这些变化**找到()**和**持久化()**后面的对象层次。

# 【Hibernate 在哪些方面表现不佳？

**查询**。实际上有一种 JPA 语言——JPQL，它允许我们编写一些*查询。我个人不喜欢它。因为这个“*一些*”并且因为我需要写一个字符串。我失去了 Java 编译器。所以，让我们寻找一个补充工具！*

*   [JPA repository](https://docs.spring.io/spring-data/jpa/docs/2.1.9.RELEASE/reference/html/#repositories.query-methods.query-creation)，它根据方法名构造查询。对于在实体上构建约束查询来说，这既简单又有用。

Hibernate 的基本 **find()** 和 **persist()** 与 JPA Repositories 相结合，通常可以满足 70–80%(如果不是更多)的应用程序数据库需求。并且做得非常干净和高效。

还剩下什么？ ***复杂的*** 查询。我们要委托给数据库的业务逻辑。我们通常会向数据库管理员咨询的事情。从业务角度来看，这可能是最重要的东西，但它并不超过我们数据库需求的 20-30%。

那么，对于剩下的 20–30%，您准备好放弃其他 70%的简单性了吗？我不推荐。相反，让我们选择一个工具来补充和提供缺失的功能。

# 流利的初级专业人员

[FluentJPA](https://github.com/streamx-co/FluentJPA) 让我们用 Java 编写 SQL，使用 JPA 模型定义的实体(映射)。它允许使用 Java 语言，具有类型安全、智能感知、重构和其他 IDE 特性。有了它，您可以满足其余的数据库需求。让我们看几个例子:

带有外部参数的简单查询的完整流程

用 Java 编写的带有 JPA 实体的子查询

相关子查询(第 8 行)

你可以用 [FluentJPA](https://github.com/streamx-co/FluentJPA) 做任何事情都没有限制。但最重要的是，您可以为每项任务选择最好的工具，而不会损害可维护性、功能或性能！

![](img/42f37e3d83d65125410540b4d5efe9f8.png)