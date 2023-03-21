# 用 Spring Boot 缓存您的 CompletableFuture 方法

> 原文：<https://medium.com/javarevisited/cache-your-completablefuture-methods-with-spring-boot-3af11a434ca4?source=collection_archive---------1----------------------->

## 调用另一个(外部)服务/系统有它自己的成本，你可以在你的应用程序中使用一个缓存层来降低它。我不会详细讲述如何与 Spring Boot 合作，因为你已经可以找到很多例子[这里](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-caching)、[这里](https://www.baeldung.com/spring-cache-tutorial)或者[这里](https://spring.io/guides/gs/caching/)。

> [这种](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#cache)方法只适用于保证给定输入(或参数)返回相同输出(结果)的方法