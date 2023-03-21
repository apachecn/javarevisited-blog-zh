# 我需要这个列表迭代方法的索引

> 原文：<https://medium.com/javarevisited/i-need-an-index-with-this-list-iteration-method-1e339fd55ed7?source=collection_archive---------1----------------------->

使用外部和内部迭代器通过索引在 Java 中对列表进行迭代

![](img/7314de330ea2352f91d53088ee34e1fb.png)

Maksym Kaharlytskyi 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 如何用索引遍历 Java 中的列表

在 Java 中，有几种方法可以迭代带有索引的`List`。我将介绍一些最常见的外部迭代方法，以及如何将它们与提供索引结合起来。我将解释如何使用`RandomAccess`接口安全地迭代`List`接口。

## 使用迭代器迭代

这是集合迭代的最基本形式，适用于任何`Collection`(Java 5 之前)，以及 Java 中的任何`Iterable`类型(从 Java 5 开始)。

```
List<Integer> list = Lists.***mutable***.with(1, 2, 3);
int **index** = 0;
for (Iterator<Integer> it = list.iterator(); it.hasNext(); **index**++)
{
    Integer integer = it.next();
    System.***out***.println(integer + **":"** + **index**);
}// Outputs:
// 1:0
// 2:1
// 3:2
```

如果您需要直接操作`Iterator`，请使用这种形式的迭代。例如，如果您有一个扁平的配对列表，您需要调用两次`next`()来重新构建它们的配对。

## 使用 Java 5 for-each 循环的迭代器

for-each 循环只是带有`Iterator`的 for 循环的语法糖。

```
List<Integer> list = List.*of*(1, 2, 3);
int **index** = 0;
for (Integer integer : list)
{
    System.***out***.println(integer + **":"** + **index**++);
}// Outputs:
// 1:0
// 2:1
// 3:2
```

在大多数情况下使用这种形式的迭代，**除了**当关注原始迭代性能和创建`Iterator`实例的时候。

## 使用索引 for 循环进行迭代

这是随机访问`List`的最快外部循环迭代。

```
List<Integer> list = Lists.***mutable***.with(1, 2, 3);
for (int **i** = 0; **i** < list.size(); **i**++)
{
    Integer integer = list.get(**i**);
    System.***out***.println(integer + **":"** + **i**);
}// Outputs:
// 1:0
// 2:1
// 3:2
```

当您优化迭代代码以提高性能时，请使用这种形式的迭代。这主要适用于库或框架代码，但也可能适用于应用程序代码中的一些性能关键或垃圾敏感区域。只有在你能保证一个`List`是`RandomAccess`的地方才使用这个代码。

但是你怎么知道一个列表是否支持随机访问呢？

## 随机访问接口

在 Java 1.4 中，引入了一个名为`RandomAccess`的标记接口。我不知道为什么它只作为一个标记接口被添加，并且没有在它上面定义`size`和`get`的方法。这个接口被许多库算法用来优化`RandomAccess`列表，并为非`RandomAccess` `List`实例提供性能安全的迭代。您将在标准 Java 库中的`instanceof`检查中找到对该接口的引用(查看`Collections`类中的一些示例)。下面是一个使用`RandomAccess`接口迭代一个可能是也可能不是`RandomAccess`的`List`的例子。

```
List<Integer> list = Stream.*of*(1, 2, 3)
        .collect(Collectors.*toCollection*(LinkedList::new));if (list instanceof RandomAccess)
{
    for (int **i** = 0; **i** < list.size(); **i**++)
    {
        Integer integer = list.get(**i**);
        System.***out***.println(integer + ":" + i);
    }
}
else
{
    int index = 0;
    for (Integer integer : list)
    {
        System.***out***.println(integer + ":" + index++);
    }
}// Outputs:
// 1:0
// 2:1
// 3:2
```

在本例中，将执行`if`语句的`else`部分，因为`LinkedList`不是`RandomAccess` `List`。

## IntStream.range()和子列表()

从 Java 8 开始，可以使用`IntStream.range()`迭代一组索引，并使用`List`通过`get()`进行查找。`IntStream.range()`可用于索引 for 循环的面向对象版本。

```
List<Integer> list = List.*of*(1, 2, 3, 4, 5);
IntStream.*range*(1, 4)
        .forEach(index ->
                System.***out***.println(*list*.get(index) + **":"** + index));// Outputs:
// 2:1
// 3:2
// 4:3
```

使用这种方法，开发人员可以迭代`List`中特定范围的索引。使用`subList`也可以达到同样的效果。

```
List<Integer> list = List.*of*(1, 2, 3, 4, 5);
list.subList(1, 4).forEach(System.***out***::println);// Outputs:
// 2
// 3
// 4
```

这段代码将输出除第一个和最后一个元素之外的所有元素。`InStream.range()`和`subList()`都包含在 from 索引中，不包含在 to 索引中。

但是请注意，使用`subList()`，我们在对`forEach`的调用中失去了对索引的访问。没有办法使用`subList`重新访问这个索引。

# 带索引的内部 Eclipse 集合迭代器

Eclipse Collections 有几个内部迭代器，它们被传递给所提供的函数接口的元素和索引。

## forEachWithIndex(ordereditable)

该方法将从第一个元素迭代到最后一个元素，并将每个元素及其索引传递给提供的`ObjectIntProcedure`。

```
MutableList<Integer> list = Lists.***mutable***.with(1, 2, 3);
list.forEachWithIndex((each, index) -> 
        System.***out***.println(each + **": "** + index));// Outputs:
// 1: 0
// 2: 1
// 3: 2
```

## reversefeachwithindex(reversible iterable)

该方法将从最后一个元素迭代到第一个元素，并将每个元素及其索引传递给提供的`ObjectIntProcedure`。

