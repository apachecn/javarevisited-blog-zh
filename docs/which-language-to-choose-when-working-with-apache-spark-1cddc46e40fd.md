# 使用 Apache Spark 时选择哪种语言

> 原文：<https://medium.com/javarevisited/which-language-to-choose-when-working-with-apache-spark-1cddc46e40fd?source=collection_archive---------2----------------------->

![](img/51428de2f371aef8cebc183eb74286a6.png)

Spark | Scala |函数式编程

我已经用 Java 工作了 7 年，最近开始用 Apache Spark 做一些真实世界的大数据和数据科学项目。

从 Apache Spark 开始，基于 Spark 提供的实现和 API 支持，出现了一个非常大的问题:

我应该选择哪种语言？

使用 Spark 时，哪种语言是最受欢迎的？

相信我，你是找到上述所有问题答案的正确地方。敬请期待！！

进一步来说，Apache Spark 提供了三个选项供选择:

1.  Java 语言(一种计算机语言，尤用于创建网站)
2.  斯卡拉
3.  计算机编程语言

因此，本文将继续讨论以上三种选择，分析每种选择的优缺点，并为具有特定背景的特定开发人员提供最佳选择。

# **1。Java**

a.太冗长了

b.它不支持读取-评估-打印-循环(REPL)交互式 shell(但在 java 9 和更高版本中存在)。

c.Scala 中的 lambda 表达式比 java 中的强大得多。

d.spark 中发布的新特性在 java 中会稍晚一些。

e.用 Scala 和 python 写代码比用 java 写快多了。

# **2。Scala**

a.Spark 本身是用 Scala 编写的，提供了比 python 更好的用户 API。

b.在处理数据和分析数据方面，Scala 比竞争对手 python 快十倍。

c.Scala 是静态类型语言，与 python 这样的动态类型语言相比，折射静态类型语言更容易维护。

d.大数据需要与不同的外部服务和数据库集成以获得更多功能，为此 Scala 提供了一个非常直观的框架，名为 Play framework，能够与各种工具集成。

e.Scala 通过 Akka 的 actors 等原语提供了强大的并发性，这是 python 的重量级 UWSGI 进程分叉所不具备的。

f.Scala 有助于实时数据流和处理以及服务器推送功能。

g.如果你使用 Spark 流，Scala 是最好的选择。

# **3。Python**

a.Python 是比 Scala 更容易理解的语言。

b.对于机器学习，python 比 Scala 更值得推荐。

c.Python 有简单的语法和良好的标准库。

d.Python 拥有比 Scala 更先进的数据科学、机器学习和自然语言处理工具。

# **结论:**

为 Spark 选择语言时，应考虑以下几点:

1.  有效性:java 代码很长，Scala 和 python 代码没有 Java 长。Scala 比两者都有更快的性能。
2.  安慰:从一种语言转换到另一种语言并不是什么大问题。但是如果你在学习 Scala 或 python 或 java 时感到不舒服，或者反之亦然，你可以选择你最舒服的语言，把其他方面抛在脑后。
3.  未来展望:在未来，Scala 和 python 将被广泛使用，R 也包括在内。但是我个人认为 Scala 将会是大多数开发者的选择。

# **个人建议:**

如果你来自 Java 背景，选择 Scala。理解事情需要一段时间。但是如果你熟悉函数式编程，学习 Scala 并不是什么大事。如果你发现有困难，你可以回到 Java。

如果你的背景是 Python，那么从 Python 开始吧，让你感觉舒服些。

快乐学习！！！