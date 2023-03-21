# 在最后一天进行远程调试—构建 DDTJ 第 10 天

> 原文：<https://medium.com/javarevisited/remote-debugging-on-the-last-day-of-building-ddtj-day-10-b829a3c946cc?source=collection_archive---------2----------------------->

![](img/0d5812a8312ff4e73195512c4456060b.png)

昨天 [DDT PoC 开始工作](https://dev.to/codenameone/first-mocked-unit-test-generated-by-ddtj-building-ddtj-day-9-36lo)，今天是我在 DDTJ 上为期两周的“黑客节”的最后一天…这就是我们的现状。

第二周终于结束了，我建立了一个 DDT 概念的工作证明。我没能完成一个完整的 MVP。不同的是，概念证明显示了一些工作，MVP 需要做一些有用的事情。滴滴涕还没有真正发挥作用。

让滴滴涕变得有用是我现在的主要目标。为了方便起见，我正试图让它与 Spring Boot 的一个应用程序一起工作。到目前为止，这不是很好，但我正在取得迭代进展。像运行“hello world”那样运行应用程序是行不通的。后端被阻塞，启动方法永远不会返回。

[Spring Boot](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e?source=---------39------------------) 有一个相对复杂的引导加载阶段，这会给调试器带来问题。方法是绑定 DDTJ 不支持的远程调试器，所以我添加了这种支持。

通过远程调试和 spring boot 的一些命令行技巧，我能够连接到 spring boot VM 并让整个系统正常工作。

# 奇怪的虫子

现在我正处于消灭虫子的阶段。用[远程调试](https://javarevisited.blogspot.com/2011/02/how-to-setup-remote-debugging-in.html)运行春开机宠物诊所 app。启动后端。把它们连接起来，找到一个 bug。修复，冲洗，重复。

这是乏味的，但大多运行顺利。

不幸的是，我似乎遇到了很难调试的问题。例如由 JDI 实现抛出的`Unexpected JDWP Error: 32`。这似乎是堆栈帧的问题，但我不知道为什么会发生这种情况。

另一个问题是，在方法入口事件中，堆栈似乎偶尔会“损坏”。当前方法不在栈顶。据我所知，这不应该发生。但是我可能忽略了文档的细微差别。

# 附加调试器

我们可以使用进程 id 和如下命令来连接调试器:

```
java -jar target/CLI-0.0.6-SNAPSHOT-shaded.jar -pid=62604
```

或者我们可以使用插座。注意套接字总是连接到本地主机，因为[远程调试](https://www.java67.com/2018/01/how-to-remote-debug-java-application-in-Eclipse.html)对 jdwp 来说是危险的:

```
java -jar target/CLI-0.0.6-SNAPSHOT-shaded.jar -attach=8000
```

请注意，后端应该已经运行之前和宠物诊所演示。我对宠物诊所使用了以下命令:

```
java -Xdebug -Xnoagent -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8000 -jar spring-petclinic-2.5.0-SNAPSHOT.jar -Dagentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8000
```

这可能有点多，但我花了很长时间才把这该死的东西通过 JDWP 连接起来。

# 即将到来的公关和我们下一步该何去何从？

这段代码在一个仍然没有运行 spring boot 宠物诊所的 PR 中。因此，我还不想承诺或创造一个公关。不幸的是，两周的假期结束了，我有其他的职责。

我会降低我在滴滴涕上的工作进度，因为我在工作中有很多事情要做。但是我计划在 2-4 周内让它成为 MVP。我认为较慢的节奏可能有助于我消化这些问题。所以总的来说，稍微休息一下可能是件好事。

在过去的两周里，我犯了很多错误，但我想我现在离错误太近了。下周我会试着写一篇事后总结，涵盖我做得好的地方和我可以改进的地方。这不会是一个总结，因为我认为随着我重新思考我的一些立场，我对这个问题的看法将会进一步发展。

我建议看一下 ddtj 项目，看看它什么时候会有新的 PR 更新。你也可以[在 twitter](https://twitter.com/debugagent) 上关注我，了解最新的发展动态。我对 DDT 的核心技术有一些很好的想法，远远超出了测试本身。这里有很多未开发的潜力需要我们去探索。