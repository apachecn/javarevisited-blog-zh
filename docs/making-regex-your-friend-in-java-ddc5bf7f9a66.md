# 让正则表达式成为你的 Java 朋友

> 原文：<https://medium.com/javarevisited/making-regex-your-friend-in-java-ddc5bf7f9a66?source=collection_archive---------1----------------------->

你见过这个迷因并有点同意它吗？

![](img/ec967d1262ed9f31c8c566db87553294.png)

正则表达式迷因

好吧，那你就像我一样。我想改变这一点。本文旨在解决这个问题，并使它成为开发人员宝库中的另一个可用工具。我相信它肯定会被过度使用，但是，也有需要它的情况。

让我们深入研究一下。

# 正则表达式构造

首先，我们来谈谈一些有助于构造你的[正则表达式](/javarevisited/7-best-regular-expression-courses-for-developers-to-learn-in-2021-9b8cb37bb3a5)的构造。请注意，这些都与 Java 相关，我相信它们会因语言而异。

## 括号“[]”与括号“()”

括号称为**字符类**，圆括号称为**捕获组**【1】。

字符类是一种表示您想要匹配哪些字符的方式。比如说。

这里的输出如下。

```
true
true
false
false
```

捕获组是完全不同的东西。这是一种方式，让你说你希望你的[正则表达式](https://javarevisited.blogspot.com/2016/10/how-to-check-if-string-is-numeric-in-Java.html)保存它找到的东西，以便你以后可以检索它。换句话说，你告诉正则表达式“匹配我的正则表达式，但是为我保存这些子结果”。让我们看一个例子。

这里我们尝试匹配一个格式为:DIGIT+DIGIT=DIGIT 的字符串。我们想提取数字。

第一种模式只使用字符类，“+”(被转义，因为它在 regex 中有另一种含义)和“=”。第二种模式使用未命名的捕获组。最后一种模式使用命名捕获组。

请注意命名捕获组的语法:(？<name>图案)</name>

下面是它的输出。

```
Number of groups: 0
true
Number of groups: 3
true
3+1=4
3
1
4
Number of groups: 3
true
3+1=4
3
1
4
3
1
4
```

这是讨论**匹配**和**查找**之间差异的好时机。

## 匹配与查找

基本上有两种方法可以使用你的正则表达式。一种是**对整串匹配**。另一个用途是**查找**子字符串。

让我们使用前面的例子，尝试从字符串中获取数字。但是在这种情况下，我们只想得到数字，我们并不关心字符串的一般格式。在这种情况下，我们可以再次使用**命名的捕获组**，但是使用**查找**方法来代替。像这样。

它的输出如下。

```
Number of groups: 1
3
3
3
1
1
1
4
4
4
```

我们可以注意到，第一个组号 0 总是来自原始模式的整个匹配。而且组数基本上是 1 +定义的组。

这个例子只是在一个字符串中查找数字，基本上没有其他考虑。因此，一个简单的规则是，如果要查找特定的子字符串，使用 **find** ，如果要确保字符串的特定格式，使用 **matches** 。

## 量词

到目前为止，我只展示了匹配单次出现的例子。但是有时能够指定重复发生是有帮助的。量词其实有三种，**贪**、**舍不得**和**所有格**。

在本文中，我将只涉及贪婪的 T10，但是如果需要的话，可以随意查看其他两个。假设您对寻找数字序列感兴趣。比方说，我们想把前面的“数字+数字=数字”的例子推广到“100+50=150”这样的数字。

我还使用了一个预定义的字符类“\d”，它仅仅表示一个数字，你可以在这里找到更多预定义的类【1】。

“\\d{1，}”的意思是至少应该有一位数字。你可以给它另一个参数，这样你允许最大值为 3，例如:" \\d{1，3}。

## “或”运算符

如果需要条件匹配，这个操作符会很有帮助。假设我们有类似“评分 5”、“分数 2”、“分数 3”和“分数 9”的字符串。在提取数字时，我们需要确保格式正确。为此，我们可以使用**或**操作符。

这里的输出和预期的一样。

```
true
Rating 5
5
true
Score 2
2
true
Scores 3
3
true
Point 9
9
false
```

我添加了“s{0，1}”，以表明您可以将 OR 中的表达式变得任意复杂。

## 反向引用

假设你得到如下的字符串。

*   苹果:3 个苹果
*   橘子:4 个橘子
*   香蕉:1 根香蕉

本质上，您希望匹配类似于“单词:数字单词”的内容

为此，你需要能够引用模式内部的匹配，这是使用**反向引用**来完成的，就像这样**。**

您可以看到，我在模式的末尾使用语法“\\k <name>”引用了命名的捕获组“fruit”。</name>

这是本文的最后一项。我希望这些工具能让你在 Java 中使用[正则表达式时更有信心。我想以一篇关于性能的笔记和在野外使用 regex 的技巧来结束这篇文章。](https://javarevisited.blogspot.com/2020/04/top-5-courses-to-learn-regular-expression-regex.html)

# 编译模式

在[模式](https://javarevisited.blogspot.com/2016/02/2-ways-to-split-string-with-dot-in-java-using-regular-expression.html)上使用 matches 函数(参见第一个例子)实际上编译了一个正则表达式并在后台匹配它。其他示例在模式上显式使用了 compile 方法。这个函数非常昂贵，我可以在下面演示。

所以你想在方法调用中重用它，也许把它设置成一个[静态最终字段](http://javarevisited.blogspot.sg/2017/01/how-public-static-final-variable-works.html#axzz51iekrZ00)。

我将重用最后一个例子的成果，并对方法进行一些[重构](/javarevisited/7-best-courses-to-learn-refactoring-and-clean-coding-in-java-47bea3c67006)。请参见下面的示例。

这是结果，所以在我的机器上差别很大。您可能会得到类似的结果。

```
+------------------------------------+-----------------------------+
|           compileAlways            |            787 ms           |
+------------------------------------+-----------------------------+
|            compileOnce             |            105 ms           |
+------------------------------------+-----------------------------+
```

在用 Java 做微基准测试时，你需要小心，因为编译器是如何工作的，以及它执行的所有优化。因此，通常建议做一次热身，就像你看到的我在第一个循环中做的那样。

我在以前的文章中谈到过这一点，比如当我比较 [LinkedList 和 ArrayList](https://www.java67.com/2012/12/difference-between-arraylist-vs-LinkedList-java.html) 时，以及我在 Java 中进行微基准测试时的主要技巧，请参见下面的链接。

[](/javarevisited/consider-linkedlist-in-java-2fed1b945b48) [## 关于在 Java 中使用 LinkedList 的几点思考

### 这将是一篇不同的文章，因为在我的机器上测试了一些之后，我的假设变得有点…

medium.com](/javarevisited/consider-linkedlist-in-java-2fed1b945b48) [](https://ludvigwesterdahl.medium.com/some-tips-for-microbenchmarking-in-java-46d6c9b669d9) [## Java 微基准测试的一些技巧

### 微基准测试是对产品的一小部分进行性能测试的一种方式。也许你不想比较…

ludvigwesterdahl.medium.com](https://ludvigwesterdahl.medium.com/some-tips-for-microbenchmarking-in-java-46d6c9b669d9) 

## 参考

[1]模式(Java SE 13 & JDK 13)
[https://docs . Oracle . com/en/Java/javase/13/docs/API/Java . base/Java/util/regex/Pattern . html](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/regex/Pattern.html)