# Spring AOP:使用切入点表达式——基于类级注释

> 原文：<https://medium.com/javarevisited/spring-aop-use-of-pointcut-expression-based-on-class-level-annotation-a4dfb224e9c1?source=collection_archive---------1----------------------->

## 处理交叉问题

![](img/9aaf4346bf2c58a0df83668acdb0c5ce.png)

照片由[泰勒·维克](https://unsplash.com/@tvick?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## **简介**

在本文中，我们将创建一个简单的性能记录器方面。使用 logger-aspect，我们将截取用特定注释标记的类中的每个公共方法。

## **场景**

首先，让我们看一个带有创建和更新方法的`SimpleService`:

对于延迟的使用，为了简单起见，让我们假设这两种方法都有正在进行的过程。

也就是说，我们的目标是记录每个方法的性能，同时保持类的高内聚和低耦合。

## **接近**

还有其他方法来处理所述场景；在我们的例子中，假设我们想要方便地注释一个类，并期望该类中的所有[公共方法](http://javarevisited.blogspot.sg/2012/12/getter-and-setter-method-vs-public-modifier-field-java.html#axzz55oDxm8vv)都有一个性能日志。现在让我们详细看看这个方法。

首先，让我们创建一个名为`PerformanceLogger`的定制注释:

然后，让我们创建一个`PerformanceLoggerAspect`，连接点匹配查找用`PerformanceLogger`注释的类:

最后，让我们用`PerformanceLogger`来注释`SimpleService`类:

## **结果**

现在，让我们创建一些测试:

准备就绪后，让我们运行测试:

```
2021-04-03 17:39:13.609  INFO 4232 --- [           main] c.emyasa.aspect.PerformanceLoggerAspect  : create method took 3025 ms
2021-04-03 17:39:15.624  INFO 4232 --- [           main] c.emyasa.aspect.PerformanceLoggerAspect  : update method took 2004 ms
```

从日志中，我们可以验证应用程序在每个方法上调用了 [performance](https://javarevisited.blogspot.com/2019/04/top-5-courses-to-learn-jvm-internals.html) logger 方面，假设一个类有一个`PerformanceLogger`注释。

## **结论**

在本文中，我们使用了一个切入点表达式，根据一个定制的注释来匹配连接点，从而创建一个简单的性能记录器方面。我们还创建了集成测试来验证`PerformanceLoggerAspect`确实在工作。

另一方面，我们的方法绝不仅仅局限于日志记录；这也适用于安全性、验证或其他横切关注点。

和往常一样，文章的源代码可以在 [GitHub](https://github.com/emyasa/medium-articles/tree/master/core-concepts) 上找到。

如果您正在寻找更多与 Java 相关的有趣文章，请随意查看:

[](/javarevisited/spring-jpa-when-to-use-join-fetch-a6cec898c4c6) [## Spring JPA:何时使用“Join Fetch”

### 避免 N+1 次查询并保留检索逻辑

medium.com](/javarevisited/spring-jpa-when-to-use-join-fetch-a6cec898c4c6) [](/javarevisited/jpa-specification-a-generic-search-e8695b1d19ec) [## JPA 规范:通用搜索

### 了解如何创建简单而通用的搜索

medium.com](/javarevisited/jpa-specification-a-generic-search-e8695b1d19ec)