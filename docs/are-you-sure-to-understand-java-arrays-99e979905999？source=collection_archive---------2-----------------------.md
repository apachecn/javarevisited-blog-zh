# 你确定了解 Java 数组吗？

> 原文：<https://medium.com/javarevisited/are-you-sure-to-understand-java-arrays-99e979905999?source=collection_archive---------2----------------------->

Java 数组是聚集元素的最原始的方式。然而，你真的能理解它们是如何工作和如何使用的吗？你可能会惊讶…

![](img/48974a455c5016b1b3688c0ce7f273af.png)

数组代表一行行的东西

# Java 数组:基础知识

Java 数组不是这种语言的新特性。回顾一下基础，让我们回忆一下数组类型是用括号`[ ]`表示的，并用花括号`{ }`以一种非常简单的方式创建的:

```
String[] myArrayOfStrings = { "Hello", "World" };
```

以上是更详细形式的语法糖(我们将在本文中坚持使用)

```
String[] myArrayOfStrings = new String[] { "Hello", "World" };
```

由于 JVM 的优化，访问数组中的元素预计会非常快。与 Rust 之类的语言相比，数组长度不是数组类型的一部分:我们的数组类型是`String[]`，没有关于其长度的信息。然而，[数组的长度是不可变的](/javarevisited/20-array-coding-problems-and-questions-from-programming-interviews-869b475b9121):数组不能改变它的大小。

为了更改数组的大小，您需要执行两步操作:首先创建一个具有预期新大小的数组，然后在目标中复制源:

```
String[] myArrayOfStrings = ...;
String[] withAugmentedSize = new Steing[4];
System.arraycopy(myArrayOfStrings, 0, withAugmentedSize, 0,
                 myArrayOfStrings.length);
```

# 数组方差

在上面的例子中，字符串数组

```
new String[] { "Hello", "World" }
```

存储在类型为`String[]`的变量中。有人可能想知道我们可以用什么样的变量来存储字符串数组。

好消息是数组类型是*协变的*。这意味着如果`String`是`Object`的子类型，那么你可以在`Object[]`类型的变量中存储一些`String[]`:

```
Object[] myArrayOfStrings = new String[] { "Hello", "World" };
```

像往常一样，您应该注意两件事:

1.  使用变量`myArrayOfStrings`不允许您对条目执行`String`操作，因为它们是`Object`类型，即使它们实际上是`String`
2.  使用`new Object[]{ ... }`创建数组也会丢失条目实际上是字符串的信息。

为什么数组是协变的？嗯，这个可能看`arrayCopy`方法就明白了。如果数组类型是不变的(如 Java 中的泛型类型)，这意味着您将需要与数组类型一样多的`arrayCopy`版本。

由于在 [Java 生态系统](/javarevisited/top-5-java-online-courses-for-beginners-best-of-lot-1e1e240a758)中每种类型都有一个数组类型，所以不能编写`arrayCopy`方法。你可以用一些泛型来争论，但是不要忘记在那个时候，Java 没有泛型！(它们只出现在 Java 5 中)。

# 什么会出错？

让我们更仔细地检查以下两个代码:

```
Object[] myArray1OfStrings = new String[] { "Hello", "World" };
```

对抗

```
Object[] myArray2OfStrings = new Object[] { "Hello", "World" };
```

在这两种情况下，变量`myArray_OfStrings`都被定义为`Object[]`。这意味着最终用户在访问数组的任何条目时都会收到一些`Object`:

```
Object x = myArray_OfStrings[0]; // works
String x = myArray_OfStrings[0]; // does not workmyArray_OfStrings[0] instanceof String; // true
String x = (String) myArray_OfStrings[0]; // works
```

但是还有另一个微妙的区别。如果我们知道试图用另一个对象改变我们的条目会发生什么:

```
myArray1OfStrings[0] = new Object(); //java.lang.ArrayStoreException
myArray2OfStrings[0] = new Object(); //works
```

到底发生了什么？这里有一个窍门:变量`myArray2OfStrings`引用了一个已经被创建为数组`Object`的数组，这个数组包含字符串实例。作为对象的数组仍然知道很多事情:它的长度，以及它的类型。换句话说，`myArray2OfStrings`引用的实例知道它是一个`Object`的数组，因此，如果您在它的一个条目中插入另一个通用对象，它不会崩溃。

相反，`myArray1OfStrings`引用的实例是一个已经被创建为`String[]`数组的数组。它还知道它的长度和类型，并且知道它应该包含的元素应该是`String`实例。因此，试图在一个条目中插入某个通用对象会导致一个`ArrayStoreException`:你不能在一个[字符串](/javarevisited/top-21-string-programming-interview-questions-for-beginners-and-experienced-developers-56037048de45)的数组中存储一个对象。

## 但是方差呢？

你应该困惑地了解到:我们上面看到的协方差是什么？如果`String[]`是`Object[]`的子类型，我们应该可以在其中存储`Object[]`。

嗯，事实上还有另一件微妙的事情。重点是`String[]`可以赋给`Object[]`，这意味着你可以在`Object[]`类型的变量中引用`String[]`的一个实例。

`instanceof`操作符也表现得很好，因为它主要检查可分配性。

但是你要注意`String[]`是*没有`Object[]`的子类*。快速测试表明:

```
String[].class.getName(); // [Ljava.lang.String
Object[].class.getName(); // [Ljava.lang.Object
String[].class.getSuperclass().getName(); // java.lang.Object
```

换句话说，`String[]`不是`Object[]`的子类，尽管它是一个子类型。具体来说，你可以在一个`Object[]`中存储一个`String[]`，但是你不能对它执行任何`Object[]`操作(具体来说，你可以随心所欲地对条目进行变异)。

# 好吧，但是谁在乎呢？

你可能觉得上面的例子真的很诡异，没有人绝对不会这么做。嗯，不要那么自信！

假设您有一些接口`Foo`和一个实现`FooImpl`:

```
public interface Foo {}class FooImpl implements Foo {}
```

然后你暴露一个`Foo`数组(没有`FooImpl`，当然！)但是由于您需要对它执行第一个自定义操作，您可以这样写:

```
Foo[] getAllFoos() { FooImpl[] exposedFoos = new FooImpl[] { ... }; performCustomOperationsOnFoos (exposedFoos);
   // this required the array to be FooImpl[]
   // for achieving custom goals return exposedFoos;
}
```

现在，如果一个客户也想改变数组来实现他的目标，他会以某种方式结束`ArrayStoreException`！

社区中没有好的实践:您应该首先复制数组以公开一个公平的`Foo[]`还是应该由客户端负责在必要时进行复制？

**我们至少可以说**(请不要卷入原始数组的争论)**每个人都应该意识到那些管理原始数组世界的微妙规则！**