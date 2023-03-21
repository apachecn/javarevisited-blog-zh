# 你知道 Oop 中的紧耦合和松耦合吗？

> 原文：<https://medium.com/javarevisited/do-you-know-the-tight-and-loose-coupling-in-the-oop-latest-b03f8e600115?source=collection_archive---------4----------------------->

## 面向对象编程

## 你的下一个面试问题可能是关于面向对象的范例。

[![](img/c066e390f8cbb81dd77a10fe673fb89a.png)](https://javarevisited.blogspot.com/2020/05/object-oriented-programming-questions-answers.html)

紧耦合和松耦合

在学习 java 编程时，您可能会遇到松耦合和紧耦合的概念。当我在网上搜索 java 面试问题时，我发现这个问题很常见。他们中的许多人试图用自己的方式用不同的例子来解释。但是网上写的答案之间的差异程度表明了为什么这是一个很难理解的概念，但是我将会讨论它，并且尝试用最简单的语言来解释它。

# 什么是紧耦合和松耦合？

**紧耦合**意味着一组类高度依赖于彼此。当一个类承担了太多的责任，或者一个问题分散在许多类中而没有自己的类时，就会出现这种情况。

**松散耦合**是通过促进单一责任和关注点分离的设计来实现的。松散耦合的类可以独立于其他类被使用和测试。

# 如何实现松耦合？

接口是用于松耦合或解耦的强大工具。类可以通过接口而不是其他具体的类进行通信。

让我们看一个 java 编程中的例子，看看我们如何实现松耦合。

## 1.紧密耦合:

紧密耦合的代码依赖于具体的实现。如果我们需要一个字符串列表，我们可以这样做:

```
ArrayList<String> myList = new ArrayList<String>();
```

那么这个实现依赖于 **ArrayList** 实现。

## 2.松散耦合:

如果我们希望将实现更改为松散耦合的代码，那么我们将引用设置为接口(或其他抽象)类型。

```
List<String> myList = new ArrayList<String>();
```

# 为什么松耦合的代码比紧耦合的好？

```
List<String> myList = new ArrayList<String>();
```

上面松散耦合的实现阻止我们使用特定于 **ArrayList** 实现的`**myList**`调用任何方法。我们仅限于那些在**列表**接口中定义的方法。这意味着`**myList**` 只能访问 **List** 接口的变量和方法，而不能访问 [**ArrayList** 类中定义的任何附加方法。](https://javarevisited.blogspot.com/2015/09/how-to-reset-arraylist-in-java-clear-vs-removeAll-example.html)

如果我们后来决定我们真的需要一个 [**LinkedList**](https://www.java67.com/2016/02/how-to-sort-linkedlist-in-java-example.html) 而不是 **ArrayList** ，那么我们只需要在一个地方修改代码，在那里我们已经创建了 **new** **List** ，而不是在我们已经调用了 **ArrayList** 方法的 100 个地方。

当然，我们可以使用第一个声明实例化一个 **ArrayList** 并限制自己不使用任何不属于 **List** 接口的方法，但是使用第二个声明使编译器保持诚实。

# 现在让我们看一个实际的例子来更好地理解它:

## IMachine 接口:

作为接口的机器

## 实现 IMachine 的 Machine 类:

Machine 类实现 IMachine 接口

## 驱动程序类别:

解释松耦合和紧耦合的驱动程序类

在上面的例子中，创建了两个引用，一个使用 **Machine** 类，另一个使用 **IMachine** 接口。

```
//Tight coupling
Machine machine = new Machine();
```

上面的**机器**引用具有**机器**类的所有方法的可见性，并且可以访问它们。

```
//Loose coupling
IMachine iMachine = new Machine();
```

鉴于 **IMachine** reference 对 **IMachine** 接口的所有方法可见，并且可以访问它们，但不能访问 **Machine** 类中编写的附加方法。

使用 **IMachine** 创建引用变量的好处是，如果在开发过程的后期， **IMachine** 引用可以指向其他一些实现 **IMachine** 接口的具体类。

另外，请注意，我们只需要在引用变量被分配给对象的一个地方进行更改，而不是在引用变量调用应用程序中数百个方法的每个地方进行更改。因此，松散耦合有助于维护代码库，并允许在开发过程中将来需要时进行更改。

*本文到此为止。希望你喜欢这篇文章。*

# 类似内容可以关注[维克拉姆古普塔](https://medium.com/u/2c3b611409dc?source=post_page-----b03f8e600115--------------------------------)。