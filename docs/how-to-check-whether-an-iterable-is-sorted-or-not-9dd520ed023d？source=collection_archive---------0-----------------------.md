# 如何在 Java 中检查一个 Iterable 是否排序

> 原文：<https://medium.com/javarevisited/how-to-check-whether-an-iterable-is-sorted-or-not-9dd520ed023d?source=collection_archive---------0----------------------->

![](img/c6ddd5ce1f9bba4a4dbc0ab81e2d28c6.png)

Jonas Jacobsson 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

`**java.lang.Iterable<T>**`

`[java.lang.Iterable<T>](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Iterable.html)`的描述相当简单。

> 实现这个接口允许一个对象成为增强的`for`语句(有时称为[“for-each loop”](https://javarevisited.blogspot.com/2015/08/java-8-journey-of-for-loop-in-java.html)语句)的目标。

简单地说，通过给定的`Iterable<T>`实例，我们可以做到这一点。

```
for (final T i : iterable) { // for-each element, as i, in iterable
    // do anything with i
}
```

**现在我们来谈谈如何知道** `**Iterable<T>**` **的实例是否已经排序的问题。**

为此我们显然需要两样东西。一个是要检查的`Iterable`实例，另一个是用于比较`Iterable`中每个元素的`[Comparable](http://javarevisited.blogspot.sg/2014/02/java-comparable-example-for-natural-order-sorting.html#axzz5BFEbT6hG)`实例。

```
static <T> boolean isSorted(
        final Iterable<? extends T> iterable,
        final Comparator<? super T> comparator) {
    // TODO: implement!
}
```

`**Iterable<? extends T>**`

这个`iterable`参数将作为一个 ***生产者*** 通过将每个元素生产到方法中来工作，这就是 ***生产者从***【PECS】***扩展*** 的部分。

`**Comparator<? super T>**`

我们将使用给定的`[comparator](http://www.java67.com/2014/11/java-8-comparator-example-using-lambda-expression.html)`参数来比较由`iterable`实例提供的每个元素。也就是说`[comparator](https://javarevisited.blogspot.com/2014/01/java-comparator-example-for-custom.html)` [](https://javarevisited.blogspot.com/2014/01/java-comparator-example-for-custom.html)将作为 ***消费*** 的对抗方法。这就是从 ***佩奇*** 到 ***消费超*** 的部分。如果您不熟悉泛型中的 PECS 概念，请参见[有效 Java](http://www.amazon.com/dp/0321356683/?tag=javamysqlanta-20) 。

有了上面的定义，方法体就有些简单了。

```
static <T> boolean isSorted(
        final Iterable<? extends T> iterable,
        final Comparator<? super T> comparator) {
    final T previous = null;
    for (final T current : iterable) { // iterable produces
        if (previous != null // comparator consumes
            && comparable.compare(previous, current) > 0) {
            return false;
        }
        previous = current;
    }
    return true;
}
```

**类型已经实现** `**Comparable**` **？**

有些类型如`[String](http://www.java67.com/2018/06/top-35-java-string-interview-questions.html)` [](http://www.java67.com/2018/06/top-35-java-string-interview-questions.html)或`[Integer](https://javarevisited.blogspot.sg/2011/08/convert-string-to-integer-to-string.html)` [](https://javarevisited.blogspot.sg/2011/08/convert-string-to-integer-to-string.html)已经实现了`Comparable`接口。这意味着我们不需要任何特定的`Comparator`实例。

```
static <T extends Comparable<? super T>> boolean isSorted(
        final Iterable<? extends T> iterable,
        final boolean natural) {
    // TODO: implement!
}
```

`**Comparable<? super T>**`

`Comparable<T>`接口是一个`[FunctionalInterface](https://javarevisited.blogspot.com/2018/01/what-is-functional-interface-in-java-8.html)` [](https://javarevisited.blogspot.com/2018/01/what-is-functional-interface-in-java-8.html)，其唯一定义的方法是`int compare(T o)`。

这意味着当你创建你的假设的`Apple`类，它扩展了`Fruit`，实现了`Comparable<Apple>`，你得到了`int compare(Apple o)`。

这使得它很难与另一种类型的`Fruit`如`Orange`相比。

这就是`? super T`来救援的地方。通过用`? super [Self]`参数化，该类可以与任何前辈或任何兄弟和后继进行比较。

这是这个方法的一行代码。

```
static <T extends Comparable<? super T>> boolean isSorted(
        final Iterable<? extends T> iterable,
        final boolean natural) {
    return isSorted(iterable,
                    natural ? naturalOrder() : reverseOrder());
}
```

`naturalOrder()`和`[reverseOrder](https://javarevisited.blogspot.com/2013/08/how-to-sort-list-in-reverse-order-in-java-collection-example.html)()`都是在`Comparator`接口中定义的实用方法，每个都有一个`<T extends Comparable<? super T>> Comparator<T> …Order()`的签名。

使用下面的测试语句，

```
final Iterable<String> unknown = Arrays.asList("c", "a", "b");
System.out.println(isSorted(unknown, true));
System.out.println(isSorted(unknown, false));final Iterable<String> natural = Arrays.asList("a", "b", "c");
System.out.println(isSorted(natural, true));
System.out.println(isSorted(natural, false));final Iterable<String > reverse = Arrays.asList("c", "b", "a");
System.out.println(isSorted(reverse, true));
System.out.println(isSorted(reverse, false));
```

我有，

```
false
falsetrue
falsefalse
true
```

如果你想学习更多关于 Java 的知识。以下是一些可供参考的有用文章:

1.  [**2019 年 Java 程序员应该学会的 10 件事**](https://javarevisited.blogspot.com/2017/12/10-things-java-programmers-should-learn.html#axzz5atl0BngO)
2.  [**从零开始学习 Java 的 10 门免费课程**](http://www.java67.com/2018/08/top-10-free-java-courses-for-beginners-experienced-developers.html)
3.  [**深入学习 Java 的 10 本书**](https://medium.freecodecamp.org/must-read-books-to-learn-java-programming-327a3768ea2f)
4.  [**每个 Java 开发者都应该知道的 10 个工具**](http://www.java67.com/2018/04/10-tools-java-developers-should-learn.html)
5.  [**2019 年 Java 与 Web Developer 应学的 10 个框架**](http://javarevisited.blogspot.sg/2018/01/10-frameworks-java-and-web-developers-should-learn.html)

# 结束语

感谢您阅读本文。对于 Java 程序员来说，这是一个非常有用技巧。如果你喜欢这篇文章，那么请考虑以下我们在媒体上发表的文章。

如果你想在每次发布新帖子时得到通知，别忘了在 Twitter 上关注**[**【Java re visited】**](https://twitter.com/javarevisited)！**