# 您不知道存在的 4 种不同的 Java 库

> 原文：<https://medium.com/javarevisited/4-miscellaneous-java-libraries-that-you-didnt-know-existed-ef37125325dc?source=collection_archive---------2----------------------->

[![](img/39d2bef82ddf89ec8452c0f236ff627c.png)](https://javarevisited.blogspot.com/2018/01/top-20-libraries-and-apis-for-java-programmers.html)

Java 已经存在了这么多年，并且在业界有着广泛的应用。多年来，许多 java 库被编写出来，以至于今天我们有几乎所有东西的[库](/javarevisited/20-essential-java-libraries-and-apis-every-programmer-should-learn-5ccd41812fc7)。在本文中，我将与您分享我职业生涯中遇到的一些最杂的库。

非常简单而有用的库，它允许你对 zip 文件进行操作，例如创建、提取、密码保护、加密、合并 zip 文件等等。
我曾经将这个库与 Java mail 结合起来，在自动生成的电子邮件上发送 [zip 文件](https://javarevisited.blogspot.com/2014/06/2-examples-to-read-zip-files-in-java-zipFile-vs-zipInputStream.html)。非常直观和易于使用的 api。这里有一个来自 github 页面的关于如何压缩文件夹的例子。

[**Apache Poi**](https://poi.apache.org/)一个优秀的库，拥有很多特性，允许您处理 microsoft office 文档(excel、word、power point)。如果你的公司非常依赖办公文件，而你想在流程自动化方面提供帮助，那么 [apache poi](https://www.java67.com/2014/09/how-to-read-write-xlsx-file-in-java-apache-poi-example.html) 或许可以帮到你。下面只是一个如何打印 excel 文档内容的例子。

在我的学生时代，有两件事我非常喜欢，一是 Java 编程，二是软件安全。当我第一次看到工具 [wireshark](https://www.wireshark.org/) 时，我很惊讶。我真的很喜欢它，我决定做一些教程来学习基础知识。

有一天我想看看能不能解析导出的*。pcap* 文件来尝试找到一些模式，经过一些搜索，我找到了 *jpcap。*

Jpcap 是一个优秀的 Java api。它能够帮助我解析文件并提取我想要的东西，而且它还有很多其他功能。这个库本身有一个接口和它自己的捕获网络包的系统。

它有一点学习曲线，但有一个广泛的编程/自动化网络流量分析的可能性。

如何在捕获中计算数据包的示例。

同样值得一提的是，在我发现 jpcap 之后的某个时候，我遇到了看起来具有非常相似特性的 [pcap4j](https://github.com/kaitoy/pcap4j#download) 。但我不能对 pcap4j 发表太多评论，因为我从未使用过它。

[**ta4j**](https://github.com/ta4j/ta4j)如果你对外汇或股票和技术分析感兴趣，这个库就是为你准备的。它包含许多指标，可以帮助你建立自动交易的机器人。

有一次，我启动了一个宠物项目，遗憾的是，我没有完成这个项目来实现一个二元选项机器人。它应该工作的方式是当 EMA(指数移动平均线)交叉指示买入或卖出时发送短信通知。

我对这个领域不再感兴趣了，但是我认为还是应该在本文中包含这个库，因为它非常好而且非常强大。这里有一个例子，一个[策略](https://javarevisited.blogspot.com/2014/11/strategy-design-pattern-in-java-using-Enum-Example.html)被用来决定机器人是否应该进入或退出交易。