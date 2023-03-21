# Spring @每个人都会犯的事务性错误

> 原文：<https://medium.com/javarevisited/spring-transactional-mistakes-everyone-did-31418e5a6d6b?source=collection_archive---------0----------------------->

![](img/bfbb82131cbd67da8ae09c8954ab1ad8.png)

[粘土银行](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

大概用的最多的一个 Spring 注释就是 ***@Transactional*** 。尽管它很受欢迎，但它有时会被误用，导致一些事情不是软件工程师想要的。

在这篇文章中，我收集了我个人在项目中遇到的问题。我希望这个列表能帮助你更好地理解事务，并帮助你解决一些问题。

# 1.同一类中的调用

***@ Transactional***很少被足够多的测试覆盖，这就导致了有些问题乍一看是看不出来的。因此，您可能会遇到以下代码:

批注在 registerAccount 方法中不起作用

在这种情况下，当调用`registerAccount()`时，保存用户和创建团队不会在普通事务中执行。***@ Transactional***由[面向方面编程](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop)提供支持。因此，当一个 bean 被另一个 bean 调用时，就会发生处理。在上面的示例中，方法是从同一个类中调用的，因此不能应用任何代理。其他标注如[***@ Cacheable***](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#cache)也是如此。

这个问题可以用三种基本方法解决:

1.  自我注射
2.  创建另一个抽象层
3.  通过包装`createAccount()`调用，在`registerAccount()`方法中使用 **TransactionTemplate**

第一种方法似乎不太明显，但这样一来，如果***@ Transactional***包含参数，我们就避免了逻辑的重复。

该注释在 registerAccount 方法中起作用

如果你用 Lombok，别忘了在你的 lombok.config 中添加 [***@Lazy*** 。](https://stackoverflow.com/questions/59505213/how-to-use-lazy-annotation-in-a-class-constructor-with-lombok)

# 2.处理不是所有的异常

默认情况下，回滚只发生在[运行时异常](http://www.java67.com/2012/12/difference-between-runtimeexception-and-checked-exception.html)和[错误](https://javarevisited.blogspot.com/2013/06/10-java-exception-and-error-interview-questions-answers-programming.html#axzz6kGEVKsf7)时。同时，代码可能包含已检查的异常，其中也有必要回滚事务。

如果需要在出现 StripeException 时回滚，请将 roll back 设置为

# 3.事务隔离级别和传播

通常，开发人员添加注释时并没有真正考虑他们想要实现什么样的行为。几乎总是使用默认隔离级别`READ_COMMITED`。

理解[隔离级别](https://en.wikipedia.org/wiki/Isolation_(database_systems))对于避免以后很难调试的错误至关重要。

例如，如果您生成报告，您可以通过在事务期间多次执行相同的查询来选择默认隔离级别的不同数据。当并行事务在此时提交某个东西时，就会发生这种情况。使用`REPEATABLE_READ`将有助于避免这种情况，并节省大量故障排除时间。

不同的传播有助于在我们的业务逻辑中绑定事务。例如，如果您需要在另一个[事务](https://javarevisited.blogspot.com/2011/11/database-transaction-tutorial-example.html)中运行一些代码，而不是在外部事务中，您可以使用`REQUIRES_NEW`传播来挂起外部事务，创建一个新的，然后恢复外部事务。

# 4.事务不锁定数据

有时，当我们在[数据库](/javarevisited/top-5-courses-to-learn-mysql-in-2020-4ffada70656f)中选择一些东西，然后更新它，并认为由于所有这些都是在一个事务中完成的，并且事务具有原子性属性，因此这段代码作为单个请求执行。

问题是没有任何东西阻止另一个应用程序实例作为第一个实例同时调用`findAllByStatus`。因此，该方法将在两个实例中返回相同的数据，并且数据将被处理两次。

有两种方法可以避免这个问题。

## 选择更新(悲观锁定)

PostgreSQL 中的选择更新

在上面的示例中，当执行 select 时，这些行被阻塞，直到更新结束。该查询返回所有已更改的行。

## 实体的版本控制(乐观锁定)

这种方式有助于避免堵塞。这个想法是给我们的实体添加一个列`version`。因此，只有当数据库中实体的版本与应用程序中的版本相匹配时，我们才能选择数据并更新它。在使用 [JPA](https://spring.io/projects/spring-data-jpa) 的情况下，可以使用***@版本*** 标注。

# 5.两个不同的数据源

例如，我们创建了一个新版本的数据存储区，但仍需要在一段时间内维护旧版本。

当然，在这种情况下，只有一个`save`会被事务处理，即在那个被认为是默认的[**transactional manager**](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction)中。

[弹簧](/javarevisited/top-10-free-courses-to-learn-spring-framework-for-java-developers-639db9348d25)在这里提供了两种选择。

## ChainedTransactionManager

ChainedTransactionManager 是声明多个数据源的一种方式，在这种方式中，在出现异常的情况下，回滚将以相反的顺序发生。因此，对于三个数据源，如果第二个数据源在提交期间发生错误，只有前两个数据源会尝试回滚。第三个已经提交了更改。

## JtaTransactionManager

该管理器允许使用基于两阶段提交的完全受支持的分布式事务。但是，它将管理委托给后端 JTA 提供商。可能是 Java EE 服务器，也可能是独立解决方案( [Atomikos](https://www.atomikos.com/) 、 [Bitrionix](https://github.com/bitronix/btm) 等)。).

# 结论

交易是一个棘手的话题，知识上经常会有问题。大多数时候，它们没有被测试完全覆盖，因此大多数错误只能在代码审查中被注意到。如果事故发生在生产中，找到根本原因总是一个挑战。

你可能喜欢的其他 **Java 和 Spring 文章** **和资源**

</javarevisited/5-best-spring-data-jpa-courses-for-java-developers-45e6438be3c9>  </javarevisited/12-advanced-spring-framework-courses-for-java-programmers-a273f6e4448c>  <https://javarevisited.blogspot.com/2020/05/top-20-spring-boot-interview-questions-answers.html>  

> 如果你不是媒体成员，我强烈推荐你加入媒体，阅读不同领域伟大作家的精彩故事。你可以**在这里加入介质**</@somasharma_81597/membership>