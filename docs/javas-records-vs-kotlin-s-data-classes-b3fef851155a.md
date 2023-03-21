# Java 的记录与 Kotlin 的数据类

> 原文：<https://medium.com/javarevisited/javas-records-vs-kotlin-s-data-classes-b3fef851155a?source=collection_archive---------2----------------------->

Java 中的记录类与 Kotlin 的数据类相比如何。

![](img/151a090e8aedde03b12d5d96612f949d.png)

照片由[迈克·肯尼利](https://unsplash.com/@asthetik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/java?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

最近发布的 Java 有一些非常值得注意的和开发人员友好的特性，包括记录类的概念。从`Record`类的 Java 文档中:

> 记录类是一个不可变的透明载体，它包含一组固定的值，称为记录组件。Java 语言[为声明记录类提供了简洁的语法，记录组件在记录头中声明。记录头中声明的记录成分列表构成了记录描述符。](/javarevisited/python-or-java-which-programming-language-beginners-should-learn-in-2020-de992b2650ec)

我看到这个类的第一个想法是，作为一个已经使用了 [Kotlin](/javarevisited/top-5-courses-to-learn-kotlin-in-2020-dfc3fa7706d8?source=---------16------------------) 一段时间的人，我不禁想知道这听起来和 Kotlin 的[数据类](https://kotlinlang.org/docs/reference/data-classes.html)有多相似。

所以我做了一些探索，这是我比较 Java 的记录和 Kotlin 的数据类的第一印象。但在此之前，让我们快速了解一下这些是什么，以及我们如何创建一个:

# 记录

```
public record Sample (String key, String value) {}
```

记录是从 Java 的基类`Record`扩展而来的数据容器。记录类的一个关键特性是

*   所有属性都是最终的和私有的。
*   使用`record`关键字声明一个类会生成`equals()`、`hashcode()`和`toString()`方法，从而减少锅炉板代码。

# 数据类

```
data class Sample (val key, val value)
```

数据类是 [Kotlin 的](/javarevisited/7-free-courses-to-learn-kotlin-in-2020-327c3872c1e1?source=collection_home---4------2-----------------------)数据容器类，它的一些关键特性是:

*   属性可以是最终的，也可以不是(将属性声明为`[var](https://javarevisited.blogspot.com/2018/03/finally-java-10-has-var-to-declare-local-variables.html)` [](https://javarevisited.blogspot.com/2018/03/finally-java-10-has-var-to-declare-local-variables.html)会使它们不是最终的，因此它们可以被更改)。
*   与记录类类似，将类声明为数据类会自动生成`[equals()](https://javarevisited.blogspot.com/2015/01/why-override-equals-hashcode-or-tostring-java.html)`、`[hashcode()](http://www.java67.com/2013/04/example-of-overriding-equals-hashcode-compareTo-java-method.html)`和`[toString()](https://javarevisited.blogspot.com/2012/12/3-example-to-print-array-values-in-java.html)`方法。

抛开基本定义不谈，让我们来谈谈一些关键的相同点和不同点:

# 类似

首先想到的显然是这两个特性如何通过自动生成三个方法来消除对样板代码的需求:`equals()`、`hashcode()`和`toString()`。在引擎盖下，这些代码是如何生成的是不同的，但是从一个高级开发人员的角度来看，代码是自动生成的。

# 差异

**1:** 数据类提供了一个额外的现成的`copy()`方法，允许您通过为该字段指定一个新值来选择性地更改类的属性值，而不必复制所有其他不需要新值的值。

例如，让我们创建一个`Sample`数据类的对象:

```
var sample1 = Sample("Key-1", "Value-1")
```

如果我们要创建另一个类似于`sample1`的样本对象，但只改变关键字段的值，我们可以这样做:

```
val sample2 = sample1.copy(key = "Key-2")
```

当您的数据类中有多个字段，而您只需更改其中一个字段时，这一特性就会大放异彩。记录没有这种功能。

**2:** 记录类中的字段都是私有的和最终的，但是对于数据类，情况并不总是这样，因为这取决于字段是如何声明的。例如，如果我们要将`Sample`类更新如下:

```
data class Sample(var key, var value)
```

我们可以更改字段`key`和`value`的值，因为它们现在是`var`对`val`。

**3:** 没有基本的数据类，相反,`data`只是一个关键字，用来表示声明的是什么类型的类。对于记录，JDK 中提供了一个基类`Record`，所有声明为`record`类的类都是从这个基类扩展而来的。基类`Record`提供了`hashCode()`、`toString()`和`[equals()](http://javarevisited.blogspot.sg/2013/08/10-equals-and-hashcode-interview.html)`方法。

**4:** 数据类可以扩展 [Kotlin](https://javarevisited.blogspot.com/2018/02/5-courses-to-learn-kotlin-programming-java-android.html#axzz6NpvpUald) 中的其他类，如果这些类可以并且被标记为开放的(使其可继承)。注意，在 [Koltin](https://www.java67.com/2020/05/5-free-courses-to-learn-kotlin-for-java-and-Android-developers.html) 中，所有的类默认都是最终的，除非它们被标记为`open`。但是 Java 的记录类不能扩展其他类。例如，以下代码将在 Kotlin 中工作:

```
open class Parent data class Sample(var key, var value): Parent()
```

另一方面，下面的代码不能在 Java 中编译(假设`Parent`类是

```
class Parent {} public record SampleRecord(String key, String value) extends Parent// The above line will fail to compile with "No extends clause 
// allowed for record"
```

**5:** 记录和数据类之间最后也是最重要的区别(在我看来)是定义实例变量的能力。在数据类中，我们可以执行以下操作:

```
data class Sample(var key, var value) { var option: String? = ""}
```

当我们创建一个类型为`Sample`的对象时，我们可以选择为`option`字段提供一个值。另一方面，记录类不允许实例变量。因此下面的代码不会编译:

```
public record SampleRecord(String key, String value) {

    String hello = "";     // This line will not compile.
}
```

# 结论

我希望这是 Java 的记录类与 Kotlin 的数据类相比的一个很好的总结！与数据类相比，记录类无疑更加不可变，数据类的[不变性](/javarevisited/how-to-create-an-immutable-list-list-and-map-in-java-5ac1254c128?source=---------31------------------)取决于类的声明方式。但是两者之间的相似性是所有开发人员的一大胜利:自动生成样板代码！