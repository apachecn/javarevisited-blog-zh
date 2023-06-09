# 像高级开发人员一样调试

> 原文：<https://medium.com/javarevisited/debug-like-a-senior-developer-1a9d9163236a?source=collection_archive---------5----------------------->

![](img/2210415b7732228265909515eb7b2a26.png)

我的关于调试的书[已经被预订了](https://www.amazon.com/dp/1484290410/)，我非常激动地宣布我正在做一个完整的在线课程来配合它。课程网站还没有准备好，但是我已经有了完整的大纲和很多录音材料。

在接下来的几个月里，我将尝试以每周两个的速度放弃视频，直到整个课程上线。这将是一门非常详细的课程。我录制了第一个模块和第二个模块的一半，我已经接近 3 个小时的密集录制材料！

看看下面这个系列的第一个视频和完整的文字记录。第一个模块包含 9 个视频，全部 100%免费，深入介绍 [IDE 调试](https://javarevisited.blogspot.com/2022/09/java-debugging-interview-questions.html)。它们大致对应于我的书的第一章。完整的课程涵盖了我在其他 13 章中涉及的细节，尽管它不能涵盖这本书的全部内容，但它展示了一些很难在书中展示的元素。

我将在一个新的 [YouTube 频道](https://www.youtube.com/@debugagent/)上发布课程，我将非常感谢喜欢和订阅。如果您喜欢该视频，请订阅该网站或其中一个邮件列表，如[此处](https://debugagent.com/)，以便获得有关新视频和完整课程的通知。

除了这里，我不打算在任何其他渠道发布课程。所以如果你在这里以外的地方看到这个课程，请告诉我，不要在那里买。

# 副本

大家好，欢迎来到大规模实际调试。在本课程中，我将教你如何调试，但更重要的是。我希望改变你看待调试和编程的方式。

在 90 年代中期，我是一个过于自信的开发者。我知道“所有的事情”,资深开发人员不断地向我咨询，尽管当时我还很年轻。后来我走运了。我正在和一位高级开发人员调试一个问题，他使用调试器的方式让我大吃一惊。

这种经历既令人惊奇又令人尴尬。我的自尊心受到了伤害，但我坚持下来，尽我所能地学习调试技术。我很幸运地看到一位大师在工作，发现还有更好的方法。我希望这个课程能把这份幸运传递给你。

这也是我选择从 [IDE 调试](https://javarevisited.blogspot.com/2011/07/java-debugging-tutorial-example-tips.html)开始课程的原因。这是我们一直在使用的东西，但当我浏览这些视频时，我保证我会涵盖你可能从未听说过的功能

一个这样的特性是对象标记，它允许我们在调试时定义一个全局静态变量。这是一个惊人的特性，我们将在本课程的第三个视频中讨论。

另一个特性是“跳转到行”,它允许我们将执行点动态地移动到任意位置。我们将在本课程的第二个视频中讨论它。或者是手表渲染器，它让我们可以定制手表区域中的所有东西的外观，达到令人惊叹的程度。我们将在课程的第六个视频中讨论这个问题。

学校里不教这些东西。

但是在我们继续之前…为什么我是教这个的人？

我写了几本书，其中一本是关于调试的，它涵盖了我们将在这里讨论的许多主题。我在这个行业干了几十年，在很多公司做过顾问。这意味着我必须进入公司，找出问题所在并解决问题，然后收取高得离谱的费用。调试器是我的秘密武器！

我还在 Sun Microsystems 和 Oracle 工作过，我开发过 JVM、飞行模拟器、嵌入式系统、云解决方案和开发工具。

你可以通过我在这里列出的社交网站联系我，也可以在乳齿象、LinkedIn 等网站上关注我。你也可以在这里使用评论区。我会尽力帮忙的。

这是我的一本新书，名为《实用调试》,与本课程规模相当。在我录制的时候，它还没有发行，但是你可以预订，这是对本课程的完美补充。当你在 2023 年 1 月 20 日看到它的时候，它可能已经发布了。无论哪种方式，你都可以在网上找到。

让我们回顾一下本课程的课程表，并讨论一下我们将涉及的主题。第一部分着重于基础知识。在 IDE 中使用调试器。这似乎是一个微不足道的练习，但我保证我将涵盖您可能从未听说过或想过的调试器功能。

我将关注 JetBrains IDEs，主要是 [IntelliJ](/javarevisited/7-best-courses-to-learn-intellij-idea-for-beginners-and-experienced-java-programmers-2e9aa9bb0c05) ，因为它有最好的调试器之一。接下来我们讨论调试的理论。调试过程背后的科学方法。

工具讨论了一些我们在跟踪问题时可以使用的很酷的工具。这些是系统级工具，我们可以利用它们来窥视系统，并找出其中发生了什么。它们大多在 Linux 系统上，因为我们在那里进行了大量的部署。尤其是在集装箱领域。

接下来，我们将深入研究如何更好地编写代码，以便在调试失败时更加容易。所有代码都失败了，我们需要确保它以一种让我们容易跟踪问题的方式失败。

调试 [Kubernetes](/javarevisited/7-free-online-courses-to-learn-kubernetes-in-2020-3b8a68ec7abc) 是下一步。当中间有这么多抽象层时，如何调试一个问题呢？Kubernetes 的规模使得调试更加困难。

接下来我们将调试[无服务器](/javarevisited/7-best-serverless-and-aws-lambda-courses-to-learn-in-2021-de1820111c85)，这是一个非常痛苦的经历。Lambda 增加了短暂状态的复杂性和几乎不可能在本地复制的环境。

调试 [fullstack](/javarevisited/top-10-online-courses-to-become-a-fullstack-web-developer-in-2020-d608a6b63232) 谈了很多关于调试前端的内容，也谈了一些关于调试数据库后端的过程。目标是在现实世界的应用程序中搜索 bug 时创建一个夹钳运动。

可观察性本身并不是调试，但它是在所有这些技术中跟踪生产问题的重要工具。

在我们上主课之前，让我们更深入地了解一下什么是调试。这对我们大多数人来说应该是显而易见的，事实也的确如此。但是也有一些细微的差别。

调试是应用于计算机编程的科学方法。我们可以直接在运行的应用程序中观察到问题或行为。这很重要。在物理学中，我们有理论物理学和实验物理学。这个类比并不完全相同，因为在编程中，我们也有生产和那些复杂性。调试是关于理解实际的实现，以各种实验的方式观察它，这样我们就可以看到实际的问题并排除那些不是问题的东西。

调试让我们对项目有了深入的了解，这是我们无法通过其他方式获得的。

调试不能代替测试，你经常需要一个调试器来调试测试或者理解缺失的测试。事实上，调试和测试是齐头并进的。作为本课程的一部分，我们将讨论测试及其与调试的特殊关系。

调试提供了对我们代码的洞察，而这通常是很难得到的。你可以用它来学习新的代码。我使用调试器来研究对我来说陌生的新代码库。没有比简单的调试器更强大的代码分析或教程了。

但最重要的是。它让我们验证我们对正在运行的应用程序的假设。这有助于我们跟踪 bug。

我的许多演示都基于 PrimeMain 应用程序，你可以在我的 github 页面上找到它。在那里的时候，请随时关注我，并查看我的其他项目。

让我们打开 IDE 并调试一个简单的应用程序。

为了开始使用调试器，我们打开 PrimeMain 项目并运行它。我们需要做一些特定于 IDE 的配置来运行它，但是几乎任何 IDE 和语言都有基本的通用概念。一旦应用程序运行，我们可以在左侧放置一个断点，ide 将在断点处停止调试。

为了开始使用调试器，我们打开 PrimeMain 项目并运行它。我们需要做一些特定于 IDE 的配置来运行它，但是几乎任何 IDE 和语言都有基本的通用概念。一旦应用程序运行，我们可以在左侧放置一个断点，ide 将在断点处停止调试。

我们在这里看到的是堆栈帧，这种情况非常简单，因为我们只有一个帧。但是当我们开始调试真正的应用程序时，我们会看到堆栈会变得很深。我们可以单击堆栈中的帧，选择一个特定的方法，然后查看堆栈中变量的值。

我们在变量视图中看到了所有这些。我们可以添加观察，检查元素，甚至改变它们的值，但是我超越了我自己。这让我们可以看到堆栈框架中每个变量的值，等等。我们将在下一个视频中对此进行深入探讨

在下一个视频中，我们将讨论程序控制流。观看视频，了解整个课程。如果您有任何问题，请使用评论部分。谢谢大家！