# 防止不稳定的测试

> 原文：<https://medium.com/javarevisited/preventing-flaky-tests-3a084ec5aa6e?source=collection_archive---------1----------------------->

![](img/368b1e3160f33f33ca256ca3d4cff14c.png)

[国立癌症研究所](https://unsplash.com/@nci?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当一个项目有一些测试成功通过或失败时，许多人都熟悉这种情况。这样的测试被称为易变的，在本文中，我们将讨论如何避免创建这样的测试。

我将使用 Java [Spring 框架](https://spring.io/?ref=hackernoon.com)作为例子，但是这里讨论的原因适用于任何环境。

# 1.不稳定的环境

易变测试的最常见原因是不稳定的环境。
例如，测试使用一些为所有测试部署的公共数据库。当在 [CI/CD 管道](/javarevisited/7-best-courses-to-learn-jenkins-and-ci-cd-for-devops-engineers-and-software-developers-df2de8fe38f3)中并行运行多个构建任务时，测试会修改彼此的数据。

在这种情况下，最可靠的解决方案是隔离环境。例如，您可以在 docker 容器中运行数据库(在 Java 中，有一个流行的库— [Testcontainers](https://www.testcontainers.org/?ref=hackernoon.com) )。因此，一个测试运行将独占使用它的数据库，在测试运行之后，它将终止它。

同样重要的是，不要忘记在每次测试后清理状态，以免影响测试套件中的后续测试。

# 2.调度程序和延迟操作

更罕见的原因是使用调度程序或延迟操作。
假设在应用程序的某个部分，我们声明了一个调度程序:

然后，在测试运行期间，它意外地工作了，因为测试正好在那个时间运行。编写测试的开发人员没有想到会有这样的副作用。

为了避免这种行为，您可以使时间表可配置。这将使应用程序更容易维护(无需重新构建代码来更改时间表)。它还将允许您在测试中覆盖调度，以便调度器只能手动运行。

然后在`application.yml`中进行测试:

如果您的应用程序使用延迟操作，您需要更加小心。一个动作可以在一个测试中注册，执行本身可以在另一个测试运行时发生。

# 3.使用睡眠

使用`[Thread.sleep(..)](https://javarevisited.blogspot.com/2011/12/difference-between-wait-sleep-yield.html)`来等待某个动作完成，这表明测试有问题。

由于某种原因(例如 GC 暂停)，超时可能不够。如果我们将超时设置为一个较大的裕量，这将显著降低测试运行的速度。

代替`sleep`，我们可以在异步操作中返回`Future`，而[使用`Future.get(20, TimeUnit.SECONDS)`等待](https://javarevisited.blogspot.com/2015/07/how-to-use-wait-notify-and-notifyall-in.html)，超时很大。因此，整个测试执行甚至会减少。

# 4.上下文开关程序

如果您使用的是 Spring，那么在缓存的上下文之间切换可能会导致不稳定的测试。这种行为很难调试，在添加另一个测试类之后，它可能随时发生。

例如，如果套件中的一个测试类使用了`@MockBean`，而另一个没有，那么将会创建两个不同的上下文。而 [JUnit](/javarevisited/5-courses-to-learn-junit-and-mockito-in-2019-best-of-lot-f217d8b93688) 引擎会以某种方式在它们之间切换。

我只知道一种保证避免它的方法——将测试类合并到测试套件中，这样同一套件中的所有测试都使用相同的应用程序上下文。

# 结论

古怪的测试会严重破坏开发应用程序时的体验。一些公司建立了特殊的服务来检测不可靠的测试，并且只重新运行它们。

当然，我的列表是不完整的，所以如果你在评论中写下你不得不处理的原因，我会很高兴。