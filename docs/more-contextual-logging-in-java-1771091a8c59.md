# Java 中更多的上下文日志记录

> 原文：<https://medium.com/javarevisited/more-contextual-logging-in-java-1771091a8c59?source=collection_archive---------0----------------------->

## 利用 Spring Boot 侦探克服反应式 Java 的线程混战

![](img/3164b82e19720ecf0624228ea23284ce.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2634180) 的[香颂](https://pixabay.com/users/heungsoon-4523762/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2634180)

在[的上一篇文章](/javarevisited/contextual-logging-in-event-driven-java-services-e6ffb676791)中，我能够克服 reactive Java 在每个线程基础上维护上下文的问题。主要的问题是，被动的 Java 请求可以由不同的线程处理，所以我们不能依赖流行的 Java 日志记录的 [MDC](http://logback.qos.ch/manual/mdc.html) 工具。