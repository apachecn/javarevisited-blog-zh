# 为什么方法命名很重要

> 原文：<https://medium.com/javarevisited/why-method-naming-is-important-1d1625a1aa8?source=collection_archive---------2----------------------->

![](img/fb707dacf3ccbfd964b723bc0f7056d2.png)

动画[错误船长历险记](https://en.wikipedia.org/wiki/Adventures_of_Captain_Wrongel)中糟糕命名的说明(俄语双关语)

方法命名很重要。我们都知道。这写在[书籍](https://javarevisited.blogspot.com/2017/10/clean-code-by-uncle-bob-book-review.html#axzz5jSEI4IYE)中，我们在每个拉取请求中都得到命名建议。这让我们更仔细地观察我们的命名，我们在命名我们的方法时变得更好:`getThis()`，`setThat()`。你知道的。

但这不仅仅是为了清晰的名字。更多的是关于*这个名字如何真正反映代码在做什么*，它如何在没有文档的情况下支持更好的可读性和对代码的理解。

> 它实际上是双向的:当检查一个方法时，你应该检查它是否真的做了它被命名的事情。因为如果不是这样，那么*可能是代码结构不良的标志*。

我们来看一个例子。

假设我们有一个代表用户可能存储的一些配置设置的实体。让它也支持版本控制，以便一些设置更改可以恢复到以前的状态。原始设置实体存储在一个单独的表中(`config_settings`)。不同以往的版本——在另一个(`config_settings_history`)。总的来说，它对应于第四种缓变维度数据管理方法。

所有这些都用数据库模式备份，如下所示:

代码片段 1:数据库模式

现在，假设我们有一个删除实体的需求。但是因为它是版本化的，所以在删除时，来自版本历史的最新实体应该是实际的。除非指定了`allEntriesShouldBeDeleted`。在这种情况下，应删除实际和历史实体。一切都很简单。

这是通过以下代码实现的:

代码片段 2:初始实现

我希望你花一分钟时间研究一下代码。你对实施有什么问题吗？或者关于方法命名？

好的，现在我们一起来检查一下。

这里我们有`deleteConfigSettings`，它检查标志，如果标志为真，就删除实体并清除它的历史记录。

或者，如果标志被设置为 false，它调用帮助器方法`setNewestEntryFromHistoryAsActual`，将历史中的实体设置为实际实体。首先，它检查是否存在任何历史条目。如果没有-它只是删除实际的。嗯，好吧，到目前为止还说得通。

等等， ***删除实际的*** ？我以为我们是在讨论从方法名来判断设置内容？这应该是你的一面旗帜。所以记住这个。此外，我们现在有了一个[代码重复](https://javarevisited.blogspot.com/2015/06/3-ways-to-find-duplicate-elements-in-array-java.html#axzz5zyF1CL8Y)，因为在同一个工作流的不同部分调用了 delete。

`Else`不过，block 还可以。我们将历史中的一个设置为当前并从历史中删除。这就是方法名**暗示**的内容，也是应该出现的内容。但是第一部分确实属于[父方法](http://javarevisited.blogspot.sg/2011/08/what-is-polymorphism-in-java-example.html)。

让我们稍微重构一下这些方法，以便方法命名更精确地反映方法实际在做什么:

代码片段 3:重构

好的，方法`setNewestEntryFromHistoryAsActual`现在是正确的，名副其实。另外，请注意[签名](https://www.java67.com/2012/09/what-is-rules-of-overloading-and-overriding-in-java.html)。它现在只把`ConfigSettingsHistoryEntity`作为一个参数。这是一个好迹象，表明我们正朝着正确的方向前进。

方法`deleteConfigSettings`现在，嗯……更丑陋了。但是，那是最好的！我们现在可以看到它有什么问题。还记得那些删除逻辑重复的吗？嗯，还是有的，只是现在更明显了。

我们有两个删除:万一`areAllEntriesShouldBeDeleted`被设置为真，以及万一没有历史条目。但是没有必要将它们放在代码的不同部分。但是第一个也*删除了所有的历史条目*，我们不能简单地把它们合并在一个地方。

等等，为什么会这样？如果我们想删除当前条目，历史条目不是应该已经被自动删除了吗？

嗯，它们现在没有被删除，因为数据库中没有**级联**！完全忽略了:

片段 4:没有瀑布

这是一个很好的方法来得到一个你要找几个小时的虫子。仔细想想——一旦原始的历史条目被删除，保留它们就没有意义了，因为它们不会被任何地方引用。

此外，如果要创建新条目，它将继承先前已删除条目的历史。在这里，你有一个错误。在代码中的某个地方忘记这个单独的调用。

让我们添加级联:

片段 5:瀑布

对我们的方法进行更多的重构:

代码片段 6:最终实现

嗯好多了，不是吗？这个`delete`只有一个且只在一个地方。`setNewestEntryFromHistoryAsActual`我们发现并修复了[数据库模式](https://javarevisited.blogspot.com/2018/05/top-5-sql-and-database-courses-to-learn-online.html)中的一个问题，该问题被糟糕的方法命名所隐藏。

如果代码现在读起来非常好，那么它的美妙之处在于:

> 如果设置了所有条目删除标志或没有历史条目—删除条目，
> 否则—将最新设置为当前

尝试阅读初始代码。嗯，这会很乱，而且很长，到最后你会忘记开头是什么！这基本上就是当你需要添加修改或任何东西时，你应该如何阅读代码。

# 你应该永远记住:越容易阅读，就越容易维护。

所以这一切背后的想法。

糟糕的方法名或不符合名称的实现会降低代码的可读性，甚至会暴露一些严重的设计缺陷，应该被视为主要的代码问题。

## 如何检查方法命名是否正确:

*   方法名准确描述了方法的功能
*   方法只做一件事，名字简单(共享多个函数的方法往往有复杂的名字，如`deleteEntryAndSetLatestFromHistory`)
*   方法参数不应该过多(`setLatestFromHistory`不应该要求当前参数、时间戳或任何东西，因为它不应该对它们进行操作)
*   试着读出你的方法(当然，当没人的时候)，它应该是描述方法逻辑的正常句子

如往常一样，我很高兴收到任何反馈或问题！请随意将它们贴在这里的回复或个人笔记中。

## 你可能喜欢的学习 Java 的其他有用资源

[在程序中命名变量时要记住的 10 个最佳实践](http://javarevisited.blogspot.sg/2014/10/10-java-best-practices-to-name-variables-methods-classes-packages.html#axzz5Bwn8nSNW)
[2019 年 Java 程序员应该学习的 10 件事](https://javarevisited.blogspot.com/2017/12/10-things-java-programmers-should-learn.html#axzz5atl0BngO)
[从零开始学习 Java 的 10 门免费课程](http://www.java67.com/2018/08/top-10-free-java-courses-for-beginners-experienced-developers.html)
[深入学习 Java 的 10 本书](https://medium.freecodecamp.org/must-read-books-to-learn-java-programming-327a3768ea2f)
[每个 Java 开发人员都应该知道的 10 个工具](http://www.java67.com/2018/04/10-tools-java-developers-should-learn.html)
[学习 Java 编程语言的 10 个理由](http://javarevisited.blogspot.sg/2013/04/10-reasons-to-learn-java-programming.html)
[2010 年 Java 和 Web 开发人员应该学习的 10 个框架 2019 年成为更优秀 Java 开发者的小技巧](http://javarevisited.blogspot.sg/2018/01/10-frameworks-java-and-web-developers-should-learn.html)
[2019 年要学习的 5 大 Java 框架](http://javarevisited.blogspot.sg/2018/04/top-5-java-frameworks-to-learn-in-2018_27.html)
[每个 Java 开发者都应该知道的 10 个测试库](https://javarevisited.blogspot.sg/2018/01/10-unit-testing-and-integration-tools-for-java-programmers.html)

</javarevisited/top-5-java-online-courses-for-beginners-best-of-lot-1e1e240a758> 