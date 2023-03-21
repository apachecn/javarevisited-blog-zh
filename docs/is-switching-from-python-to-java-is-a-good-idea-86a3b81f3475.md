# 从 Python 转到 Java 是个好主意吗？

> 原文：<https://medium.com/javarevisited/is-switching-from-python-to-java-is-a-good-idea-86a3b81f3475?source=collection_archive---------0----------------------->

![](img/e58bcac7fd1fe4711b32a4d5b2e5fe84.png)

将 Python 作为第一编程语言的想法是有理性背景的。首先，Python 的语法简洁明了，对象和变量工作的底层模型完全一致。这意味着你可以毫不费力地写出“真正的”非常强大的应用程序。所以很多学校教学生用 Python 编程也没什么奇怪的。

然而，懂两门语言总比一门好。如果你想在 Python 之后学习第二语言，Java 可能是一个不错的选择。在本文中，我们将讨论一个软件开发初学者从 Python 转换到 Java 的情况。

![](img/619ad6f781283b99169b413c90db261d.png)

# 为什么是 Java？

有许多不同的流行语言，如 C++，C#，JavaScript，学习其中任何一种都有好处和坏处。那么为什么是 Java 呢？以下是一些想法。

*   根据大多数排名，Java 是最受欢迎的语言之一。因此，很容易找到一份好工作。
*   Java 有工业实力。它被大群人用于大型系统。大公司需要很多初级软件开发人员。所以它是初学者友好的。
*   Java 是一种通用语言，你可以用它来开发几乎任何类型的程序。它有几乎所有的图书馆。企业、移动、大数据、云服务、软件定义网络(SDN)等等。
*   如果你懂 Java，学习更复杂的语言比如 [C++](https://hackernoon.com/top-5-free-c-courses-to-learn-programming-in-2019-d27352277da0) 比刚学完 Python 容易多了。
*   Java 是 Android 的本地语言，Android 是最流行的移动平台。所以懂 Java 是手机软件开发最简单的方法。
*   JVM (Java 虚拟机)语言家族非常广泛和多样。不仅是 Java，还有现代的 Groovy、Closure 和 Kotlin。以及 Ruby 和 Python 特殊版本——JRuby 和 Jython。如果您知道如何使用 JVM 的本地语言 Java，那么使用其他语言就更容易了。Groovy 或 Kotlin 有可能成为下一个重要的编程工具(但不是很快)。

如果你需要更多的理由，你可以看看这篇关于为什么学习 Java 是个好主意的文章:

<https://javarevisited.blogspot.com/2018/07/10-reasons-to-learn-java-programming.html>  

# 转 Java…怎么办？

1.  **学习 Java 基础知识(Java 核心)**

你可以使用面向初学者的 Java 课程(对你来说既快又简单)或面向 Python 软件开发者的 Java 课程。比如:。

*   这是一个巨大的网站，有很好的文章、课程和有趣的问题要解决。
*   学习 Java 的实用导向课程— [CodeGym](https://codegym.cc/) 。在 CodeGym 上学习 Java，就是用快速的自动验证、提示、帮助等等来解决海量的编码任务。当然也有讲座，这是一门完整的课程。

**或者你可以在其他地方找到这些话题:**

*   首先读一点这个可怕的 Java 语法
*   去尝试编写简单的编码任务，只是为了适应你的新生活
*   Java 里的一切都是对象，所以要学习 OOP
*   学习 Java 集合，它们真的很方便
*   以及艰难但有用的 Java 多线程

2.将你自己的项目从 Python 翻译成 Java。

这可能是你学习时做的简单的编码任务，或者是你喜欢的项目，如果你做的话。用 Java 重写就行了。它非常有用。

**3。了解 JVM(针对更高级的程序员)**

首先，您可能会阅读一些关于 JVM、内存管理、Java 垃圾收集的文章:

[JVM 如何工作——JVM 架构？](https://www.geeksforgeeks.org/jvm-works-jvm-architecture/)

[了解 JVM 内部机制](https://www.cubrid.org/blog/understanding-jvm-internals/)

关于这个话题有一些有趣的课程。比如:

[了解 Java 虚拟机:内存管理](https://www.pluralsight.com/courses/understanding-java-vm-memory-management)

[理解 Java 虚拟机:类加载和反射](https://www.pluralsight.com/courses/understanding-java-vm-class-loading-reflection)

[了解 Java 虚拟机:安全性](https://www.pluralsight.com/courses/understanding-java-vm-security)

4.别忘了订阅 [Java 的 StackOverflow](https://stackoverflow.com/questions/tagged/java) 和一些 Reddit 社区，比如 [learnjava](https://www.reddit.com/r/learnjava/) 。使用它们！

# 开始之前。“渐进式”Python vs“遗留式”Java？

我偶然发现这样一种观点，Java 是过去的遗迹，而 Python 是现代的，并且经常是进步的。这很有趣，尤其是当你考虑到 Python 比 Java 还要老一点的时候。当然，Python 超越了它的时代。你可能会说，Java 马上“出手”，很快流行起来，而 Python 做得很慢，现在才开始在流行程度上赶上 Java。这些年来，他们都做出了重大的改变，变得更适合现代编程，并保持了他们的优势。我想说他们都有强和弱的一面，这与累进无关。在这里，我将描述这两种语言之间真正的和最重要的区别。

# 主要区别:动态类型的 Python 和静态类型的 Java

Java 是静态类型，而 Python 是动态类型。这是本题语言的主要区别。从技术上来说，这意味着在 Python 中你创建你的变量，而不定义它们的类型。类型只在运行时定义，没有运行时错误的唯一条件是支持您对变量使用的操作。所以，如果我们有两个对象，比如鸟和狗，在我们运行程序之前给它们一个快速移动的命令，它们是一样的。然而，如果我们在运行时一切都做对了，第一个会飞起来，第二个会运行起来。在 Java 中，狗在运行前“知道”自己是狗，因为它是由程序员定义的。你用 Java 写更多的东西，而你的电脑工作更少。

# Python 简洁的语法与 Java 冗长的语法

Python 的创建考虑到了简短的语法和可读性。Java 更多的是关于严格的结构和远离偶然的错误。经典例子:

**Java**

```
public class HelloWord{
     public static void main (String[] args)
       {
           System.out.println(“Hello, world!”);
        }
}
```

**巨蟒**

```
print “Hello, world!”
```

# 解释 Python 和编译 Java

Java 是编译语言，Python 是解释语言。这样说是一种简化，但却接近事实。

无论如何，在执行过程中，Python 代码在运行时被解释。Java 代码在编译时被编译成 JVM 的字节码，JVM 运行它。

# 跨平台

Java 虚拟机( [JVM](https://javarevisited.blogspot.com/2019/04/top-5-courses-to-learn-jvm-internals.html) )是创建可移植的跨平台应用程序的强大工具。这些应用程序可以在任何安装了 JVM 的设备上运行。Python 编译器将代码转换成依赖于操作系统的特定机器代码。

# Java 和 Python 中的多线程

Java 中多线程的实现比 Python 好得多(这里我们说的是标准的 CPython)。Python 中的内存管理不是线程安全的，因此，该语言的实现阻止了 Python 字节码的多线程执行。

在 Python 中，有全局解释器锁或 GIL，这种互斥锁可以防止多个线程在 Python 中执行。因此，Python 唯一的多线程选项是访问 IO 资源。因此，Python 让第二个线程执行字节码，而第一个线程应该等待来自网络或文件系统的数据。

Java 进程成功地使用了多核和超线程技术。线程安全保证不是隐藏在语言内部，而是隐藏在许多线程安全工具中，使它成为一个简单的过程。

# 向后兼容性

通常，编程语言的发展涉及到向后兼容性。然而，这并不完全适用于 Python。Python 2 于 2000 年问世，Python 3 于 2008 年问世。它们或多或少是兼容的，但在功能和语法上有足够的差异，所以原则上它们可以被认为是不同的语言。Python 3 没有修改 Python 2 中的新趋势和思想，而是被设想为一种新语言，在某种程度上使用了 Python 2 的经验。Python 2 的开发继续单独进行，其最终实现(2.7 版)将只支持到 2020 年。

Java 是在牢记向后兼容性的基础上创建的。这是好是坏？这种方法导致了巨大的遗留代码库。另一方面，这种古老的代码可以逐步更新，将 Java 情况下“老手”在现代环境下不会启动的风险降到最低。

# 那又怎样？

我描述了一些主要的差异，但几乎没有提到它们会导致什么。在这一点上，我承诺修复它，并从软件开发人员的角度讨论这些差异的影响。

## 编码速度或程序员效率

语法越短，开发人员编写的速度就越快。在 [Python](https://javarevisited.blogspot.com/2018/05/10-reasons-to-learn-python-programming.html#axzz61YisEoc6) 中，你不应该写所有这些括号和类，甚至变量类型，所以……是的，Python 程序员比 [Java 开发者](https://javarevisited.blogspot.com/2018/05/10-tips-to-become-better-java-developer.html#axzz5jwmmAbXI)工作得更快，尤其是在拥有短程序和脚本的小团队中。Java 冗长，但结构非常好。因此…

## 模式支持和分析效率

…因此，Java 代码的分析和支持比 Python 容易得多，尤其是在大型项目中。在 Python 中，类型信息的缺乏将这种分析变成了一个难题。此外，Java 提供类型安全，并在编译时捕获所有潜在的错误，而不是像 [Python](https://javarevisited.blogspot.com/2019/05/python-vs-javascript-which-programming-language-beginners-should-learn.html) 那样在运行时捕获。因此，出错的概率会降低。它极大地简化了大型应用程序的管理。运行时错误比编译期间的错误更难识别和修复。

## 程序执行速度

在 Python 的例子中，你做得更少，但是计算机做得更多。在 Java 中，与 [Python](https://javarevisited.blogspot.com/2018/03/top-5-courses-to-learn-python-in-2018.html) 相比，一切都是颠倒的。因此，由于所有这些静态和编译，而不是动态类型和解释， [Java](https://dev.to/javinpaul/10-free-courses-to-learn-java-in-depth-3ikn) 运行得更快。这意味着您的 Java 程序将在更短的时间内执行，操作也将更快。对于用户来说，这并不总是一个明显的区别。然而，有时这可能是至关重要的。如果你想了解更多的区别，这张信息图也能帮到你

<https://javarevisited.blogspot.com/2018/06/java-vs-python-which-programming-language-to-learn-first.html>  

# 结论

专业软件开发人员并不总是能够选择使用什么工具。时代在改变雇主的偏好，他们的偏好也可能随之改变。好消息是这些工具比它们看起来更相似。而且 [Java](/javarevisited/top-5-java-online-courses-for-beginners-best-of-lot-1e1e240a758) 是一个很好很强大的工具，即使在 [Python](/@javinpaul/top-5-courses-to-learn-python-in-2018-best-of-lot-26644a99e7ec) 之后它看起来太冗长了。

学习 Python 和 Java 的资源

<https://hackernoon.com/10-free-python-programming-courses-for-beginners-to-learn-online-38312f3b9912>  </javarevisited/10-free-courses-to-learn-java-in-2019-22d1f33a3915>  <https://javarevisited.blogspot.com/2019/10/the-java-developer-roadmap.html> 