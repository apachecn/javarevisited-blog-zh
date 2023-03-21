# 用 Java 和 SpringBoot 简单实现微服务和注册

> 原文：<https://medium.com/javarevisited/simple-implementation-of-microservice-and-registry-using-java-and-springboot-63b705f5d220?source=collection_archive---------1----------------------->

![](img/6c9b993ec24d16ea31a1e046a14a36b2.png)

Ravi Palwe 在 [Unsplash](https://unsplash.com/s/photos/lego?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

我最近[发表了一篇文章](https://levelup.gitconnected.com/simple-implementation-of-eureka-server-with-springboot-780b64e19735)，其中我们讨论了客户端发现机制的重要性以及如何使用网飞尤里卡来实现它。

一旦 eureka 服务器启动并运行，我们就可以根据需求构建并运行其他微服务，服务会自动向 eureka 注册中心注册。