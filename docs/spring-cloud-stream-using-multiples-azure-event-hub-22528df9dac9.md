# 使用多个 Azure 事件中心的 Spring Cloud 流

> 原文：<https://medium.com/javarevisited/spring-cloud-stream-using-multiples-azure-event-hub-22528df9dac9?source=collection_archive---------0----------------------->

# TL；速度三角形定位法(dead reckoning)

最近在我参与的一个项目中，我们选择使用 Spring Cloud Stream 作为你的消息框架，Azure Event Hub 作为你的消息代理。但是在 ***标准*** 配置中的 Azure Event Hub 每个名称空间有 10 个主题的限制，而我们的应用程序需要 40 个主题。将 Azure Event Hub 升级到一个 ***专用*** 版本的成本非常昂贵，所以我们决定并行使用 4 个 Azure Event Hub ***标准*** 的实例。使用 Spring Cloud Stream 可以轻松地进行这种配置，我将展示如何在…