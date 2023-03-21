# 数据库的并发和锁定

> 原文：<https://medium.com/javarevisited/concurrency-and-locking-on-databases-daa14e2aa68d?source=collection_archive---------0----------------------->

## 解决使用 JPA、Hibernate 和 Spring 数据时的并发问题。

![](img/9ee10d34aec537faf5a52cc8a4455ca5.png)

托拜厄斯·菲舍尔在 [Unsplash](https://unsplash.com/s/photos/database?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

一旦应用程序上的不同线程(或微服务生态系统中的不同应用程序实例)需要对同一个数据库表进行更改，最终就会出现竞争情况。

通过对同一个表进行操作，这些不同的线程最终会想要更新同一行。如果没有进一步的控制，一些更新将无声无息地丢失…

[![](img/071c0b3cc3d0a1fed5a94a60402af978.png)](https://www.java67.com/2016/02/top-20-hibernate-interview-questions.html)

默默的失去改变

很明显，当我们谈论在两个流中更新同一个字段时，这种情况会发生。

但是，在更新不同的字段时也会出现这种情况！默认情况下， [Hibernate](/javarevisited/top-5-hibernate-online-training-courses-for-beginners-and-advance-java-programmers-469460596b2b) 为所有实体字段发出一个 UPDATE 语句，因此整行都被更新。Hibernate 的`[@DynamicUpdate](https://docs.jboss.org/hibernate/orm/5.4/javadocs/org/hibernate/annotations/DynamicUpdate.html)`注释不就解决问题了吗？它只会更新已更改的字段，对吗？嗯，no. [Hibernate](/javarevisited/top-5-books-to-learn-hibernate-for-java-developers-b2cb4b16ccd6?source=---------14------------------) 会将内存中实体的状态与数据库进行比较。然后，它将为所有不同的列生成一个`UPDATE`语句，不管它们在哪里发生了更改。因此，保存内存中的实体仍然会覆盖数据库中所做的更改！

这是一个无声的问题，因为没有引发错误来标记覆盖。唯一的解决方案是重新加载记录并重试更新。但是要这样做，应用程序必须知道存在冲突。

这个问题有两个可行的解决办法。所有这些都要求在处理受影响的实体时事务处于活动状态。

# 乐观锁定

可以并发处理的每个 JPA 实体都用一个`[@Version](https://docs.jboss.org/hibernate/jpa/2.1/api/javax/persistence/Version.html)`列增强。

当对自上次读取以来已被修改的行进行更新时，版本号不再相同，并且会引发异常。

通过重新加载受影响的实体、重新应用更改并再次保存，可以实现恢复。

乐观锁定的缺点:

1.  冲突只有在提交时才知道，所以在所有处理完成后。事务将在此时回滚。当使用其他非事务性资源来处理实体时，这可能是一个问题。
2.  重试机制的复杂性:只能回放处理的结果。如果处理中涉及非事务性资源，必须注意不要重复[非幂等操作](https://javarevisited.blogspot.com/2016/05/what-are-idempotent-and-safe-methods-of-HTTP-and-REST.html#axzz5j9AEsxuT)。

# 悲观锁定

换句话说，在数据库级别使用带有行锁定的长时间运行的事务。每个想要处理行的事务都需要等待，直到另一个事务完成或者超时。当事务完成时，锁被释放。

锁超时会导致异常，但不一定会导致事务回滚。只有在`[@Transactional](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Transactional.html)`方法或`[TransactionTemplate](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/support/TransactionTemplate.html)`回调中使用 Spring 事务时，回滚才有保证，因为如果这些方法抛出异常，Spring 会回滚事务。

最常见的悲观锁定模式是锁定行，防止在事务开始时读取和修改该行。这被称为*悲观写*锁。当同一行被锁定时，任何其他事务都不能读取或修改该行。它经常导致一个`SELECT … FOR UPDATE`或类似的声明被发布。

也可以锁定该行，只允许修改，这意味着其他[事务](https://javarevisited.blogspot.com/2011/11/database-transaction-tutorial-example.html#axzz5WDqhDqX3)可以随时读取它。这是一个*悲观读*锁。并非所有的数据库或 JPA 实现都支持这种模式，通常会自动升级到悲观写锁。

所需的 JPA 锁定模式可以按照本 [Java EE 教程中关于锁定模式](https://javaee.github.io/tutorial/persistence-locking002.html)的描述进行配置。当使用 Spring 数据时，我们可以在 Spring 存储库方法中使用`[@Lock](https://docs.spring.io/spring-data/data-jpa/docs/current/api/org/springframework/data/jpa/repository/Lock.html)`注释来指定所需的锁模式。锁定超时值可以通过设置应用于 Spring 存储库方法的`[@QueryHints](https://docs.spring.io/spring-data/data-jpa/docs/current/api/org/springframework/data/jpa/repository/QueryHints.html)`注释中的`javax.persistence.lock.timeout`提示来定义。

悲观锁定的缺点:

1.  依赖数据库进行同步。
2.  超时必须被配置为一个提示，所以它不是一个契约，因此不一定可以在数据库之间移植。
3.  当多个资源被锁定时，可能会发生死锁。
4.  在数据库级别使用更多资源。

# 应该用哪个？

看情况。一如既往。

理想情况下，应用程序流应该以这样一种方式设计，即[并发性](/javarevisited/8-best-multithreading-and-concurrency-courses-for-experienced-java-developers-8acfd3b25094)不是问题。因此，没有共享资源。这个一般原则也适用于数据库资源，比如**行**。

然而，当不可能防止不同的流同时更新同一行时，有两个因素会影响对这些机制之一的选择。

最后，应该在权衡以下任何一种情况所带来的后果之后做出选择:

## 交通

悲观锁定增加了数据库服务器所需的资源，而乐观锁定将负担留给了客户端。

对于低流量，悲观锁定可能是合适的。但是，如果流量增加，数据库服务器可能会受到资源的限制，从而导致另一种类型的问题。此外，客户机中的线程将被阻塞，直到行锁被释放，从而降低了客户机处理请求的能力。

当流量水平上升时，乐观锁定是更好的选择。

## 并发

乐观锁定要求客户端应用程序在发生冲突时刷新其数据。这种刷新增加了数据库往返次数，从而增加了数据库服务器的负载。

高并发性场景是悲观锁定的理想选择。

# 结论

有两种方法可用于在[数据库](/hackernoon/top-5-sql-and-database-courses-to-learn-online-48424533ac61)级别管理并发:**乐观锁定**和**悲观锁定。**

在两种锁定模式下，应该在仔细考虑流量和并发性对应用程序和数据库服务器的影响后，选择一种模式或两种模式的组合。

众所周知，负载测试应始终在预期的最大流量水平下执行，以增强对解决方案的信心并尽早发现问题。

# 资源

关于`@QueryHints`注释的文档:[query hints(Spring Data JPA 2 . 4 . 7 API)](https://docs.spring.io/spring-data/data-jpa/docs/current/api/org/springframework/data/jpa/repository/QueryHints.html)

链接到一个关于锁模式的 Java EE 教程。

关于`@Lock`注释的文档:[锁(Spring Data JPA 2.4.7 API)](https://docs.spring.io/spring-data/data-jpa/docs/current/api/org/springframework/data/jpa/repository/Lock.html)

关于`@Transactional`注释的文档:[事务型(Spring Framework 5.3.5 API)](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Transactional.html)

关于`@Version`注释的文档:[版本(hibernate-JPA-2.1-API 1 . 0 . 0 . final API)(jboss.org)](https://docs.jboss.org/hibernate/jpa/2.1/api/javax/persistence/Version.html)

关于`@DynamicUpdate`的文档注释:[dynamic update(Hibernate JavaDocs)(jboss.org)](https://docs.jboss.org/hibernate/orm/5.4/javadocs/org/hibernate/annotations/DynamicUpdate.html)