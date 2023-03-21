# 什么是 CQRS？在微服务中有多重要？

> 原文：<https://medium.com/javarevisited/what-is-cqrs-how-important-is-it-in-microservices-20e65dc17c25?source=collection_archive---------0----------------------->

## 这是一个做出重大承诺的重要模式

[![](img/c2e79f29776ac3852a5d6061454fa6b0.png)](https://javarevisited.blogspot.com/2021/09/microservices-design-patterns-principles.html)

照片由 [Unsplash](https://unsplash.com/s/photos/database?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 sanga Rima Roman Selia 拍摄

CQRS 是一种[微服务架构模式](https://javarevisited.blogspot.com/2021/09/microservices-design-patterns-principles.html)，它代表命令和查询责任分离。

这种模式背后的基本思想是将写操作与读操作分开。不是使用一个数据存储来执行 CRUD 操作，而是…