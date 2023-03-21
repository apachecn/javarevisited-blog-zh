# 进入云原生 Dojo:黑带级调试

> 原文：<https://medium.com/javarevisited/enter-the-cloud-native-dojo-blackbelt-level-debugging-46938f387db0?source=collection_archive---------4----------------------->

![](img/8fd80bce75eb6a814e89d83230ea46a8.png)

调试通常被视为一种艺术形式或手艺。这适用于大多数工程相关的故障排除过程(例如，摩托车维修技术)。我们通常被一个高级开发人员灌输基本的步骤，然后被扔进众所周知的池子里。

因此，即使是高级工程师，在调试技能上有时也会有差距。很少有关于这个主题的大学课程或书籍，所以真的很难责怪他们。

在他的书《为什么程序会失败——系统调试指南》中，Andreas Zeller 讲述了他年轻时在电脑商店工作的一个故事。一位顾客带着一台新的 Commodore 64 电脑走进商店。对于上下文:那时的计算机直接引导到一个 basic 解释器；basic 接受行号作为第一个参数。他尝试输入这个有效的基本行:

```
10 print “Hello World”
```

他发现了一个语法错误。他很惊讶，因为这个程序看起来是正确的，而且并不复杂。不了解任何基础的你大概也能看懂…

一般来说，在[调试](https://javarevisited.blogspot.com/2011/07/java-debugging-tutorial-example-tips.html#axzz6bYzaddcE)和[编程](/javarevisited/8-advanced-python-programming-courses-for-intermediate-programmer-cc3bd47a4d19)中，我们需要将问题分解成更小的部分。所以他输入了:

```
10
```

空洞的陈述。

原来，用户已经习惯了用打字机打小写的 L 和字母 O 来打数字 1 和 0。他在电脑上遵循同样的做法，只是输入“lo”

当我读到那个故事时，我笑出声来，但我也想:这不是一个关于调试的故事。但它是，[调试](https://www.java67.com/2018/01/how-to-remote-debug-java-application-in-Eclipse.html)是关于意料之外的。这是关于缩小(或切片)问题，直到我们有一个我们可以观察到的问题。此时，解决方案呈现在我们面前。

在本文中，我想回顾一下我们在调试现代应用程序时面临的三大挑战:

1.  多语言调试
2.  调试不可再现的
3.  数据污染

# 多语言调试

这不是一个新问题。作为一个曾经以构建 JVM 为生的人，我偶尔会“元调试”:调试对 [JVM](/javarevisited/7-best-courses-to-learn-jvm-garbage-collection-and-performance-tuning-for-experienced-java-331705180686) 的调试支持。这让我在 Java 和本机代码之间来回切换，两个调试器都在运行和调试。

在构建低级虚拟机代码时，这是意料之中的。但这是一种越来越普遍的现象。一个服务器可能是用 [Python](/javarevisited/best-python-books-a93d1a0d842d) 或者 [Java](/javarevisited/6-best-object-oriented-programming-books-and-courses-for-beginners-d46235cbda49) 编写的，带有 [JavaScript](/javarevisited/5-free-books-to-learn-javascript-for-beginners-4cca79834262) 前端。我们可以通过前端调试器一直跟踪到后端。

类似地，在微服务部署中，每个服务可能用不同的语言实现。理论上，我们可以孤立地测试一切。实际上，这是不现实的。bug 时有发生。根据他们的定义，他们是意想不到的。

在无服务器的情况下，这个问题变得更加严重。在本地复制一个无服务器环境是如此具有挑战性，以至于我们听到这样的争论:本地调试无服务器是一种反模式。

[远程调试是有问题的，有风险的，](https://talktotheduck.dev/psa-the-risks-of-remote-jdwp-debugging)而且它不能扩展到复杂的部署。所以许多开发人员将自己局限于日志记录和一些可观察性工具。

虽然这有助于解决一些问题，但是对于本地[调试](https://javarevisited.blogspot.com/2011/02/how-to-setup-remote-debugging-in.html)来说，这是很差的替代品。持续可观察性工具为我们提供了一种超越简单监控的方法。我们可以在生产服务器上进行类似于传统调试器的源代码级调试。

# 调试不可再现的

有两种不可复制的 bug:一种是我们无法在本地复制的，另一种是我们根本无法复制的。如果我们可以在生产中重现该问题，我们可以使用持续可观察性工具来检查服务器。

然而，如果我们不能，我们实际上就陷入了日志和可观察性分析。我们最终会像警方犯罪现场调查员一样查看法医信息。此时，做某事有点晚了，所以我们需要确保在“下一次”发生这种情况时有新的日志。

作为开发人员，我们需要与用“不能复制”来关闭 bug 的文化作斗争。这是一种逃避。无法重现的场景应该添加日志或类似的保护措施来验证开发人员的假设。那样的话，我们就不会再被无法繁殖的难题困扰了。

# 数据污染

我们通常认为错误是失败、崩溃和停机。虽然这些确实不好，但它们通常是最好的 bug。我们知道有问题，解决方案通常是显而易见的和直接的。

数据污染是阴险的。调试非常困难，事后修复也非常困难，因为仅仅修复代码是不够的。

那么什么是数据污染漏洞呢？

这是一个导致错误数据的错误。这本身不是一个大问题…问题是这些数据可能会在微服务之间传播并进入数据库。在这一点上，它成为一个巨大的问题。

坏数据是一个问题，但是导致它的错误可能在任何地方，甚至在不同的服务器上。这就像大海捞针。这些 bug 特别阴险，因为它们通常只出现在生产中，而之后的清理工作可能比问题本身更糟糕。

一个很好的例子是“undefined”，它从[错误的 JavaScript 代码](/javarevisited/7-best-courses-to-learn-refactoring-and-clean-coding-in-java-47bea3c67006)开始传播，并以某种方式蔓延到数据库中，从而污染了所有地方的数据库。事后调试的方法通常是在写入或发送数据的地方放置一个堆栈日志。在那里使用一个条件来验证这确实是无效数据，并检测违规情况。

这可以通过代码来完成，也可以通过持续的可观察性工具来完成，比如 [Lightrun](https://lightrun.com/) 。

# TL；速度三角形定位法(dead reckoning)

调试是我们日常使用但仍然没有投入足够时间磨练的技能。我们最终会一遍又一遍地使用相同的工具和技术。我们退回到使用日志，而不使用已经存在多年的复杂功能。

不幸的是，虫子并不是静止不动的。当我们用惊人的容器技术扩展我们的基础设施时，我们的分布式解决方案的缺陷的规模。它们在规模上变得更加阴险。

我们需要新的工具和新的技术来处理 bug 可伸缩性，就像我们处理容器伸缩一样。

最初发表于[的新栈](https://thenewstack.io/enter-the-cloud-native-dojo-blackbelt-level-debugging/)。