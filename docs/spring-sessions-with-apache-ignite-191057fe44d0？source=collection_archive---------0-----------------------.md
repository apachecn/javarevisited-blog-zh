# Apache Ignite 的春季会议

> 原文：<https://medium.com/javarevisited/spring-sessions-with-apache-ignite-191057fe44d0?source=collection_archive---------0----------------------->

![](img/743269007f49ada6696de0163a3c32fd.png)

莫里斯·萨尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

[Apache Ignite](https://ignite.apache.org/) 是一个高性能的分布式数据库，它将数据存储在内存中(也是持久的)。

与每一个 NoSQL 解决方案一样，它支持开箱即用的复制和分片，但也提供了 [ACID 事务](https://ignite.apache.org/features/acid-transactions.html)、 [SQL 支持](https://ignite.apache.org/docs/latest/SQL/sql-introduction)和[计算网格](https://www.gridgain.com/docs/latest/developers-guide/distributed-computing/distributed-computing)。

在本文中，我们将使用 Apache Ignite 作为 Spring 会话的数据库。例如，如果您已经将该工具用于 Map-Reduce 计算，则没有必要再部署一个数据源(如 Redis)来存储会话，您可以重用 Apache Ignite。

# 关系

有几种方法可以从 Java 应用程序连接到 ignite:

1.  胖客户端
2.  瘦客户端
3.  [JDBC 司机](https://javarevisited.blogspot.com/2015/07/javasqlsqlexception-no-suitable-driver-found-jdbc.html#axzz6v01lJ700)

## 胖客户端

我的建议是，如果不使用特定的操作，就避免使用胖客户端。重点是胖客户端作为没有数据的节点插入到集群拓扑中。默认情况下，[环形拓扑](https://en.wikipedia.org/wiki/Ring_network)用于节点通信，因此每个客户端节点都会降低整体通信速度(您可以使用 [ZooKeeper Discovery](https://www.gridgain.com/docs/latest/developers-guide/clustering/zookeeper-discovery) 来改进)。

## 瘦客户端

瘦客户端是我最喜欢的选择。您可以在此表中看到支持的功能。

因此，最新版本的瘦客户机几乎可以做任何事情，而且没有一些缺点。它不插入拓扑，而是连接到一个服务器节点。瘦客户端的版本可以不同于集群版本，这使得集群升级变得非常容易。

## JDBC 司机

[JDBC 驱动](https://www.gridgain.com/docs/latest/developers-guide/SQL/JDBC/jdbc-client-driver)是一种相对简单的连接方式，尤其是如果您以前使用过关系数据库，并决定用 Apache Ignite 替换以前的数据库。

如果你计划使用[分布式计算](https://www.gridgain.com/docs/latest/developers-guide/distributed-computing/distributed-computing)或低级操作`IgniteCache`，最好立刻选择瘦客户端。

因此，要连接到 Apache Ignite，我们需要添加一个依赖项:

并创建瘦客户机的 bean:

# 点燃弹簧数据

为了以 [Spring Data](https://spring.io/projects/spring-data) 的方式使用 Apache Ignite，我们需要添加这两个依赖项:

并通过添加***@ EnableIgniteRepositories***注释来启用 Ignite 储存库。

之后，我们可以使用***@ repository config:***创建存储库接口

# 春季会议

Apache Ignite 连接被建立， [Spring 数据仓库](https://www.java67.com/2021/01/spring-data-jpa-interview-questions-answers-java.html)被创建。现在，我们只需要创建一个`org.springframework.session.SessionRepository`的 bean。Apache Ignite 是键值存储，可以序列化/反序列化任何 Java objec T21，所以我们可以使用 T2 作为一个值。

# 结论

由于有了`ignite-spring-data-2.2-ext`依赖关系，将 Apache Ignite 连接到 Spring Boot 项目变得非常简单，它允许我们使用带有[派生查询方法](https://spring.getdocs.org/en-US/spring-data-docs/spring-data-jpa/reference/jpa.repositories/jpa.query-methods.html)和 [@Query annotations](https://javarevisited.blogspot.com/2021/09/spring-data-jpa-query-example-tutorial.html) 的标准 Spring 数据仓库进行更复杂的查询。

之后，您不仅可以使用 Apache Ignite 存储会话，还可以将其作为应用程序缓存或主数据库(Apache Ignite 从 2.1 版开始支持[本机持久性](https://ignite.apache.org/docs/latest/persistence/native-persistence))。

你可以在 [GitHub](https://github.com/wirtsleg/spring-sessions-with-apache-ignite) 上找到完整的源代码。