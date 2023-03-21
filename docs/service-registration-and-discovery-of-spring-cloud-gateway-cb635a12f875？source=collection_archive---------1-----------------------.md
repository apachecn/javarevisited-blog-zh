# Spring 云网关的服务注册和发现

> 原文：<https://medium.com/javarevisited/service-registration-and-discovery-of-spring-cloud-gateway-cb635a12f875?source=collection_archive---------1----------------------->

![](img/9dd7e63a9744026f66e1847580798678.png)

在上一篇文章中，我介绍了 Spring Cloud Gateway 的 Predict(断言)和 Filter(过滤器)。大家对春云网关有了初步的了解。在服务路由转发部分，前一篇文章是硬编码的。路由转发。本文解释了 Spring Cloud Gateway 如何与服务注册中心协作，在