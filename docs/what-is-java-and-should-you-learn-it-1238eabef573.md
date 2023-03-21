# Java 是什么，该不该学？

> 原文：<https://medium.com/javarevisited/what-is-java-and-should-you-learn-it-1238eabef573?source=collection_archive---------2----------------------->

![](img/4b69c336f51bd7494e0c4b5d53466d5b.png)

这个指南是给每个想学习第一门编程语言的人的。如果你是一个经验丰富的开发人员，你已经知道这些事情。初学者，继续读下去！

## Java 是什么？

Java 是一种编程语言。你用它来制作软件。以下是 Java 的主要特征:

*   它是面向对象的。为了利用它的力量，你需要理解面向对象编程的基础知识。*
*   它是多平台的。您可以在多个平台和设备上重用相同的代码。
*   有点慢而且罗嗦。不适合低级**和高负荷的任务。***

* [面向对象编程](/javarevisited/my-favorite-courses-to-learn-object-oriented-programming-and-design-in-2019-197bab351733?source=collection_home---4------0-----------------------)是指编程语言使用对象的思想来表示数据和方法。

* *低级意味着编程语言非常接近机器本身的语言，几乎不需要**抽象**。

* *高负载任务是那些使用大部分可用资源进行处理的任务。

## Java 的超能力是什么？

Java 的超级大国是 JVM — [Java 虚拟机](https://javarevisited.blogspot.com/2019/04/top-5-courses-to-learn-jvm-internals.html)。为了理解这个概念，我们需要后退一步。

假设您想编写一个消息应用程序。它需要与互联网通话，向屏幕输出一些文本，并在机器的硬盘上存储一些数据。你希望你的应用程序是多平台的，这样人们就可以在 MAC、PC 和其他设备上使用它。

但有一个问题:MAC 和 PC 处理网络、磁盘和用户输入的方式不同。你不能在两个不同的平台上使用相同的代码。你至少要为输入、输出、网络和用户相关的东西重写[代码](/javarevisited/clean-code-a-must-read-coding-book-for-programmers-9dc80494d27c)。而且那只是针对两个系统。如果你想让 messenger 应用程序在智能冰箱上运行呢？你想合作的平台越多，你要做的工作就越多。

输入 JVM。Java 虚拟机就像它听起来的那样:一个虚拟容器，它模仿一台计算机，具有标准的输入、输出、网络、磁盘、屏幕等等。在 [Java](/javarevisited/top-5-java-online-courses-for-beginners-best-of-lot-1e1e240a758) 中，你为那个虚拟计算机编写代码，然后 Java 编译器弥合虚拟机和实际硬件之间的差距。

使用 Java，你仍然需要为不同的平台编译你的代码。但是现在你可以在所有平台上使用相同的代码——你只需要改变编译环境。

> 假设你在一家拥有巨大仓库的批发公司工作。你得到一个任务，写一些软件来帮助仓库工人跟踪库存。你用 Java 编写代码，然后编译到员工随身携带的设备上——比如说，一个 Windows 操作系统的平板电脑。软件运行良好一年，你保持更新。然后该公司转向基于安卓系统的笔记本电脑。好的，没问题。你把你的软件编译到 Android 设备上，你就可以开始了。

## 那么，缺点是什么呢？

Java 没有 C++或 Swift 快。C 和 Swift 是非常高效的语言，因为它们与处理器和操作系统紧密相关。尽管与机器语言相比，这些语言相当高级，涉及相当多的抽象，但 [C++](/@javinpaul/top-10-courses-to-learn-c-for-beginners-best-and-free-4afc262a544e) 和 [Swift](/javarevisited/top-5-online-courses-to-learn-ios-12-swift-in-2019-a35ae1be7b2b?source=---------22------------------) 并没有做很多多余的工作。

因此，您将从基于 Swift 和 C 的程序中获得速度优势。但是你仍然不得不面对 C 和 Swift 中的代码因平台不同而不同的事实。

另一方面，Java 有它唯一的平台，它可以带着它到处走。Java 中的一切都必须首先与 JVM 对话，然后这个平台与计算机对话。这就是为什么 Java 应用程序没有人们预期的那么快。对于消费级桌面软件来说，这没什么大不了的，但是对于服务器解决方案和高负载任务来说，这种速度上的差异是至关重要的。

Java 相当罗嗦。事情就这么发生了。这里有一些用 [Python](/javarevisited/10-free-python-tutorials-and-courses-from-google-microsoft-and-coursera-for-beginners-96b9ad20b4e6) 编写的在屏幕上绘制一些符号的代码:

这是同样的事情，但是在 Java 中:

## Java——类似于 JavaScript 吗？

哦，不。那是完全不同的语言，由不同的团队为不同的任务开发的。名字的相似是由于 Java 在 20 世纪 90 年代中期的营销战和大肆宣传。

Java 是一种用于制作软件的编译语言。JavaScript 是一种用于构建动态网页的脚本语言，也就是说，网页在每次浏览时都会显示不同的内容。当然，你可以用 JavaScript 构建某种软件，但是 JS 的概念、结构和整体工作流程在很多方面与 Java 有本质的不同。

## 哪些人应该学习 Java？

Java 工作量很大。对孩子们来说，这不是那种迷人的时髦编程语言。

![](img/626b28deb11629104c70178c8204ca86.png)

以下是何时考虑学习 Java:

*   如果你想要一份长久的职业，维护全球的金融或工业基础设施。许多现实世界的东西都运行在 Java 上，从自动取款机和收银机到生产线。将这些系统过渡到其他技术需要几十年的时间，在这一点上，这种过渡似乎不太可能。
*   如果你喜欢 Android 生态系统。 [Java](/javarevisited/my-favorite-books-to-learn-java-in-depth-must-read-9c4468aeec99) 非常广泛的用于 [Android 开发](/hackernoon/top-5-courses-to-learn-android-for-java-programmers-667e03d995b4)。

然而，学习 Java 作为你的第一语言可能会很乏味。如果你想涉足编程领域，试着编写一些在你的浏览器中运行的 JavaScript，或者学习一些 Python。这两种语言适用于不同的任务，但都适合作为你的第一编程语言。

想了解更多编程知识？探索[实习](https://practicum.yandex.com/)的教育资源。我们提供在线教育，帮助您学习新的技术技能，提升您的职业生涯。