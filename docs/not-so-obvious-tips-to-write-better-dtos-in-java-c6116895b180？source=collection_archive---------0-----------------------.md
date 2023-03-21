# (不太明显)用 Java 编写更好的 dto 的技巧

> 原文：<https://medium.com/javarevisited/not-so-obvious-tips-to-write-better-dtos-in-java-c6116895b180?source=collection_archive---------0----------------------->

## 如何编写简化代码的 dto？

![](img/51b7a02232efa4839fd11698858786e9.png)

由米莎·叶利舍耶夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

今天，应用程序趋向于更加分布式。我们需要编写更多的代码来连接其他服务，并且仍然尽量保持简单。

为了使用来自外部服务的数据，我们通常将 JSON 有效负载转换为数据传输对象(d to)。处理 dto 的代码会很快变得复杂**，**，但是一些技巧会有所帮助。我们可以编写更易于交互的 dto，使客户端代码更易于编写和阅读。结合使用，这些建议有助于保持简单。

# DTO 从手册中序列化

让我们从使用 JSON 的典型方式开始。下面是一个 JSON 结构。它代表了里贾纳披萨。

为了在我的应用程序中使用这些数据，我创建了一个名为`PizzaDto`的简单 DTO。

`PizzaDto`是一个[普通的旧 Java 对象](https://en.wikipedia.org/wiki/Plain_old_Java_object):一个有属性、getters、setters 的对象，仅此而已。它反映了 JSON 结构，所以 object 和 JSON 之间的转换只是一行代码。下面是一个关于[杰克逊图书馆](https://github.com/FasterXML/jackson)的例子:

转换很简单。那么，有什么问题呢？

在现实生活中，dto 可能相当复杂。创建和初始化 DTO 的代码可能非常庞大:有时有几十行代码。有时更多。这是一个问题，因为复杂的代码包含更多的 bug，并且对变化的响应更慢。

我简化 DTO 创建的第一次旅行是使用一个 [**不可变的**](/javarevisited/how-to-create-an-immutable-list-list-and-map-in-java-5ac1254c128) DTO:一个创建后不能修改的 DTO。

如果你不熟悉这个想法，这听起来可能有点奇怪，所以让我们集中讨论这个话题。

# 创建不可变的 dto

简单地说，当一个对象的状态在构造后不能改变时，它就是不可变的。

让我们重写`PizzaDto`，使其成为不可变的[。](https://javarevisited.blogspot.com/2010/10/why-string-is-immutable-or-final-in-java.html#axzz4sZOoYUxv)

不可变版本没有设置器。所有属性都是最终的，必须在构造时初始化。

如你所见，配料清单并不是按原样存储的。相反，我使用`List.copyOf()`来保存输入的不可修改的[副本。这可以防止客户修改储存在 DTO 中的配料。](https://javarevisited.blogspot.com/2018/02/java-9-example-factory-methods-for-collections-immutable-list-set-map.html)

这很重要，因为没有蘑菇的里贾纳披萨绝对不是里贾纳披萨。

更严重的是，[*Effective Java*](https://www.java67.com/2018/01/effective-java-3rd-edition-by-joshua-bloch-must-read-book-for-java-develoeprs.html)的作者 Joshua Bloch 给出了创建不可变类的建议:

> 如果您的类有任何引用可变对象的字段，请确保该类的客户端无法获取对这些对象的引用约书亚·布洛赫

如果你的 DTO 的任何属性是可变的，你需要制作**防御副本**。有了防御副本，你的 DTO 就不会被外部修改。

***Post Publish edit:****从 Java 16 开始，* [*记录*](https://docs.oracle.com/en/java/javase/16/language/records.html) *提供了一种更简洁的创建不可变类的方法。*

好的。现在我们有了一个不可改变的 DTO。但是它是如何简化代码的呢？

# 不变性的好处

不变性带来了许多好处，但这里是我最喜欢的:不可变变量是无副作用的。

让我们通过一个例子来看看这一点。这段代码中有一个错误:

运行这段代码后，`pizza`没有预期的状态。哪一行导致了问题？

我们将考虑两个答案:首先是可变变量，然后是不可变变量。

第一个回答，用一个**可变**披萨。`pizza`由`make()`创建，但可以在`verify()`和`serve()`内修改。因此，bug 可能来自这 3 行中的任何一行。

现在，第二个答案，用一个**不变的**披萨。`make()`返回一个披萨，但是`verify()`和`serve()`不能修改它。问题只能来自`make()`。在这里，投资的范围要小得多。这个 bug 更容易被发现。

当我们使用不可变变量时，调试更容易。但是还有更多。

当一个比萨饼无效时，`verify()`可能抛出一个异常来中断这个过程。让我们改变这一点。我们希望`verify()`修复无效的披萨。

既然比萨饼是不可改变的，`verify()`不能只是修复它。它必须创建并返回一个修改过的比萨饼，并且必须修改客户端代码:

在这个新版本中，很明显`verify()`返回了一个新的、固定的披萨。不变性使你的代码更加明确。变得更容易阅读，更容易进化。

你可能不知道，但是我们每天都在使用不可变对象。`java.lang.String`、`java.math.BigDecimal`、`java.io.File`是不可变的。不变性提供了[许多其他优势](https://www.youtube.com/watch?v=FQERMVABRrQ)。约书亚·布洛赫(Joshua Bloch)在他的 [*有效 Java*](https://www.oreilly.com/library/view/effective-java/9780134686097/) 中，简单地建议“最小化可变性”。

> 不可变类比可变类更容易设计、实现和使用。它们更不容易出错，也更安全。约书亚·布洛赫

现在，有趣的问题是:我们能把它用于我们的 dto 吗？

# 不可变 DTOs 有意义吗？

DTO 的目的是在进程间传送数据。它被初始化，然后，它的状态不应该演变。要么它将被[序列化为 JSON](https://www.java67.com/2016/10/3-ways-to-convert-string-to-json-object-in-java.html) ，要么它将被客户端使用。这使得**不变性**成为一种自然的契合。不可变的 DTO 将在进程间传送数据，并保证数据不会被改变。

那么，为什么我首先写一个可变的`PizzaDto`，而不是一个不可变的？问题是，我非常确定我的 JSON 库需要 DTO 上的 getters 和 setters。

原来我错了！

# Jackson 的不可变 dto

[Jackson](https://github.com/FasterXML/jackson) 是 Java 中最常见的 JSON 库。

当你的 DTO 有 getters 和 setters 时， [Jackson 可以将对象映射到 JSON](https://www.java67.com/2015/02/how-to-parse-json-tofrom-java-object.html) 而不需要任何额外的配置。但是对于不可变对象，Jackson 需要一点帮助。它需要知道如何构造对象。

对象的构造函数必须用`@JsonCreator`注释，每个参数必须用`@[JsonProperty](https://javarevisited.blogspot.com/2017/10/jackson-json-parsing-error.html)`注释。让我们在 DTO 的构造函数上添加这些注释。

仅此而已。我们有一个不可变的 DTO，Jackson 可以把它转换成 JSON，再转换回 object。

# 具有 Gson 和 Moshi 的不可变 dto

Gson 和[魔石](https://github.com/square/moshi)是杰克森的两个替代品。

有了这些库，将 JSON 转换成不可变的 DTO 更加简单，因为它们不需要任何额外的注释。

但是，为什么杰克逊需要注释，而 Gson 和 Moshi 不需要？

这不是魔法。实际上，当 [Gson](https://www.java67.com/2017/05/how-to-convert-java-object-to-json-using-Gson-example-tutorial.html) 和 Moshi 从 JSON 生成一个对象时，他们通过反射创建并初始化它。最后，他们只是不使用构造函数。

我不太喜欢这种方法。这是一种误导，因为开发人员可能会将一些逻辑放入构造函数中，而永远不知道它没有被调用。相比之下，我觉得杰克逊要安全得多。

# 避免空值

杰克逊还有一个优点。如果我们在构造函数中加入一些逻辑，无论 DTO 是由应用程序代码创建的，还是由 JSON 生成的，它都会被调用。

我们可以利用这一点，避免空值。我们可以改进构造函数，用非空值初始化字段。

在下面的代码片段中，当输入为 null 时，字段用空值初始化。

很多时候，空和`null`没什么区别。如果我们用空值替换空值，客户端可以使用 DTO 属性，而无需首先检查它是否为空。另外，它降低了获得 [NullPointerExceptions](https://www.java67.com/2019/06/top-25-java-exception-interview-questions-answers.html) 的机会。

根据这个技巧，您可以编写更少的代码，并提高健壮性。我们怎样才能做得更好？

# 最后但同样重要的是，与建设者一起创造 DTO

我使用另一个技巧来简化 DTO 初始化。对于每个 DTO，我创建一个 [**建造者**](http://javarevisited.blogspot.sg/2012/06/builder-design-pattern-in-java-example.html) 同伴。构建器提供了一个流畅的 API 来简化 DTO 初始化。

这是一个使用构建器创建 PizzaDto 的示例:

对于复杂的 dto，构建者使代码更具表现力。这种模式如此出色，以至于 Joshua Bloch 几乎用它来开始他的 [*有效 Java*](https://www.oreilly.com/library/view/effective-java/9780134686097/) 。

> 这个客户端代码易于编写，更重要的是易于阅读。约书亚·布洛赫

它是如何工作的？builder 对象只是存储值，直到我们调用`build()`，它实际上用存储的值创建了所需的对象。

这里有一个`PizzaDto`的例子。

有些人使用 [Lombok](https://projectlombok.org/) 在编译时创建构建器。这使得 dto 变得简单。

我更喜欢用[生成器 IntelliJ 插件](https://plugins.jetbrains.com/plugin/6585-builder-generator)生成生成器代码。然后，我可以添加方法重载，就像我在前面的代码片段中做的那样。构建器更加灵活，客户端代码更加精简。

# 结论

这些是我用来写 dto 的主要技巧。一起使用，它们真的改善你的代码。代码库变得更容易阅读，更容易维护，并且最终更容易与您的团队共享。

感谢阅读。我希望你学到了一些技巧。如果您有任何反馈，或者您有其他与 d to 相关的提示，请留下您的评论。我很想读一读！

也感谢贡献者:hélose hem Bert 的早期评论和 Julien Sobczak 的宝贵反馈。最后，[Java 访问了](https://medium.com/javarevisited)。这篇文章的出版吸引了更多的人。

# 资源

## 书

*   [**有效 Java**](https://www.oreilly.com/library/view/effective-java/9780134686097/) ，作者约书亚·布洛赫，2017

## 正式会议

*   [**永恒性的力量和实用性**](https://www.youtube.com/watch?v=FQERMVABRrQ) ，文卡特·苏布拉马年，2018