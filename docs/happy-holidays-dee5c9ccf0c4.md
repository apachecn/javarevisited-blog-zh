# 节日快乐！

> 原文：<https://medium.com/javarevisited/happy-holidays-dee5c9ccf0c4?source=collection_archive---------8----------------------->

是时候玩一些爪哇驯鹿游戏了。

![](img/ab332972b45057031f11972f9a34ba25.png)

纽约的灯节

这个博客将主要是代码。我将展示一些 Eclipse 集合和 Java 流 API，并加入一些节日元素。在 Eclipse Collections 的帮助下，我将从用 Java 实现一个驯鹿枚举开始。

```
public enum Reindeer
{
    ***Dasher***, ***Dancer, Prancer***, ***Vixen***, ***Comet***, ***Cupid***, ***Donner***, ***Blitzen***,
    ***Rudolph***; public static ImmutableList<Reindeer> all()
    {
        return *theMostFamousReindeerOfAll*()
                .newWithAll(*theOtherReindeer*()
                        .flatCollect(Reindeer::*toList*));
    } private static ImmutableList<Twin<Reindeer>> theOtherReindeer()
    {
        return Lists.***immutable***.with(
                ***Dasher***.and(***Dancer***),
                ***Prancer***.and(***Vixen***),
                ***Comet***.and(***Cupid***),
                ***Donner***.and(***Blitzen***));
    } private static 
    ImmutableList<Reindeer> theMostFamousReindeerOfAll()
    {
        return Lists.***immutable***.with(Reindeer.***Rudolph***);
    } private static 
    ImmutableList<Reindeer> toList(Twin<Reindeer> twin)
    {
        return Lists.***immutable***.with(twin.getOne(), twin.getTwo());
    } public Twin<Reindeer> and(Reindeer other)
    {
        return Tuples.*twin*(this, other);
    } public int nameLength()
    {
        return this.name().length();
    } public Character firstLetterOfName()
    {
        return Character.*valueOf*(this.name().charAt(0));
    }
}
```

首先，我用名为`all`的方法创建了一个鲁道夫在前面的驯鹿`[ImmutableList](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/list/ImmutableList.html)`。我创建了一个其他驯鹿的`ImmutableList`，基于它们在流行歌曲中的通常顺序。我使用类型`[Twin](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/tuple/Twin.html)`，它是一个`[Pair](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/tuple/Pair.html)`，对于两个项目具有相同的类型。最后，我使用方法`[flatCollect](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/list/ImmutableList.html#flatCollect-org.eclipse.collections.api.block.function.Function-)`将所有的驯鹿组合成一个列表，这个列表被添加到鲁道夫使用`[newWithAll](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/list/ImmutableList.html#newWithAll-java.lang.Iterable-)`方法的`ImmutableList`中。

现在我将为驯鹿命名游戏实现一些测试。

```
*/**
 * Create a comma separated String of the Reindeer names.
 */* @Test
public void reindeerNameGame1()
{
    String expectedNames = **"Rudolph, "** +
            **"Dasher, Dancer, "** +
            **"Prancer, Vixen, "** +
            **"Comet, Cupid, "** +
            **"Donner, Blitzen"**; Assert.*assertEquals*(
            expectedNames,
            Reindeer.*all*().makeString(**", "**)); Assert.*assertEquals*(
            expectedNames,
            String.*join*(**", "**,
                    Reindeer.*all*()
                            .asLazy()
                            .collect(Reindeer::name))); Assert.*assertEquals*(
            expectedNames,
            Reindeer.*all*()
                    .stream()
                    .map(Reindeer::name)
                    .collect(Collectors.*joining*(**", "**)));
}
```

在这个测试中，我展示了三种不同的方法来创建驯鹿名的逗号分隔的字符串*。首先，我使用 Eclipse 集合中的`[makeString](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/RichIterable.html#makeString-java.lang.String-)`。方法`[makeString](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/RichIterable.html#makeString-java.lang.String-)`不要求对象是一个`CharSequence`。它使用对象的`toString`实现。*

*接下来，我使用 Java 8 中添加的`[String.join](https://docs.oracle.com/javase/9/docs/api/java/lang/String.html#join-java.lang.CharSequence-java.lang.Iterable-)` 。这个方法采用了一个`CharSequence`的`Iterable`，它是我用一个带有`[collect](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/LazyIterable.html#collect-org.eclipse.collections.api.block.function.Function-)`的`[LazyIterable](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/LazyIterable.html)`创建的。*

*最后，我用一个`[stream](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/collection/ImmutableCollection.html#stream--)`和从`all`返回的`ImmutableList`，然后用`map`将每个`Reindeer`传递给它的`name`，用`Collectors.joining`将它们全部转化成一个`String`。所有三种方法都有完全相同的结果。*

