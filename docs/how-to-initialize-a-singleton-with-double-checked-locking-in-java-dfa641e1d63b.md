# 如何在 Java 中初始化带有双重检查锁定的单例

> 原文：<https://medium.com/javarevisited/how-to-initialize-a-singleton-with-double-checked-locking-in-java-dfa641e1d63b?source=collection_archive---------3----------------------->

![](img/099dcf0774d650d40fbc2e159e3c966a.png)

Emile Perron 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我们都在努力理解 Java 中的[并发。即使您的代码在您的系统上看起来运行良好，但并发性错误是不请自来的，代码仍然可能在任何时候被破坏和失败。](/javarevisited/8-best-multithreading-and-concurrency-courses-for-experienced-java-developers-8acfd3b25094)

在这篇文章中，我们将看看在 Java 中初始化单例类的方法之一。单例应该由多个线程使用，并且只需创建一个实例。

让我们先来看看当它被一个单独的线程使用时是如何完成的。然后，我们可以找到问题并纠正它们，以便它可以在多线程环境中工作。

## 在单线程环境中初始化单例

这对于单线程应用程序来说非常好。但是，如果两个或更多的线程同时进入 if 块呢？在这种情况下，它们肯定会成功，因为“实例”尚未初始化。这导致了 Singleton 类的多个对象的创建。

解决此类并发问题[的最常见方法之一是使用 synchronized 关键字。如果任何代码被包装在一个同步的块中或者使用一个](https://www.java67.com/2012/12/producer-consumer-problem-with-wait-and-notify-example.html)[同步的方法](https://www.java67.com/2013/01/difference-between-synchronized-block-vs-method-java-example.html)，你可以限制只能访问一个线程。其他线程将不得不等待，直到当前线程退出同步块。

## 创建对象时使用同步

那么，如何将 [synchronized 关键字](https://javarevisited.blogspot.com/2020/04/difference-between-atomic-volatile-and-synchronized-in-java-multi-threading.html)应用到上面的代码中，以便当多个线程试图创建实例时，只允许一个线程访问 if 块内部。

其中一种方法是让整个方法同步。

这很好，但是从性能的角度来看，这是一个大禁忌。如果正确地创建了实例，我们实际上不需要限制对它的访问，因为 If 条件失败，实例被返回。

这里的解决方案是仅在创建实例时限制访问。您可能会考虑这样做:

仅仅在创建对象时锁定类是行不通的。假设线程 A 和 B 都进入了 if 块，假设线程 A 获得了一个锁并创建了一个新对象。退出后，线程 B 获取一个锁并创建另一个对象。

这就是[双重检查锁](https://javarevisited.blogspot.com/2014/05/double-checked-locking-on-singleton-in-java.html#axzz6j8KhisSX)发挥作用的地方。这个错误可以通过在 synchronized 块中放置另一个 if 条件来避免。因此，即使两个线程访问第一个 if 块，一个线程获得锁并创建对象，另一个线程仍然会检查对象是否被创建。

这就是双重检查锁定的实现方式:

这就是我们的代码最终的样子。

看起来很好，不是吗？但是等等。这段代码还有一个 bug。

假设一个线程创建了一个实例，另一个线程发现这个实例不为空并返回它。但是为什么这会产生问题呢？这与 [JVM](/javarevisited/7-best-courses-to-learn-jvm-garbage-collection-and-performance-tuning-for-experienced-java-331705180686?source=---------8------------------) 允许发布部分初始化的对象这一事实有关。线程 A 可能会为实例对象分配一些内存空间，而不会完全初始化它。线程 B 可能会看到实例不为空，并返回部分构造的实例，这可能会导致微妙的错误。解决方案是让实例变得易变。

这就是如何用[双重检查锁定](https://www.java67.com/2015/09/thread-safe-singleton-in-java-using-double-checked-locking-pattern.html)初始化单例: