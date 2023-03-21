# 如何编写你的同事会讨厌重构的构造函数

> 原文：<https://medium.com/javarevisited/how-to-write-java-constructors-your-colleagues-will-hate-refactoring-2974848b6380?source=collection_archive---------0----------------------->

## 编码气味地狱的三个步骤。

![](img/46de49ad8ce12d8ebc66c826158975c8.png)

你即将辞掉工作，想留下一个持久的印象？下面的规则将证明你有多在乎你所做的事情。正是这种*签署*源代码片段的微妙方式让人们一直在谈论你。

请看一下这段代码。它只是尖叫，“*重构我”*。

# 规则 1:删除所有二级构造函数

二级构造函数，或便利构造函数(Swift 人这样称呼它们)，提供了获取新对象的替代方法。当调用以下任一项时，它们为不属于其参数列表的参数提供默认值:

*   指定的建造商
*   另一个方便的构造函数

这里有一个难以忽视的事实(一语双关):没人在乎你的好意，那就去删除那些丑小鸭吧。这样，创建对象的人就必须负责所有的参数和潜在的类型转换。

# 规则 2:拥有不止一个主构造函数

主构造函数或指定构造函数(在 [Swift](https://www.java67.com/2019/03/5-free-courses-to-learn-swift.html) 中)初始化给定类的所有字段。所有其他构造函数都重定向到主构造函数，以消除代码复制的需要。不像 [Kotlin](https://kotlinlang.org/docs/reference/classes.html#constructors) , [Java](/javarevisited/10-free-courses-to-learn-java-in-2019-22d1f33a3915) 根本不在乎你有多少主构造函数——你也不应该！

> Kotlin 中的一个类可以有一个**主构造函数**和一个或多个**次构造函数**。

# 规则 3:尽可能多地包含副作用

大约一个月前，我在重构一些遗留代码，猜猜我发现了什么:一个构造函数中的一个语句。我怎么没想到呢？这就像去你最喜欢的意大利餐馆买一份披萨，甚至不用点餐。永远不要让用户决定何时调用一个方法——为他们调用它。

因此，在构造函数中包含有副作用的代码是一份不断赠送的礼物:

*   对象总是执行代码，因此使用者不能推迟执行
*   如果构造函数的行为改变，消费者需要改变他们的实现。所以让他们忙起来。
*   构造函数中的代码在调用线程上运行
*   我最喜欢的一个:构造函数可以抛出异常来终止对象创建。这甚至会导致安全问题，称为*终结器攻击*(参见 [Java 安全编码指南，第 7–3/OBJECT-3 节](https://www.oracle.com/technetwork/java/seccodeguide-139067.html#7))。多酷啊。

# 结论

如果你不在乎你的同事和职业，就遵循上面的规则。我保证会给你留下深刻的印象。

现在，如果你不介意的话:我有一张桌子要清理。

其他**编程篇**你可能喜欢的
[2019 年 Java 程序员应该学会的 10 件事](https://javarevisited.blogspot.com/2017/12/10-things-java-programmers-should-learn.html#axzz5atl0BngO)
[2019 年你可以学会的 10 种编程语言](http://www.java67.com/2017/12/10-programming-languages-to-learn-in.html)
[每个 Java 开发者都应该知道的 10 个工具](http://www.java67.com/2018/04/10-tools-java-developers-should-learn.html)
[学习 Java 编程语言的 10 个理由](http://javarevisited.blogspot.sg/2013/04/10-reasons-to-learn-java-programming.html)
[10 个框架 Java 和 Web 开发者应该学会的 2019 年](http://javarevisited.blogspot.sg/2018/01/10-frameworks-java-and-web-developers-should-learn.html)
[成为 2019 年要学的框架](http://javarevisited.blogspot.sg/2018/05/10-tips-to-become-better-java-developer.html)
[2019 年要学 Python 的 10 个理由](https://javarevisited.blogspot.com/2018/05/10-reasons-to-learn-python-programming.html)
[每个 Java 开发者都应该知道的 10 个测试库](https://javarevisited.blogspot.sg/2018/01/10-unit-testing-and-integration-tools-for-java-programmers.html)

<https://javarevisited.blogspot.com/2019/10/the-java-developer-roadmap.html> 