```
**/**
 * Count the Reindeer names based on their size.
 */* @Test
public void reindeerNameGame2()
{
    IntBag nameCounts =
            Reindeer.*all*()
                    .asLazy()
                    .collectInt(Reindeer::nameLength).toBag();

    IntBag nameCountsFromIntStream =
            IntBags.***mutable***.withAll(
                    Reindeer.*all*()
                            .stream()
                            .mapToInt(Reindeer::nameLength));

    Assert.*assertEquals*(nameCounts, nameCountsFromIntStream);
    Assert.*assertEquals*(3, nameCounts.occurrencesOf(5));
    Assert.*assertEquals*(3, nameCounts.occurrencesOf(6));
    Assert.*assertEquals*(3, nameCounts.occurrencesOf(7));

    Bag<Integer> nameCountsBy =
            Reindeer.*all*().**countBy**(Reindeer::nameLength);

    Assert.*assertEquals*(3, nameCountsBy.occurrencesOf(5));
    Assert.*assertEquals*(3, nameCountsBy.occurrencesOf(6));
    Assert.*assertEquals*(3, nameCountsBy.occurrencesOf(7));

    Map<Integer, Long> streamNameCounts =
            Reindeer.*all*()
                    .stream()
                    .collect(Collectors.*groupingBy*(
                            Reindeer::nameLength,
                            Collectors.*counting*()));

    Assert.*assertEquals*(new Long(3), streamNameCounts.get(5));
    Assert.*assertEquals*(new Long(3), streamNameCounts.get(6));
    Assert.*assertEquals*(new Long(3), streamNameCounts.get(7));
}*
```

*在这个测试中，我展示了四种不同的方法来计算驯鹿的长度。首先，我创建一个`[IntBag](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/bag/primitive/IntBag.html)`，通过使用`[collectInt](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/LazyIterable.html#collectInt-org.eclipse.collections.api.block.function.primitive.IntFunction-)`收集驯鹿的所有名字长度，然后转换结果`[toBag](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/IntIterable.html#toBag--)`。*

*在第二个例子中，我通过使用`mapToInt`将驯鹿的名字长度映射到一个 int，从驯鹿创建了一个`IntStream`。我使用`[withAll](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/factory/bag/primitive/MutableIntBagFactory.html#withAll-java.util.stream.IntStream-)`方法从一个`IntStream`创建一个`IntBag`。*

*这个方法在 [Eclipse Collections 9.0](/@donraab/nine-features-in-eclipse-collections-9-0-a2ca97dfdf74) 中变得可用(查看第 3 项以获得与以前版本的比较)。第三种解决方案是最简单的。我使用方法`[countBy](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/RichIterable.html#countBy-org.eclipse.collections.api.block.function.Function-)`，该方法可用于 Eclipse 集合中扩展`[RichIterable](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/RichIterable.html)`的类型。*

*最后，我用`[Stream](/javarevisited/7-best-java-collections-and-stream-api-courses-for-beginners-in-2020-3ad18d52c38)` [](/javarevisited/7-best-java-collections-and-stream-api-courses-for-beginners-in-2020-3ad18d52c38)搭配`[Collectors.groupingBy](/javarevisited/7-best-java-collections-and-stream-api-courses-for-beginners-in-2020-3ad18d52c38)`和`[Collectors.counting](https://www.java67.com/2014/04/java-8-stream-examples-and-tutorial.html)` ，这样就产生了`Integer`到`Long`的`Map`。*

```
**/**
 * Group Reindeer by the first letter of their names.
 */* @Test
public void reindeerNameGame3()
{
    Multimap<Character, Reindeer> multimap =
            Reindeer.*all*().groupBy(Reindeer::firstLetterOfName); Map<Character, List<Reindeer>> mapOfLists =
            Reindeer.*all*()
                    .stream()
                    .collect(Collectors.*groupingBy*(
                            Reindeer::firstLetterOfName)); Assert.*assertEquals*(**"Dasher, Dancer, Donner"**,
            multimap.get(**'D'**).makeString(**", "**));
    Assert.*assertEquals*(**"Comet, Cupid"**,
            multimap.get(**'C'**).makeString(**", "**)); Assert.*assertEquals*(multimap.get(**'D'**), mapOfLists.get(**'D'**));
    Assert.*assertEquals*(multimap.get(**'C'**), mapOfLists.get(**'C'**));
}*
```

*在这个测试中，我展示了两种根据驯鹿姓氏的第一个字母对它们进行分组的方法。首先，我使用直接在`ImmutableList`上可用的`[groupBy](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/list/ImmutableList.html#groupBy-org.eclipse.collections.api.block.function.Function-)`方法，它返回一个`[Multimap](https://www.eclipse.org/collections/javadoc/9.0.0/org/eclipse/collections/api/multimap/Multimap.html)`。*

*在第二个例子中，我在 [***上使用 stream 方法，在***](https://javarevisited.blogspot.com/2018/02/java-9-example-factory-methods-for-collections-immutable-list-set-map.html) 上使用 ImmutableList，在`[Collectors](https://docs.oracle.com/javase/9/docs/api/java/util/stream/Collectors.html)`实用程序类上使用`groupingBy Collector`使用 collect 方法。*

*我希望你喜欢参加这些驯鹿游戏。在这三个例子中，我探索了一堆 [Eclipse 集合](https://www.eclipse.org/collections/)和[Streams](https://docs.oracle.com/javase/9/docs/api/java/util/stream/Stream.html)API，它们可能对您自己的 Java 代码有用。*

*祝节日快乐，新年快乐！*

*[*月食收藏*](https://github.com/eclipse/eclipse-collections) *为* [*投稿*](https://github.com/eclipse/eclipse-collections/blob/master/CONTRIBUTING.md) *。如果你喜欢这个库，你可以在 GitHub 上让我们知道。**