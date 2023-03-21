# 使用 Spring 和注入用户上下文的异步流程

> 原文：<https://medium.com/javarevisited/async-process-using-spring-and-injecting-user-context-6f1af16e9759?source=collection_archive---------0----------------------->

![](img/5e71b75b7a6a9c599c4e6a1d29ac2821.png)

## 为什么我们需要一个单独的帖子呢？

*   在 java 世界中，启动异步进程的一般做法是创建一个单独的线程或使用 Runnable 方法实现类。但是这种方法对 spring 不起作用，因为使用 spring 的全部思想是利用“依赖注入”的力量，所以如果我们创建一个…