```
MutableList<Integer> list = Lists.***mutable***.with(1, 2, 3);
list.reverseForEachWithIndex((each, index) ->
        System.***out***.println(each + **": "** + index));// Outputs:
// 3: 2
// 2: 1
// 1: 0
```

## 带范围的 forEachWithIndex(ordereditable)

除了`ObjectIntProcedure`之外，还有一个`forEachWithIndex`过载，它包含`from`和`to`指数范围。

```
MutableList<ObjectIntPair<String>> result =
        Lists.***mutable***.empty();
MutableList<String> list =
        Lists.***mutable***.with(**"1"**, **"2"**, **"3"**, **"4"**, **"5"**);
list.forEachWithIndex(1, 3,
        (each, index) -> *result*.add(
                PrimitiveTuples.*pair*(each, index)));

var expected = List.*of*(
        PrimitiveTuples.*pair*(**"2"**, 1),
        PrimitiveTuples.*pair*(**"3"**, 2),
        PrimitiveTuples.*pair*(**"4"**, 3));
Assertions.*assertEquals*(expected, result);
```

根据指定的范围，此方法可用于向前和向后迭代。以下代码将反向执行，因为`from`索引大于`to`索引。

```
MutableList<ObjectIntPair<String>> result =
        Lists.***mutable***.empty();
MutableList<String> list =
        Lists.***mutable***.with(**"1"**, **"2"**, **"3"**, **"4"**, **"5"**);
list.forEachWithIndex(3, 1,
        (each, index) -> *result*.add(
                PrimitiveTuples.*pair*(each, index)));

var expected = List.*of*(
        PrimitiveTuples.*pair*(**"4"**, 3),
        PrimitiveTuples.*pair*(**"3"**, 2),
        PrimitiveTuples.*pair*(**"2"**, 1));
Assertions.*assertEquals*(expected, result);
```

## ziptwithindex

这个方法获取集合的所有元素，并将它们和它们的索引一起“压缩”到`Pair`实例中。

```
MutableList<String> list = Lists.***mutable***.with(**"1"**, **"2"**, **"3"**);
MutableList<Pair<String, Integer>> zipped = list.zipWithIndex();

var expected = List.*of*(
        Tuples.*pair*(**"1"**, 0),
        Tuples.*pair*(**"2"**, 1),
        Tuples.*pair*(**"3"**, 2));
Assertions.*assertEquals*(expected, zipped);
```

## collectWithIndex(ordereditable)

这种方法允许开发人员收集集合中的所有元素以及它们在新集合中的索引。这是一种更通用的、潜在有用的`zipWithIndex`形式，结果可以是开发人员想要的任何类型，而不仅仅是一个`Pair<Integer, Integer>`。在下面的例子中，集合被收集到一个新的`StringIntPair`记录实例集合中。

```
record StringIntPair(String value, int index){};
MutableList<String> list = Lists.***mutable***.with(**"1"**, **"2"**, **"3"**);
MutableList<StringIntPair> collected =
        list.collectWithIndex(StringIntPair::new);

var expected = List.*of*(
        new StringIntPair(**"1"**, 0),
        new StringIntPair(**"2"**, 1),
        new StringIntPair(**"3"**, 2));
Assertions.*assertEquals*(expected, collected);
```

collectWithIndex 方法将 ObjectIntToObjectFunction 作为参数。

# Eclipse 集合 11.0 的到来

写完这篇博客后，我决定是时候给`OrderedIterable`和`ListIterable`接口添加`selectWithIndex`和`rejectWithIndex`方法了。当 Eclipse Collections 11.0 发布时，开发人员将能够使用将元素和索引都作为参数的谓词进行包含性和排他性过滤。

下面的示例演示了如何根据偶数和奇数索引将一个列表划分为单独的列表。

```
ImmutableList<Integer> list = Lists.***immutable***.with(1, 2, 3, 4, 5);
ImmutableList<Integer> evenIndexes = 
        list.selectWithIndex((each, index) -> index % 2 == 0);
ImmutableList<Integer> oddIndexes = 
        list.rejectWithIndex((each, index) -> index % 2 == 0);

Assert.*assertEquals*(Lists.***immutable***.with(1, 3, 5), evenIndexes);
Assert.*assertEquals*(Lists.***immutable***.with(2, 4), oddIndexes);
```

`selectWithIndex`和`rejectWithIndex`方法在 Eclipse 集合 11.0.0.M3 里程碑版本中可用。

# 摘要

在这篇博客中，我展示了用元素和索引迭代`List`实例的不同方法。在普通 Java 中，您目前唯一的选择是使用外部迭代器。使用 [Eclipse 集合，](https://github.com/donraab/eclipse-collections)开发人员可以选择使用一些专门的内部迭代器，将元素和索引作为参数传递给提供给他们的函数接口。

尽情享受吧！

*我是*[*Eclipse Collections*](https://github.com/eclipse/eclipse-collections)*OSS 项目在*[*Eclipse Foundation*](https://projects.eclipse.org/projects/technology.collections)*的项目负责人。* [*月食收藏*](https://github.com/eclipse/eclipse-collections) *是开投* [*投稿*](https://github.com/eclipse/eclipse-collections/blob/master/CONTRIBUTING.md) *。如果你喜欢这个库，你可以在 GitHub 上让我们知道。*

**您可能想探索的其他文章:**

</javarevisited/the-java-programmer-roadmap-f9db163ef2c2>  </javarevisited/7-best-webflux-and-reactive-spring-boot-courses-for-java-programmers-33b7c6fa8995>  </javarevisited/my-favorite-spring-mvc-courses-for-java-developers-5ede7f85dd88> 