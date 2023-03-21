# 如何对您的应用程序运行性能测试

> 原文：<https://medium.com/javarevisited/how-to-run-a-performance-test-on-your-application-42551fb62d38?source=collection_archive---------1----------------------->

模拟用户行为，以更好地了解您的系统如何应对更高的负载。

在本文中，我们将使用 *docker-compose* 设置一个简单的本地环境。最好使用专用的远程环境(开发/测试/验收),在那里验证应用程序的性能。如果这对于您和您的应用程序不可用，您可能会发现这种本地方法有许多优点。我们将把 java 应用程序作为本地集群中的一个容器来运行，但是这个配置对于任何应用程序都是有效的…