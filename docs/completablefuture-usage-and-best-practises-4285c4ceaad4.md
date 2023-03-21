# Java 简化版 CompletableFuture

> 原文：<https://medium.com/javarevisited/completablefuture-usage-and-best-practises-4285c4ceaad4?source=collection_archive---------0----------------------->

![](img/3565c52094f85ed614bce74be4d8d72f.png)

照片由[福蒂斯·福托普洛斯](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 1.介绍

随着 [Java 8](/javarevisited/top-5-courses-to-learn-new-features-of-java-8-to-java-13-107eb51d2a13) 中`CompletableFuture` 的引入，异步编程的世界向前迈进了一大步。但是当我们已经有了**未来**接口自 [Java 5](https://www.java67.com/2020/04/top-5-advanced-courses-to-learn-java-perofrmance-concurrency-memory-management.html) 以来，人们可能会想知道引入它背后的真正原因。CompletableFuture 背后的真正动机是克服 **Future** 接口的某些限制: