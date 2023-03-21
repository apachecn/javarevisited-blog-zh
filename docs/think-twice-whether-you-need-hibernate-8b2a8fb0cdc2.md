# 三思是否需要休眠

> 原文：<https://medium.com/javarevisited/think-twice-whether-you-need-hibernate-8b2a8fb0cdc2?source=collection_archive---------0----------------------->

![](img/b406039021832860766758f231412057.png)

托拜厄斯·菲舍尔在 Unsplash[拍摄的照片](https://unsplash.com?utm_source=medium&utm_medium=referral)

我经常看到人们如何将冬眠纳入一个项目，而不是真正考虑他们是否真的需要它。过了一段时间，当服务越来越多的时候，他们开始怀疑这是不是一个错误。

让我们试着提前考虑 hibernate 的利弊，这样下次我们就可以决定是否需要在新的微服务中添加这种依赖性。也许简单的 [Spring JDBC](/javarevisited/top-10-free-courses-to-learn-spring-framework-for-java-developers-639db9348d25) 是有意义的，没有所有 JPA 的复杂性？

# 一堆不明显的东西

Hibernate 可能看起来像是大量的注释，可以神奇地从数据库中选择所有内容并保存。我们只需要正确地添加 ***@ManyToMany*** 、 ***@OneToMany*** 等注释即可。

如果你深入挖掘，许多程序员在工作面试中不能准确地说出他们的应用程序是如何工作的。例如，在下面的代码中，我们没有在任何地方显式调用`save()`方法，但是更改将保存在数据库中。

该实体将在数据库中更新

对我来说，这种代码让阅读变得非常困难。代码审查人员将花费大量时间来弄清楚发生了什么。下一次你不得不修复一个 bug 的时候，这将会花费很多时间。

# 懒惰抓取问题

另一个常见的情况是在事务中使用延迟获取。向实体添加一个惰性字段并在事务中引用它来获取数据似乎很有诱惑力。
然后再加一个字段，再加一个。除了几个对象的 1-2 个字段，我们很可能不需要所有的数据。但结果是，应用程序向数据库发送了 5-10 个请求，完全拉出了对象。相反，可以只写一个 select 请求必要的数据。

如果对字段的访问发生在服务(和事务)之外，仍然有这样荒谬的结构向代码审查者问好:

荒谬的构造获取一个懒惰的对象

似乎可以使用*抓取或 ***@EntityGraph*** 。但是 ***急切*** 影响其他代码， ***@EntityGraph*** 需要单独请求(如果我们已经在写了，还需要 hibernate 吗？).*

# *N+1 选择问题*

*用 hibernate 编写批量插入的代码不是一件容易的事情。不要忘记:*

1.  *添加一个属性到***application . yml***`spring.jpa.properties.hibername.jdbc.batch_size: 50`*
2.  *创建一个按批次大小递增的序列
    `CREATE SEQUENCE account_id_seq START 1 INCREMENT BY 50;`*
3.  *设置序列发生器以同时使用多个序列值。否则，[休眠](/javarevisited/top-5-hibernate-online-training-courses-for-beginners-and-advance-java-programmers-469460596b2b)将参考序列 N 次。*

*人们经常忘记序列生成器，这完全否定了批量插入的整个优化。*

*如果一切配置良好，Hibernate 真正方便的是级联批量插入。由于 hibernate 首先请求 id，这允许为相关实体的级联保存设置外键。通过遵循相同的场景，您可以在没有 Hibernate 的情况下做到这一点。*

*不要忘记使用 ***@BatchSize*** 来批量选择与 ***@ManyToMany*** 或 ***@OneToMany*** 注释关联的实体。否则，将执行 N+1 个请求。*

*如果您仍然决定使用 [Hibernate](/javarevisited/top-5-books-to-learn-hibernate-for-java-developers-b2cb4b16ccd6) ，我建议在您的测试中使用 [**QuickPerf**](https://github.com/quick-perf/quickperf) 库来确定到底有多少对数据库的请求被执行。*

*使用 QuickPerf 检查选择次数*

# *二级缓存*

*最后，值得一提的是 Hibernate 中的二级缓存。如果使用标准的 Ehcache 实现，那么当应用程序伸缩时，不同的实例将在缓存中存储不同的数据。*

*因此，服务的响应可能取决于请求到达的实例。*

*为了避免这种情况，最好现在就开始使用 Spring Cache。然后，在扩展时，连接一个分布式实现(Redis、Hazelcast、Apache Ignite 等)就足够了。).*

*我还要提醒你，在使用 Hibernate 的 [L2 缓存时，每个请求仍然会获得一个到数据库的连接。](https://javarevisited.blogspot.com/2017/03/difference-between-first-and-second-level-cache-in-Hibernate.html)*

*甚至为缓存的实体获取 JDBC 连接*

# *结论*

*我不是想说你千万不要用 [Hibernate](https://www.java67.com/2016/02/top-20-hibernate-interview-questions.html) 。在某些情况下，它会很有用。但我想说，你应该在一个项目的早期就一直思考这个问题，以免将来后悔。*