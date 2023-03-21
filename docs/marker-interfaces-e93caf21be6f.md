# 标记接口

> 原文：<https://medium.com/javarevisited/marker-interfaces-e93caf21be6f?source=collection_archive---------4----------------------->

![](img/25fc63cadb00f1fa930962f187d08070.png)

标记一块场地的工程师

用 Java 编程的时候，建议总是 [**编程到接口而不是实现。**](/javarevisited/oop-good-practices-coding-to-the-interface-baea84fd60d3#:~:text=Coding%20to%20interfaces%20is%20a,actual%20class%20with%20the%20implementation.) 接口是编程构造，允许我们对客户隐藏方法和类的真正实现细节。为了使代码不被遗漏和误操作，也为了提供灵活性，我们应该始终致力于通过接口公开我们的功能。

接口也是一个伟大的面向对象设计工具。我们知道，在 [java](/javarevisited/top-5-java-online-courses-for-beginners-best-of-lot-1e1e240a758) **中，我们可以从一个类继承，但是我们可以实现任意多的接口。**这样我们可以保持接口**的简洁和有意义**，同时我们逐渐地让类完成它们被设计的相关行为。从面向对象的角度来看，最好使用只有几个方法的接口，因为它们遵守一个重要的原则**，叫做接口分离原则** ( [**参见 S.O.L.I.D 原则**](/javarevisited/keep-calm-and-s-o-l-i-d-7ab98d5df502) )。

> 当您必须实现一个接口，迫使您实现您并不真正关心的方法时，这种感觉并不好。尊重接口分离原则将帮助你构建可重用和可维护的代码！

因此，如果我们使用的接口的大小对代码的可重用性和维护有如此直接的影响，我们能拥有完全没有方法的接口吗？

答案是**是的！**该接口称为**标记接口**。很少看到程序员使用它们，但是它们确实存在，而且非常有用。Java API 中最流行的标记接口之一可能是**可串行化的、可克隆的**和**远程的**。自己去看，去 [Java API 文档](https://docs.oracle.com/javase/7/docs/api/java/io/Serializable.html)并搜索它们中的任何一个，你会读到这样的内容:

> … **序列化接口没有方法或字段，仅用于标识可序列化的语义。**

现在我们已经理解了标记接口背后的思想，让我们来看一个简单的例子，它将说明如何在不增加任何额外行为的情况下帮助我们进行标记。

想象一下，一家矿产勘探公司正在使用软件来加速对他们正在寻找的某些材料的检测。他们尤其对某些种类的岩石和金属感兴趣。他们将金属和岩石称为“*”可探索物“【T3”)，因为它们是他们追求的主要材料类别。此外，有时在他们的活动中，他们发现许多其他材料，他们知道在市场上也是有价值的，但他们的设施没有能力处理它们。他们不想丢弃它们，因为他们知道他们仍然可以从中获得一些边际利润，所以他们所做的是将它们转发给一家专门的关联公司，该公司将从他们那里购买它们。他们把这另一类材料称为*“可销售的”。**

他们建立了一个早期分类程序，称为*“材料资源管理器”*，用于对“*资源管理器”*进行初步评估，并将其转发给各自的部门进行进一步评估和处理。首先，在我们研究这个程序之前，让我们来看看他们的系统支持的不同材料。

金属是“可探索的”

岩石是“可探索的”

摇滚和金属是“可探索的”，他们实现了*Explorable.java*接口，他们必须履行契约。

白银是“可勘探的”和“可销售的”

标记界面的示例

由于他们对加工白银不感兴趣，但他们知道白银“适销对路”，当他们找到白银时，他们使用**标记接口**来确保软件知道如何将其转发给他们的合作伙伴。

使用标记接口的代码示例

marker 接口有助于 MarketExplorer 检测所有可以提供给外部合作伙伴/分销商的材料，这样可以让公司赚取额外的收入，而不必自己处理这些材料。

正如我们所看到的，标记接口是一个有趣的概念，但没有广泛使用，它可以在某些场合提供一些额外的灵活性。我真的很想知道你是否曾经使用过它们，它们是否曾经帮助你解决了一个商业问题。请让我知道你的想法。

如果你喜欢这个故事，请给我们一些👏也请[跟随我们。](/@javing.uk)