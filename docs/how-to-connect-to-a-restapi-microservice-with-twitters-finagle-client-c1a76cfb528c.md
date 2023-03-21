# 如何使用 Twitter 的 Finagle 客户端连接到 RestAPI 微服务

> 原文：<https://medium.com/javarevisited/how-to-connect-to-a-restapi-microservice-with-twitters-finagle-client-c1a76cfb528c?source=collection_archive---------2----------------------->

以免使用 TDD 方法并开始连接到另一个 API。我们将探讨开发人员可能会遇到的一些挑战以及如何解决它们。我们希望用 [Finagle](https://twitter.github.io/finagle/) 创建一个可扩展且易于测试的实现。

*Finagle 是一个用于 JVM 的可扩展 RPC 系统，用于构建高并发服务器。Finagle 为几种协议实现了统一的客户端和服务器 API，并且是为高性能和并发性而设计的。Finagle 的大部分代码与协议无关，简化了实现* …