# Java 微服务中的 CQRS

> 原文：<https://medium.com/javarevisited/cqrs-in-java-microservices-85ca7aa94d0e?source=collection_archive---------0----------------------->

## 使用事件来源实施 CQRS

![](img/e650934ca6a58d7a9601be703807545d.png)

图片由[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2926087)的 Gerd Altmann 提供

命令查询责任分离(CQRS)是服务架构中的一种模式。这是关注点的分离，也就是说，写的服务和读的服务的分离。为什么要分离读写服务？微服务的优势之一是能够独立扩展服务。我们经常可以用一些…