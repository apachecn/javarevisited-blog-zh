# 工厂问题—第 2 部分(不可变)

> 原文：<https://medium.com/javarevisited/as-a-matter-of-factory-part-2-immutable-8cb72ff897ee?source=collection_archive---------5----------------------->

在这个关于集合工厂的博客系列的第 1 部分[中，我展示了如何使用](/@donraab/as-a-matter-of-factory-part-1-mutable-75cc2c5d72d9) [Eclipse 集合](https://www.eclipse.org/collections/)中的工厂类来创建可变集合的实例。在第 2 部分中，我将展示如何使用相同的工厂类来创建不可变的集合。

如果您想在 Eclipse 集合中创建不可变的集合，那么根据您对`with`或`of`的偏好，您可以使用`<FactoryClass>.***immutable***.with(x, y, z)`或`<FactoryClass>.***immutable***.of(x, y, z)`。

```
ImmutableList<String> list = 
    Lists.***immutable***.with("1", "2", "3");ImmutableSet<String> set = 
    Sets.***immutable***.with("1", "2", "3");ImmutableBag<String> bag = 
    Bags.***immutable***.with("1", "2", "3");ImmutableMap<Integer, String> map = 
    Maps.***immutable***.with(1, "1", 2, "2", 3, "3");
```

如果您想要一个更简洁的创建不可变容器的选项，您可以在 Eclipse 集合中使用静态导入的`[**Iterables**](https://www.eclipse.org/collections/javadoc/8.2.0/org/eclipse/collections/impl/factory/Iterables.html)`类。

```
ImmutableList<String> list = 
    ***iList***("1", "2", "3");ImmutableSet<String> set = 
    ***iSet***("1", "2", "3");ImmutableBag<String> bag = 
    ***iBag***("1", "2", "3");ImmutableMap<Integer, String> map = 
    ***iMap***(1, "1", 2, "2", 3, "3");
```

**"i"** 方法是"**不可变**"的简称， **"m"** 方法是"**可变**"的简称。正如在第 1 部分中提到的，对于原始工厂，目前还没有等价的 Iterables 类。

如果您想要空的不可变集合，请在不可变工厂上使用 empty 方法。

```
ImmutableList<String> list = 
    Lists.***immutable***.empty();ImmutableSet<String> set = 
    Sets.***immutable***.empty();ImmutableBag<String> bag = 
    Bags.***immutable***.empty();ImmutableMap<Integer, String> map = 
    Maps.***immutable***.empty();
```

空的不可变集合是单例集合。

## ImmutableCollection 不扩展集合

在 Eclipse 集合中,`**MutableCollection**`和`**ImmutableCollection**`层次之间有重要的区别。扩展`**ImmutableCollection**`的接口并不像它们的可变对应物那样扩展`**java.util**` 等价物。本 [StackOverflow 帖子](https://stackoverflow.com/questions/29504881/immutablelist-does-not-extend-list)中详细解释了这一设计决策。虽然一个`**ImmutableList**`不扩展`**java.util.List**`，但是`**ImmutableList**`的每个实现都必须扩展`**java.util.List**`，如下图所示。`**ImmutableCollection**`实现不会有任何类似`add`、`remove`、`addAll`、`removeAll`或 clear 的变异方法，为这些类型返回的`Iterator`将不支持`remove`。然而，实现必须扩展`**java.util.List**`以支持`List`的`equals`和`hashcode`契约。

![](img/28fb157d9818c018b1c539559d2cc629.png)

ImmutableList 不扩展 java.util.List

您不能直接创建`**ImmutableList**`实现，而是必须使用适当的工厂类。工厂类不返回实现类类型，而是返回接口类型，如`**ImmutableList**`或`**ImmutableSet**`。

工厂类可以为较小的集合大小返回一个优化的容器。例如，对于`**ImmutableList**`，有针对空、单例、双例、三例以及所有到十例的优化实现。这些实现都不需要后备数组，而是如图所示，直接引用它们的元素。对于集合和贴图，优化会进行到四元组(4)。对于包，有一个特殊的优化称为`**ImmutableArrayBag**`，它将高达 20 个元素，然后将使用一个`**ImmutableHashBag**`用于更大的集合。下面应该给出了通过使用小型不可变集合所获得的潜在内存节省的指示。

```
Memory Cost in Bytes

Type - Size: Mutable — Immutable
— — — — — — — — — — - - - - - - -
List — 0: 40 – 16
List — 1: 48 – 16
List — 2: 48 – 24
List — 3: 56 – 24
List — 4: 56 – 32
List — 5: 64 – 32
List — 6: 64 – 40
List — 7: 72 – 40
List — 8: 72 – 48
List — 9: 80 – 48
List -10: 80 – 56
List -11: 88 – 80
— — — — — — — — — — 
Bag — 0 : 216 – 16
Bag — 1 : 232 – 32
Bag — 2 : 248 – 104
Bag — 3 : 264 – 136
Bag — 4 : 280 – 152
Bag — 5 : 296 – 184
— — — — — — — — — — 
Set — 0 : 112 – 16
Set — 1 : 72  – 32
Set — 2 : 96  – 56
Set — 3 : 112 – 72
Set — 4 : 208 – 96
Set — 5 : 224 – 176
```

我使用了`ObjectSizeCalculator.getObjectSize*()*`来计算每个对象的内存开销。对于 List，我对元素使用了`null`,以减少数据结构的内存开销。对于包和布景，我使用了`new Object()`来表示元素，使其具有尽可能小的对象足迹。如果不调整可变集合的大小，潜在的内存节省也会更大。我以前在一个应用程序中工作过，那里有数百万个小集合、列表和地图。使用这些小的不可变集合实现的内存节省是巨大的。

## 与 JDK 集合类型的互操作

从技术上讲，您可以将一个`**ImmutableList**`强制转换成一个`**java.util.List**`，但是这是一种丑陋且不安全的为现有库提供互操作的方式。相反，每个不可变接口上都有方法可以将类型转换为合适的 JDK 类型。下面展示了如何使用这些方法。

```
List<String> list = 
    Lists.***immutable***.with("1", "2", "3").**castToList**();Set<String> set = 
    Sets.***immutable***.with("1", "2", "3").**castToSet**();Map<Integer, String> map = 
    Maps.***immutable***.with(1, "1", 2, "2", 3, "3").**castToMap**();
```

为现有的 JDK 集合类型提供良好的互操作一直是 Eclipse 集合的核心设计原则。

**注:**在`**ImmutableBag**`上没有`**castToBag**`方法，因为现在的 JDK 没有`**Bag**`类型。

## **不可变集合的可变构建器**

还有另一种非常有用的创建不可变集合的工厂方法。在 Eclipse 集合中，每个可变类型都有一个方法，可以将自身复制到不可变的等价类型。方法名为`**toImmutable**`。

```
ImmutableList<String> list = 
    Lists.***mutable***.with("1", "2", "3").**toImmutable**();ImmutableSet<String> set = 
    Sets.***mutable***.with("1", "2", "3").**toImmutable**();ImmutableBag<String> bag = 
    Bags.***mutable***.with("1", "2", "3").**toImmutable**();ImmutableMap<Integer, String> map = 
    Maps.***mutable***.with(1, "1", 2, "2", 3, "3").**toImmutable**();
```

这意味着每个可变集合都是其对应的不可变类型的自然 ***构建器*** 。然而，我们不提供不可变的构建器来转换成任何不同的类型。所以如果你想从一个`**MutableList**`构建一个`**ImmutableSet**`，你必须使用两种方法中的一种。

```
ImmutableSet<String> set1 = 
    Lists.***mutable***.with("1", "2").**toSet**().**toImmutable**();

ImmutableSet<String> set2 = 
    Sets.***immutable***.withAll(Lists.***mutable***.with("1", "2"));
```

## **对象和原语不可变集合**

以下是您可以使用不可变工厂创建的所有对象集合类型的示例。

```
ImmutableList<T> list = Lists.***immutable***.empty();
ImmutableSet<T> set = Sets.***immutable***.empty();
ImmutableSortedSet<T> sortedSet = SortedSets.***immutable***.empty();
ImmutableMap<K, V> map = Maps.***immutable***.empty();
ImmutableSortedMap<K, V> sortedMap = SortedMaps.***immutable***.empty();
ImmutableStack<T> stack = Stacks.***immutable***.empty();
ImmutableBag<T> bag = Bags.***immutable***.empty();
ImmutableSortedBag<T> sortedBag = SortedBags.***immutable***.empty();
ImmutableBiMap<K, V> biMap = BiMaps.***immutable***.empty();
ImmutableListMultimap<K, V> mm = Multimaps.***immutable***.list.empty();
ImmutableSetMultimap<K, V> mm = Multimaps.***immutable***.set.empty();
ImmutableBagMultimap<K, V> mm = Multimaps.***immutable***.bag.empty();
```

Eclipse 集合也支持所有八种 Java 原语类型的不可变容器。这在对象和原始容器之间提供了良好的对称性。这里所有的原始类型都可以使用不可变工厂来创建。

```
ImmutableIntList list = IntLists.***immutable***.empty();
ImmutableIntSet set = IntSets.***immutable***.empty();// supports all combinations of primitives
ImmutableIntIntMap map = IntIntMaps.***immutable***.empty();  
ImmutableIntObjectMap map = IntObjectMaps.***immutable***.empty();
ImmutableObjectIntMap map = ObjectIntMaps.***immutable***.empty();ImmutableIntStack stack = IntStacks.***immutable***.empty();
ImmutableIntBag bag = IntBags.***immutable***.empty();
```

## 增长和收缩不可变集合

一旦创建了一个不可变的集合，您可能希望通过添加或删除元素来扩大或缩小它。正如我之前提到的，在 Eclipse 集合中的不可变集合接口上没有像`add`、`addAll`、`remove`或`removeAll`这样的变异方法。除了提供 ***结构不变性*** 之外，这还提供了我所说的 ***契约不变性*** 。有一些方法允许安全地复制和增长或收缩不可变集合。`**ImmutableCollection**`的扩展有`newWith`、`newWithAll`、`newWithout`、`newWithoutAll`等方法。对于`**ImmutableMap**`实现，这些方法被命名为`newWithKeyValue`、`newWithAllKeyValues`、`newWithoutKey`和`newWithoutAllKeys`。

```
ImmutableList<String> list0 = 
    Lists.***immutable***.empty();
ImmutableList<String> list1 = 
    list0.**newWith**("1");
ImmutableList<String> list2 = 
    list1.**newWithAll**(Lists.***mutable***.with("2"));ImmutableSet<String> set0 = 
    Sets.***immutable***.empty();
ImmutableSet<String> set1 = 
    set0.**newWith**("1");
ImmutableSet<String> set2 = 
    set1.**newWithAll**(Sets.***mutable***.with("2"));ImmutableMap<String, String> map0 = 
    Maps.***immutable***.empty();
ImmutableMap<String, String> map1 = 
    map0.**newWithKeyValue**("1", "1");
ImmutableMap<String, String> map2 = 
    map1.**newWithAllKeyValues**(
        Lists.***mutable***.with(Tuples.*pair*("2", "2")))
```

为了提供良好的对称性，这些方法在原始容器上也是可用的。

```
ImmutableIntList list0 =
        IntLists.***immutable***.empty();
ImmutableIntList list1 =
        list0.**newWith**(1);
ImmutableIntList list2 =
        list1.**newWithAll**(IntLists.***mutable***.with(2));ImmutableIntSet set0 =
        IntSets.***immutable***.empty();
ImmutableIntSet set1 =
        set0.**newWith**(1);
ImmutableIntSet set2 =
        set1.**newWithAll**(IntSets.***mutable***.with(2));ImmutableIntIntMap map0 =
        IntIntMaps.***immutable***.empty();
ImmutableIntIntMap map1 =
        map0.**newWithKeyValue**(1, 1);
```

在写这篇博客的时候，我发现我们在不可变的原始地图容器中缺少了一些对称性。我们目前在不可变的原始地图上没有`**newWithAllKeyValues**`,所以我为这个特性提交了一个[发布请求](https://github.com/eclipse/eclipse-collections/issues/344)。

## 摘要

要在 Eclipse 集合中创建可变或不可变的容器类型，您需要记住的只是类型的多个工厂类名的模式(`**List**`有`**Lists**`，`**Set**`有`**Sets**`，等等)。).然后使用 IDE 的代码补全来决定您是想要不可变的 还是可变的*实例。然后你可以根据你的方法命名偏好选择`**with**`或者`**of**`。*

*[Eclipse 集合](https://github.com/eclipse/eclipse-collections)适用于 Java 版本 5 到 9。因此，如果您正在从事任何尚未升级到 Java 8 的项目，这些工厂今天仍然可以使用。如果您使用的是 Java 8 之前的 Java 版本，只需使用[Eclipse Collections 7 . 1 . 1](http://mvnrepository.com/artifact/org.eclipse.collections/eclipse-collections/7.1.1)。如果使用的是 Java 8 或以上版本，可以使用 [Eclipse Collections 8.x](http://mvnrepository.com/artifact/org.eclipse.collections/eclipse-collections/8.2.0) 或即将发布的 9.x 版本。如果你愿意尝试并给我们提供反馈，今天有 9.x 的里程碑版本。*

*关于所有工厂类型以及如何创建集合容器的完整参考，您可以阅读 [Eclipse 集合参考指南](https://github.com/eclipse/eclipse-collections/blob/master/docs/guide.md#-creating-collections-containers)的这一部分。*

*[*月食收藏*](https://github.com/eclipse/eclipse-collections) *为* [*投稿*](https://github.com/eclipse/eclipse-collections/blob/master/CONTRIBUTING.md) *打开。如果你喜欢这个库，你可以在 GitHub 上让我们知道。**