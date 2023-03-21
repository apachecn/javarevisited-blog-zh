# 对 NoSQL 说不

> 原文：<https://medium.com/javarevisited/say-no-to-nosql-6bab085aacd2?source=collection_archive---------1----------------------->

![](img/37fa3bcf64a1382b47180aa58c862c4e.png)

[马特·本内特](https://unsplash.com/@mbennettphoto?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

如果你有一个高负荷的项目或者上百万的用户，那么这篇文章不适合你。这篇文章是关于小项目的。

此外，假设您正在考虑潜在的数百万用户，他们根本不在地平线上。在这种情况下，考虑快速开发的数据库的最佳选择也是有意义的。在这种情况下，更重要的是快速构建 POC，而不是为未来的工作负载做好准备。

我理解将尽可能多的新技术引入项目的愿望。这种方法适用于宠物项目，但是在现实生活中会产生很多额外的 bug，并使系统的支持和开发变得非常复杂。

# 一致性

关系数据库解决了维护数据一致性的大问题。

1.  **数据模式**。您可以始终确保数据库中的所有数据都与模式匹配。没有意外的`[null](https://www.java67.com/2021/05/how-to-solve-nullpointerexception-in-java.html)`字段，没有过时的数据，没有人能确定它的模式。
2.  **ACID 事务**允许您始终以可预测的形式保存数据。没有中间状态，仅保存了一部分更改。不需要为了任务调度和幂等处理而产生任务调度和幂等处理。在一个事务中包含几个操作就足够了。
3.  [**外键**](http://javarevisited.blogspot.sg/2012/12/foreign-key-vs-primary-key-table-sql-database-difference.html) 保护数据的完整性，不允许留下与已删除实体相关的废弃数据。

# 指数

并不是所有的 [NoSQL 数据库](/javarevisited/5-best-nosql-database-programmers-and-developers-can-learn-42a0bdfa9a12)都允许你建立二级索引，更少的数据库允许你创建**唯一的**二级索引。

对于用户电子邮件的唯一性来说，创建一个[索引](https://javarevisited.blogspot.com/2013/08/difference-between-clustered-index-and-nonclustered-index-sql-server-database.html)而不是发明一系列复杂的操作就足够了，这真是太好了。

# 并发

并发性是一个困难的话题，总有出错的空间。

如果您需要实现一个预订模块或一个简单的支付系统，那么没有什么比对数据库中的行使用**悲观锁**更简单的了。代码的可读性显著提高，错误的数量最小化。

# 部署

最后，如果项目不是托管在云中，那么部署和支持一个 [NoSQL 解决方案](https://javarevisited.blogspot.com/2019/03/top-5-nosql-database-web-developers-should-learn.html#ixzz64aBvbXQ4)对你的[开发人员来说可能是一个挑战。可能会出现裂脑，文件描述符可能会用完，可能需要对 JVM 进行特殊的调优。从每个特定的数据库中能得到什么并不明显。](/javarevisited/the-2018-devops-roadmap-31588d8670cb)

同时，任何开发人员都有使用关系数据库的经验。

# 结论

所有这些问题都是可以解决的，但是一开始就花时间在这上面是没有意义的。

您仍然有时间忍受由多对多关系连接的数据的搭配。但是，当人们已经有信心认为一个想法可行，并且用户数量即将大幅增长时，这样做是值得的。