# Project Loom 之后，请求线程模型是个好东西吗？

> 原文：<https://medium.com/javarevisited/is-the-thread-per-request-model-a-good-thing-after-project-loom-6d08012839e8?source=collection_archive---------1----------------------->

[![](img/5ca6fea2f294f655c0c029fe8b5da6bd.png)](https://javarevisited.blogspot.com/2014/07/top-50-java-multithreading-interview-questions-answers.html)

Java 服务器应用程序通常倾向于多线程。这种多线程特性允许 Java 应用程序同时为多个用户服务，而不是按顺序服务。

# 每请求线程模型

几年前，对于实现 web 服务器的[,首先，人们可能会考虑旋转新线程来处理新请求的可能性——每个请求一个线程的*模型。但是，硬件限制将只允许它们在 JVM 崩溃并出现*](https://javarevisited.blogspot.com/2015/06/how-to-create-http-server-in-java-serversocket-example.html) *[OutOfMemoryError](https://javarevisited.blogspot.com/2011/09/javalangoutofmemoryerror-permgen-space.html) 之前运行这么多线程。在这一点上，这种想法无疑是可笑的。此外，就线程带来的内存开销和创建线程所花费的时间(1 ms)而言，创建线程的成本很高。*

# 线程池

人们可能会进一步考虑将线程池化并使用这些[池化线程](https://javarevisited.blogspot.com/2013/07/how-to-create-thread-pools-in-java-executors-framework-example-tutorial.html)来服务请求的可能性。线程池似乎是一个足够好的选择，因为通常来说，将昂贵的资源放在一起是一个好主意。`[ExecutorService](https://javarevisited.blogspot.com/2017/02/difference-between-executor-executorservice-and-executors-in-java.html)`做同样的事情——它共享线程。正如我们之前所知道的，我们可以创建和共享多少线程是有限制的；线程多不代表性能好。

在他的书["*Java Concurrency in Practice*，"](/javarevisited/is-java-concurrency-in-practice-still-valid-8bb54fc3fb7f)中，Brian Goetz 给出了下面的公式来为给定的机器和应用程序找到理想的线程池大小。

```
**Number of threads = Number of Available Cores * (1 + Wait time / Service time)**
```

*等待时间*是应用程序等待通过网络访问的远程资源的时间，*服务时间*是 CPU 忙于计算结果的时间。以下是一台**双核**机器的理想线程池大小，该机器连接到一个微服务，该微服务在**25 毫秒**内做出响应，并花费**10 毫秒**的 CPU 时间来计算响应。

```
2 * (1 + (25 / 10)) = 7
```

这个应用程序的池中最好只有七个线程。[异步库和框架](/javarevisited/7-best-webflux-and-reactive-spring-boot-courses-for-java-programmers-33b7c6fa8995)可以通过以交错方式在不同线程中运行每个请求阶段来改善情况。JEP 425 评论说这种风格-

> 与 Java 平台不一致，因为应用程序的并发单元——异步管道——不再是平台的并发单元。

# 项目织机和虚拟线程

Project Loom 引入了名为**虚拟线程**的轻量级用户模式线程作为`[java.lang.Thread](https://www.java67.com/2013/01/difference-between-callable-and-runnable-java.html)`的实例。到目前为止，我们讨论的线程只是平台线程的一层薄薄的包装。平台线程很庞大，并且依赖于操作系统。

[Java](/javarevisited/10-best-places-to-learn-java-online-for-free-ce5e713ab5b2) 不自由改进它们，操作系统直接把这些线程分配给处理器。另一方面，JDK 的调度程序将虚拟线程分配给平台线程，操作系统照常将虚拟线程分配给处理器。

## 虚拟线程如何工作

我们都知道阻塞线程是邪恶的，会对应用程序的性能产生负面影响。好吧，在这种情况下不会。当一个虚拟线程在 I/O 上阻塞或者在 [JDK](https://javarevisited.blogspot.com/2015/01/what-is-rtjar-in-javajdkjre-why-its-important.html) 中的一些阻塞操作，比如`BlockingQueue.take()`，它会自动从平台线程中卸载。

JDK 的调度程序可以在这个现在空闲的平台线程上安装和运行其他虚拟线程。当阻塞操作准备完成时，它将虚拟线程提交回调度器，调度器将虚拟线程安装在可用的平台线程上以恢复执行。

此平台线程不必与卸载虚拟线程的平台线程相同。因此，我们现在可以构建具有高吞吐量的高并发应用程序，而无需消耗更多的线程(默认情况下，虚拟线程的[执行器](https://javarevisited.blogspot.com/2016/12/difference-between-thread-and-executor.html)将使用与可用处理器数量一样多的平台线程)。

## 带有虚拟线程的每请求线程模型？

不应该共享虚拟线程，因为它们不是昂贵的资源。人们可以创建数百万个来处理网络操作。它们应该按需旋转，并在任务完成后被杀死，因此适合短期任务。

虚拟线程的这些属性提供了接近最佳的 CPU 利用率，并在吞吐量而不是速度方面显著提高了性能。现在我们已经有了所有的支持数据，可以有把握地说，Java 服务器应用程序中的每个请求的虚拟线程模型比池化平台线程更安全、更高效。

关于虚拟线程的更多信息在下面的文章中。

1.  [我是如何在不停止 JVM 的情况下启动 500 万个虚拟线程的](/javarevisited/how-i-spun-up-5-million-virtual-threads-without-stalling-the-jvm-1188d806e6bd)。