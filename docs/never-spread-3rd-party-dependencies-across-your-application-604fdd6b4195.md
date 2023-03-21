# 不要在你的应用程序中散布第三方依赖

> 原文：<https://medium.com/javarevisited/never-spread-3rd-party-dependencies-across-your-application-604fdd6b4195?source=collection_archive---------2----------------------->

以正确的方式管理您的第三方依赖关系。将第三方依赖的代码与应用程序的其余部分隔离开来。编写可重用的代码。用适当的 JUnit 测试覆盖这个逻辑。

[![](img/7b414e7aee4a63e76f10abb907b60d25.png)](https://javarevisited.blogspot.com/2016/09/top-5-json-library-in-java-JEE.html)

索菲娅·列夫琴科在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当您依赖第三方时，您必须以正确的方式使用它们。典型的错误方法是在整个应用程序中使用第三方依赖的类。

虽然由于实施速度的原因，它在开始时可能看起来不错，但以后您将付出代价。随着世界上一切事物的变化，您对第三方的依赖也将发生变化，您当前的版本将会过时。那你应该把它换成新版本。变化足以迫使你重写一些代码。

那么你应该在你的应用程序中的许多地方这样做。当然，你可能会打碎东西。你将被迫做大量的回归测试。迁移到第三方依赖的新版本将是一个巨大的痛苦！

我来说说我自己经历的例子吧。在一家公司，我的任务是将 Apache [HTTP 客户端库](/javarevisited/20-essential-java-libraries-and-apis-every-programmer-should-learn-5ccd41812fc7)从版本 4 迁移到版本 5。他们使用这个库实现了对 20 多个不同 API 的 REST 调用。

每个 API 都有一堆不同的端点。总共有 100 多个不同的终点。不幸的是，他们没有为访问第三方 API 端点编写通用代码，但是每种方法都有自己的代码。

许多地方都有大量的复制粘贴代码。你可以想象这次迁移有多艰难！您应该在所有这些地方更改代码。因为 Apache HTTP 客户端库 4 和 5 之间存在差异，所以为了不破坏某些东西，每个更改都需要回归测试。

此外，主要是没有任何自动集成测试来使这种迁移更容易(为了不破坏 API 方之间的契约)。最后，这是一个非常耗时且容易出错的过程！

## 结论

将代码从第三方依赖中分离出来，放入自己的服务层。不要在你的应用程序中散布来自第三方依赖的代码。编写包含 JUnit 和集成测试的可重用代码。它最终会给你带来好处的！