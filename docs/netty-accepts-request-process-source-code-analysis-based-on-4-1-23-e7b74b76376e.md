# Netty 接受请求流程源代码分析(基于 4.1.23)

> 原文：<https://medium.com/javarevisited/netty-accepts-request-process-source-code-analysis-based-on-4-1-23-e7b74b76376e?source=collection_archive---------6----------------------->

## 启动后，Netty 如何接受客户端请求

![](img/2beb0ac7a4bdb73b2e26c0b844be82c9.png)

亚宁·迪亚兹在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**前言**

在上一篇文章中，我们分析了服务器是如何启动的。服务器启动后，必须接受客户端的请求，并返回客户端想要的信息。否则，您希望您的服务器做什么…