# 扩展 Spring Boot 微服务

> 原文：<https://medium.com/javarevisited/scaling-spring-boot-microservices-81b7e8c61b73?source=collection_archive---------1----------------------->

![](img/ad3731ba9ac8a6eb610305172149595a.png)

[伊恩·泰勒](https://unsplash.com/@carrier_lost?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

假设我们已经编写了 Spring Boot 应用程序。它已经成功地工作了一段时间。现在，我们知道我们需要启动服务的多个实例来应对增加的负载和可用性。但是在开发过程中，我们没有考虑到这一点。

有什么可以阻止我们只是启动几个实例呢？

1.  使用调度程序
2.  使用 WebSockets
3.  存储在内存中的用户会话
4.  应用程序缓存——它可以是组件中的简单并发哈希表，也可以是 spring 缓存。

使用正确的工具，所有这些都可以很容易地适应。

# **调度程序**

使用分布式调度器不是最简单的任务，调整和重写代码需要时间。我们希望快速实现从 1 到 N 个同时运行的实例的转换。

幸运的是，有一个与 Spring 集成的库，它允许你用几个注释来做这件事。

Shedlock 在您的数据库中创建一个表(几乎支持任何存储)，并使用它来协调实例，以便一次只有一个实例运行特定的调度程序。

我们需要用默认锁启用 Shedlock 最多 5 分钟。

并添加 ShedLock'a 注释来指定特定调度程序的名称。

因此，在调度程序的下一个时钟周期，每个实例将尝试在锁表中写一行关于它自己的内容，第一个成功的实例将继续执行任务。其他人都会跳过滴答。

在我们的案例中，锁定任务将最多持续 5 分钟。任务完成后，实例将删除表中的锁。如果 5 分钟过去了，锁还没有被解除，其他实例将认为该实例在任务执行时已经冻结。

# **Websockets**

如果在我们的应用程序中，用户可以相互发送消息，为此，我们使用 WebSockets，那么当用户 Bob 连接到实例 A，而用户 Alice 连接到实例 b 时，可能会出现这种情况。

编写这样一个层是一项费力的任务，将大大延迟应用程序的缩放时间。相反，我们可以使用 Spring 的开箱即用的`BrokerRelay` 实现，它使用外部代理(RabbitMQ、ActiveMQ 等)。)来处理 WebSockets、管理订阅等等。

现在，`/topic`和`/queue`的所有消息都将被转发到外部代理。详细的交互方案可以在 [Spring 文档](https://docs.spring.io/spring-framework/docs/4.3.x/spring-framework-reference/html/websocket.html#websocket-stomp-message-flow)中看到。

# **用户会话**

当然，每个应用程序实例的会话不必不同。幸运的是，它们可以存储在任何数据库中。对于冷门存储，需要编写自己的 SessionRepository。

对于 [JDBC](/javarevisited/top-5-courses-to-learn-jdbc-and-database-connectivity-for-java-developers-free-and-best-of-lot-7945156fcc3?source=---------9------------------) ，加一个依赖就够了。

并在***application . properties:***中设置属性

然而，在这种情况下，值得记住的是，会话数据本身将被存储为 blob，有时，当更新 [Spring](/javarevisited/10-best-online-courses-to-learn-spring-framework-in-2020-f7f73599c2fd) 时，由于反序列化旧对象的问题，您将不得不清除会话。

# **应用缓存**

我们不希望每个实例都有自己的缓存。否则，请求结果可能取决于负载平衡器将用户发送到哪里。

对于使用共享缓存的所有实例，连接外部存储是值得的，比如 Redis、Apache Ignite、Hazelcast 等。

对于 Spring-Cache 和 Redis，包含一个依赖项就足够了。

之后，通过`@EnableCaching`注释， [Spring Boot](/javarevisited/10-advanced-spring-boot-courses-for-experienced-java-developers-5e57606816bd?source=collection_home---4------0-----------------------) 将自动启用 Redis 作为默认配置。

例如，可以对其进行定制，以便为每个实体设置不同的生存时间。

# **结论**

因此，即使您在创建应用程序时没有考虑伸缩性，也有一组工具可以帮助您从 1 个实例移动到 N 个同时运行的实例，而无需重写服务本身的业务逻辑。