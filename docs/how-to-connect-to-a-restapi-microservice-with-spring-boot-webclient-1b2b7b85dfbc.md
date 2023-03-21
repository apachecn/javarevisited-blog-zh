# 如何使用 Spring Boot 网络客户端连接到 RestAPI 微服务

> 原文：<https://medium.com/javarevisited/how-to-connect-to-a-restapi-microservice-with-spring-boot-webclient-1b2b7b85dfbc?source=collection_archive---------0----------------------->

在我之前的[文章](/javarevisited/how-to-connect-to-a-restapi-microservice-with-twitters-finagle-client-c1a76cfb528c)中，我们使用 Finagle 连接到一个远程 REST API，并创建了一个 *CompletableFuture* 链来获取账户余额和交易。我们现在用 [WebClient](https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/boot-features-webclient.html) 使用相同的用例来看看两个选项之间的区别。

[![](img/2a6ba39a74b1487b7ae36891a826d128.png)](https://javarevisited.blogspot.com/2020/05/top-20-spring-boot-interview-questions-answers.html)

我们可以用下面的 Java Config 类创建一个带有可配置超时的 web client bean。