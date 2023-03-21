# 在生产数据库上运行 SQL 查询时要记住的 5 件事

> 原文：<https://medium.com/javarevisited/5-things-you-should-keep-in-mind-while-running-sql-queries-on-production-databases-bf29aa8d75da?source=collection_archive---------1----------------------->

![](img/22a006e58897bbc56b3cf37e34bfd7d1.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[埃斯特·扬森斯](https://unsplash.com/@esteejanssens?utm_source=medium&utm_medium=referral)拍摄的照片

你有没有遇到过这样的情况，一些看起来无害的行为导致了生产问题，而且时间太长了？我希望你没有，因为这肯定不是一次愉快的经历。

其中一个看起来无害的动作是在生产数据库上运行 [SQL 查询](http://javarevisited.blogspot.sg/2012/12/how-to-find-second-highest-or-maximum-salary-sql.html)。在我职业生涯的早期，我也遇到过这种情况，我删除了一些重复的配置，但一周后发现它停止向下游发布消息。

当您在复杂的系统中工作时，该系统有如此多的组件、数百万行代码、数千种配置以及包含数百个表的许多数据库，您必须非常小心地对待您所做的任何事情。通常没有真正的方法来执行类似生产的测试，因此最好的办法是尽可能隔离和限制您的变更。

总之，所有这些背景知识是因为今天我将与大家分享一些在查询生产或实时数据库时防止生产问题的技巧。

由于许多数据科学家不是 SQL 专家，但他们确实编写了 [SQL 查询](https://www.java67.com/2013/04/10-frequently-asked-sql-query-interview-questions-answers-database.html)、[存储过程](http://javarevisited.blogspot.sg/2013/02/-create-and-call-mysql-stored-procedure-database-sql-example-tutorial.html)，并与测试和生产数据库进行交互，因此他们看似无害的操作很可能会导致生产问题。

去年同一时间，我们遇到了这样一个事件，数据科学家的**选择查询阻塞了生产中的一些流程**。看似无害的 SELECT 查询锁定了一个表，该表是尝试更新和向同一个表中插入数据的过程所需要的。

数据科学家在一天结束时运行查询，然后忘记了这件事，第二天早上才发现一些重要的工作还没有完成，它们从昨晚就开始运行了。最终，数据库管理员参与进来，他们切断了阻塞作业的连接，一切都恢复了。

顺便说一下，如果你不熟悉 [Microsoft SQL Server](/javarevisited/5-best-courses-to-learn-microsoft-sql-server-in-depth-e9f11b73c14a) 和 [T-SQL](/javarevisited/top-10-free-courses-to-learn-microsoft-sql-server-and-oracle-database-in-2020-6708afcf4ad7) ，那么我还建议你参加一个综合课程，学习 SQL Server 基础知识以及如何使用 T-SQL。如果你需要推荐，我建议你去 Udemy 上的 Brewster Knowlton 的 [**微软 SQL 初学者**](https://click.linksynergy.com/deeplink?id=JVFxdTr9V80&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fmicrosoft-sql-for-beginners%2F) 在线课程。从 SQL Server 中的 T-SQL 和 SQL 查询开始是很棒的课程。

[](https://click.linksynergy.com/deeplink?id=JVFxdTr9V80&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fmicrosoft-sql-for-beginners%2F) [## Microsoft SQL Server 和 T-SQL:从初级到高级

### Brewster Knowlton 曾在商业智能行业的各个方面工作过。具有丰富的写作经验…

udemy.com](https://click.linksynergy.com/deeplink?id=JVFxdTr9V80&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fmicrosoft-sql-for-beginners%2F) 

# 在生产数据库上运行 SQL 查询时要考虑的 5 件事

嗯，这是数据科学家在花费很长时间时忘记取消查询的极端情况之一，但发生这种情况的可能性很高，特别是如果您有生产访问权限，并且不太了解数据库锁定。

最好是提高您对锁定和隔离级别的了解，但为了安全起见，在生产环境中运行 SQL 查询时，您也可以遵循以下提示:

## 1.总是使用 NOLOCK 选项进行查询

如果某个作业也在运行并试图更新您正在查询的同一个表，这可能会导致生产问题。通过说 NOLOCK，您可以降低阻塞和死锁的风险，就像

```
SELECT Id, Name, Address from Employee with (NOLOCK) where Id= 2
```

当您使用 NOLOCK 提示运行查询时，它会指示查询引擎不要发出共享锁，并且不支持排他锁。使用 with NOLOCK 选项时，可能会读取未提交的事务或在读取过程中回滚的一组页面。

脏读是可能的。不过，值得注意的是，这个选项只适用于 SELECT 语句，对 Microsoft SQL Server 可用。请参阅 Udemy 上的 [**使用 Transact-SQL**](https://www.udemy.com/course/70-461-session-2-querying-microsoft-sql-server-2012/) 查询微软 SWL 服务器课程，了解更多关于表级和行级锁定的信息。

[](https://click.linksynergy.com/deeplink?id=JVFxdTr9V80&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2F70-461-session-2-querying-microsoft-sql-server-2012%2F) [## 70-461，761:使用 Transact-SQL 查询 Microsoft SQL Server

### 点评”指导老师讲解的很详细。非常容易理解。”——琳达·沈《精品课程……

udemy.com](https://click.linksynergy.com/deeplink?id=JVFxdTr9V80&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2F70-461-session-2-querying-microsoft-sql-server-2012%2F) 

## 2.在备份或辅助数据库服务器中运行查询

如果可能，总是在备份或辅助服务器上运行查询，而不是在主服务器上运行。只有当您因为觉得数据不是最新的而无法使用辅助服务器时，才使用主服务器，但底线是要避免在生产时间接触主服务器或主服务器。

## 3.在生产环境中运行之前，在 UAT/测试数据库上测试您的查询

这也是我以前在解释 UNIX 命令时可能告诉过你的规则。与在生产机器上运行之前应该在暂存箱上测试的 [UNIX 命令](http://java67.blogspot.com/2012/10/unix-command-to-find-symbolic-link-or.html)类似，在生产中运行查询之前，也应该首先在 QA 或 UAT 环境上运行查询。这不仅给你一个很好的想法，还可以避免语法错误和生产中的意外错误。

## 4.避免在工作时间接触生产数据库

如果您正在为一个系统工作，该系统有一些市场时段，如股票交易所，从早上 9 点到下午 4 点，那么您应该避免在这段时间接触您的生产数据库，并且只在市场时段之后运行您的查询。

在市场时段，DB 上有很多活动，您那些看起来无害的[选择查询](http://javarevisited.blogspot.sg/2011/10/selct-command-sql-query-example.html)很有可能会干扰它们。

## 5.仔细检查您的 SQL 查询

如果您不是单独工作，并且有一些团队成员，您可以随时要求您的同事对您想要在生产中运行的查询进行四目检查。如果您可以从 DBA 那里查看您的查询，那就更好了。

以上是在查询生产数据库时需要记住的一些**重要提示。您还应该了解更多关于数据库如何执行 SQL 查询的信息，如索引如何工作、锁定如何工作、表扫描、行扫描、表级锁定或行级锁定等。**

如果您很好地了解这些基础知识，您可以更确定地预测您的查询行为，并可能避免不愉快的意外。

有一本书可以在这里帮到你，那就是马库斯·温南(Markus Winand)所著的《SQL 性能讲解 》，这本书将从数据科学家的角度让你对数据库有一个很好的了解。

[](http://www.amazon.com/Performance-Explained-Everything-Developers-about/dp/3950307826/?tag=javamysqlanta-20) [## SQL 性能解释了开发人员需要了解的关于 SQL 性能的一切

### Amazon.com 上的 SQL 性能解释了开发人员需要了解的关于 SQL 性能的一切

www.amazon.com](http://www.amazon.com/Performance-Explained-Everything-Developers-about/dp/3950307826/?tag=javamysqlanta-20) 

> 你呢？在查询生产/实时数据库时，您需要遵循哪些提示或注意哪些事项？

您可能想探索的其他**有用的编程资源**有:

*   [初学者学习棱角的 10 门免费课程](/javarevisited/top-10-free-courses-to-learn-angular-framework-in-2020-bb62148c73d3)
*   [2023 年 React 开发者路线图](https://javarevisited.blogspot.com/2018/10/the-2018-react-developer-roadmap.html)
*   [40 多岁能学编码和 Web 开发吗？](/javarevisited/can-you-learn-programming-and-become-a-web-developer-in-the-40s-and-50s-f9e117f32721)
*   [2023 年学会反应的 10 门免费课程](/javarevisited/top-10-free-courses-to-learn-react-js-c14edbd3b35f)
*   成为全栈式网络开发人员的 10 门最佳课程
*   每个软件工程师都应该学习的 10 件事
*   [2023 年我最喜欢学的课程 node . js](/javarevisited/top-10-online-courses-to-learn-node-js-in-depth-8ef0e31ca139)
*   [我最喜欢的学习 HTML 和 CSS 的免费课程](/javarevisited/5-free-html-and-css-courses-to-learn-front-end-web-development-online-8b04517c6ecb?source=collection_home---4------0-----------------------)
*   [2023 年学习打字稿的前 7 门课程](/javarevisited/7-best-courses-to-learn-typescript-in-depth-58439e1ce729)
*   [7 门免费学习网页设计自举的课程](/javarevisited/7-free-courses-to-learn-bootstrap-for-web-designers-and-developers-5135215648f1)
*   [我最喜欢的深入学习 Web 开发的课程](/better-programming/my-5-favorite-courses-to-learn-web-development-in-2019-a5e74167f8b2)

感谢您阅读本文，如果您喜欢这些在生产环境中运行 SQL 查询的简单安全提示，请与您的朋友和同事分享。这很有帮助

**附注**——如果你正在寻找一些免费的课程来开始学习数据库和 SQL 基础知识，那么你应该查看 Udemy 上的 [**数据库和 SQL 查询简介**](http://bit.ly/2BQuq2O) 课程，它是完全免费的，你只需要一个免费的 Udemy 帐户就可以访问这门课程。

[](http://bit.ly/2BQuq2O) [## 免费数据库管理教程-数据库和 SQL 查询介绍

### 我是一名程序员、经理、教育家和游戏玩家。我喜欢数据和分析。在我的日常工作中，我处理数据库…

bit.ly](http://bit.ly/2BQuq2O)