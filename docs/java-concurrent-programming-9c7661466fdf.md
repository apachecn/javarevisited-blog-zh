# [Java]并发编程

> 原文：<https://medium.com/javarevisited/java-concurrent-programming-9c7661466fdf?source=collection_archive---------4----------------------->

![](img/23a34dba21c94a03886a8626548aec1b.png)

> 用户理所当然地认为他们的系统可以同时做多件事情。他们认为自己可以继续在文字处理器中工作，而其他应用程序可以下载文件、管理打印队列和传输音频。…能做这些事情的软件被称为*并发软件*
> 
> —[Java 教程](https://docs.oracle.com/javase/tutorial/essential/concurrency/)

# 什么是并发软件？

关于并发编程，我找不到比 Java 教程中写的更好的定义了。它被称为“[多个操作序列在重叠的时间段内运行](https://www.toptal.com/software/introduction-to-concurrent-programming)”。Java 用`Thread`类支持并发。

# 什么是线程？

**线程是程序中执行的线程。**JVM 允许应用程序同时运行多个执行线程。为了深入理解 Java 提供的多线程环境，您需要知道线程之间是如何交互的。

每个线程都有一个优先级。优先级较高的线程优先于优先级较低的线程执行。每个线程可能会也可能不会被标记为**守护进程。**

[守护线程](http://www.java67.com/2016/06/difference-between-daemon-vs-user-thread-in-java.html)是一个低优先级线程，在后台运行，执行垃圾收集等任务。当在某个线程中运行的代码创建一个新的[线程对象](https://javarevisited.blogspot.com/2020/04/how-to-use-exchanger-in-java-with-example.html#axzz6dqIph09n)时，该新线程的优先级最初被设置为等于创建线程的优先级，并且当且仅当创建线程是守护线程时，该新线程才是[守护线程。](https://javarevisited.blogspot.com/2012/03/what-is-daemon-thread-in-java-and.html)

# 线程是如何创建的？

创建 Thread 实例的应用程序必须提供将在该线程中运行的代码。有两种方法可以做到这一点:

一种方法是创建一个扩展`Thread`的子类，并覆盖它的`run`方法。或者，你可以直接提供一个[可运行的](https://javarevisited.blogspot.com/2016/08/useful-difference-between-callable-and-Runnable-in-Java.html#axzz6e8hmwujv)对象作为参数。`Runnable`接口定义了一个方法`[run](https://javarevisited.blogspot.com/2012/03/difference-between-start-and-run-method.html#axzz6vPUwyVzv)` <https://javarevisited.blogspot.com/2012/03/difference-between-start-and-run-method.html#axzz6vPUwyVzv>，其中包含了执行代码。

`Runnable`对象被传递给`Thread`构造函数，如示例所示。哪个更好看？根据 [ORACLE](https://docs.oracle.com/javase/tutorial/essential/concurrency/runthread.html) 文档，使用 Runnable 对象更加通用，因为您可以扩展除 Thread 之外的类。

# 线程的操作

## 休眠时暂停执行

`Thread.sleep()`使当前线程暂停执行一段时间。当正在运行的方法被中断请求终止时，线程抛出`InterruptedException`，这通常发生在一个线程中断另一个活动线程时。

## 用中断中断执行

`Thread.interrupt()`与`Thread.sleep`不同，导致当前线程[停止执行](https://javarevisited.blogspot.com/2011/10/how-to-stop-thread-java-example.html)并做其他事情。线程通过调用线程对象上的中断来发送中断，以使线程被中断。

## 等待另一个线程加入

`[join](https://javarevisited.blogspot.com/2013/02/how-to-join-multiple-threads-in-java-example-tutorial.html#axzz6pTHjcWBu)`方法允许一个线程等待另一个线程完成它的操作。如果 newThread 是当前正在工作的线程，`newThread.join`使执行加入主线程。然后，主线程暂停其操作，直到新线程终止。

# 结果

这个`Thread`对象是用 [Java](https://javarevisited.blogspot.com/2016/06/5-books-to-learn-concurrent-programming-multithreading-java.html) 设计的，目的是让开发人员根据需要处理任意多的操作。因此，由它们来决定创建多少线程，响应中断，并在指定的时间内交互工作。

然而，随着线程数量的增加，相应地管理它们变得棘手，这需要另一个对象将它们集成在一起。在[的下一篇文章](/ryanjang-devnotes/java-managing-concurrency-467993a01e6f)中，我们将讨论对象。

感谢你阅读这篇文章。

# 参考

    <https://www.toptal.com/software/introduction-to-concurrent-programming> 