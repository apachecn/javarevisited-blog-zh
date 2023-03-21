# 事件驱动的 Java 服务中的上下文日志

> 原文：<https://medium.com/javarevisited/contextual-logging-in-event-driven-java-services-e6ffb676791?source=collection_archive---------3----------------------->

## 远离基于线程的上下文

![](img/50421b18cea977c84219947922bfac52.png)

图片由来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2171410) 的 [Jeon Sang-O](https://pixabay.com/users/jeonsango-1594796/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2171410) 提供

微服务倾向于做一件事情，并且依赖于其他微服务的帮助来执行完整的事务。当操作在服务中来回切换时，拥有一个跟踪 ID 会有所帮助，这样您就可以知道是谁发出了请求。这样你就可以完成整个交易，即使它可能…