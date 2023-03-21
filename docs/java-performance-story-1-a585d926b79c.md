# Java 性能，故事 1

> 原文：<https://medium.com/javarevisited/java-performance-story-1-a585d926b79c?source=collection_archive---------1----------------------->

这是未来故事系列中关于如何编写高性能 Java 代码的介绍性故事。在这个故事中，给出了一个小而有用的提示。

[![](img/3af8a4882c27fb284e36929d2ebac543.png)](https://javarevisited.blogspot.com/2019/04/top-5-courses-to-learn-jvm-internals.html)

詹尼克·塞尔兹在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

编写高性能的代码并不容易。当谈到性能时，我们通常指的是高速。如果我们也使用小资源，那就完美了。

想象一下，在某个方法中，你对同一个对象调用了同一个方法更多次，例如 *person.getAge()* 在方法执行期间被调用了 3 次*。这个方法每次都返回相同的结果，因为一个人的年龄每年都会改变一次。:-)*

这是不必要的速度浪费！正确的方法是使用局部变量并调用 *person.getAge()* 一次:

*int personsAge = person . getage()；*

然后您可以使用 *personsAge 更改每个 *person.getAge()* 的用法。*如果你连续调用 *person.getAge()* 知道你总是得到相同的值，那么 [JVM](/javarevisited/7-best-courses-to-learn-jvm-garbage-collection-and-performance-tuning-for-experienced-java-331705180686) 必须总是停止当前方法，为下一个方法准备[栈](https://javarevisited.blogspot.com/2013/01/difference-between-stack-and-heap-java.html)(*person . getage()*)然后调用它，把结果整数值放入栈中，并返回，把整数放回表达式中。

本文的最后一个技巧:如果你重构类似的代码，请使用你的 [IDE](/javarevisited/7-best-courses-to-learn-intellij-idea-for-beginners-and-experienced-java-programmers-2e9aa9bb0c05) 工具！如果您手动更改每个事件，则可能会出现一些错误。尽可能聪明地使用你的工具！

## 结论

编写高性能代码一点也不容易！这只是漫长旅程的开始。我会分享我的经验，并帮助你也这样做。举例——也许它们只是简单的例子，但即便如此，你也很难在其他地方找到这样的提示。继续学习，再见！

</javarevisited/10-best-places-to-learn-java-online-for-free-ce5e713ab5b2>  </swlh/top-10-java-books-for-programmers-all-time-great-82b0ee0b831a> 