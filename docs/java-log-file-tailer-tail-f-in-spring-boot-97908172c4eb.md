# Spring Boot 的 Java 日志文件 tailer (tail -f)

> 原文：<https://medium.com/javarevisited/java-log-file-tailer-tail-f-in-spring-boot-97908172c4eb?source=collection_archive---------0----------------------->

在本文中，我们将重点关注 Spring Boot 端点上的流式文件更改。您可能使用了`tail -f`命令来跟踪文件中的变化。我们将从新的端点看到相同的输出。

最简单的方法是返回我们从端点文件中读取的内容。这个特性已经可以在 Spring Boot[级](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/actuate/logging/LogFileWebEndpoint.html)的基础上[在这里](https://docs.spring.io/spring-boot/docs/current/actuator-api/htmlsingle/#logfile)作动器。

![](img/f32863e77a1dcbc4aea21b156b35bbe9.png)