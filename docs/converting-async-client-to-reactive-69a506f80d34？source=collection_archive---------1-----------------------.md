# 将异步客户端转换为被动客户端

> 原文：<https://medium.com/javarevisited/converting-async-client-to-reactive-69a506f80d34?source=collection_archive---------1----------------------->

![](img/899adef0b2781964fe0d32db4dac30e5.png)

照片由[Tine ivani](https://unsplash.com/@tine999?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

在过去的几年里，[反应式编程](https://en.wikipedia.org/wiki/Reactive_programming)越来越流行。这有几个原因。

最重要的是，反应式编程使用非阻塞技术来有效利用 CPU。因此，同一组服务器可以处理高得多的负载。此外，声明性范式使得反应式编程易于维护，不像异步编程那样有回调地狱。

# 项目设置

Java 世界中有几个反应式库。本文中的例子将使用[项目反应器](https://projectreactor.io/),因为它与 Spring 框架有很好的集成。

要使用 Project Reactor，让我们添加 [reactive MVC](/javarevisited/7-best-webflux-and-reactive-spring-boot-courses-for-java-programmers-33b7c6fa8995) 依赖项，这样现在我们将使用 Netty server 而不是 Tomcat。

# 高端客户

我将用 [Elasticsearch](https://www.elastic.co/) 作为例子。Elasticsearch 是一个分布式数据存储，它提供了一个带有 HTTP web 界面的全文搜索引擎。

它有`[HighLevelRestClient](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/master/java-rest-high.html)`，是一个支持异步调用的 Java 客户端。在本文中，我们将围绕这个支持反应式接口的客户机编写一个包装器。

让我们定义所需的接口:

`[Mono](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html)`是 0 或 1 元素的 API。你可以在这里了解更多关于项目反应堆[的信息。](https://projectreactor.io/learn)

# 反应式实施

现在让我们实现这个客户端。例如,`index`方法如下:

每种方法的回调侦听器是不同的，所以让我们分别定义它们。

`[Sink](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Sinks.html)`允许以编程方式推送无功流信号。

我们可以用同样的方式实现其他方法，比如`search`。

# 被动客户端使用

然后我们只需要创建这个客户端的 bean:

我们可以用它来写一个知识库。

所以现在我们可以使用反应式声明管道的所有优点与 ElasticSearch 进行交互。

你可以在 [GitHub](https://github.com/wirtsleg/async-to-reactive) 上找到完整的源代码。