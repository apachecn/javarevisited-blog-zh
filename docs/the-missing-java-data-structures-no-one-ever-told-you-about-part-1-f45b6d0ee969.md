# 没有人告诉过你的缺失的 Java 数据结构—第 1 部分

> 原文：<https://medium.com/javarevisited/the-missing-java-data-structures-no-one-ever-told-you-about-part-1-f45b6d0ee969?source=collection_archive---------0----------------------->

Eclipse Collections 提供了 JDK 中没有的其他集合类型。

![](img/0cb86cbfe08dc715288ebe751f313b1d.png)

在 JDK 中找不到 Eclipse 集合数据结构

# 在 JDK 找不到的数据结构

JDK 可能没有构建 Java 应用程序所需的所有数据结构。在这个博客系列的第 1 部分中，我将介绍几种您在今天的 JDK 中找不到的数据结构，但是它们可以在 [Eclipse 集合](https://github.com/eclipse/eclipse-collections)中找到。

*   `[Interval](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/impl/list/Interval.html)`
*   `[Bag](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/api/bag/Bag.html)`
*   `[Multimap](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/api/multimap/Multimap.html)`
*   `[HashingStrategy](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/api/block/HashingStrategy.html)` ( `[Set](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/impl/set/strategy/mutable/UnifiedSetWithHashingStrategy.html)`、`[Map](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/impl/map/strategy/mutable/UnifiedMapWithHashingStrategy.html)`、`[Bag](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/impl/bag/strategy/mutable/HashBagWithHashingStrategy.html)`、`[Pool](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/api/set/Pool.html)`)
*   `[BiMap](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/api/bimap/BiMap.html)`

# 间隔

*   一个`[Interval](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/impl/list/Interval.html)`是具有`from`、`to`和`step`值的一系列`Integer`值。
*   一个`Interval`是不可变的。
*   根据`from`是小于还是大于`to`值，`Interval`可以前进或后退。
*   在 [Eclipse 集合](https://github.com/eclipse/eclipse-collections)中，`Interval`类是一个`List<Integer>`，也是`RandomAccess`。
*   在 Eclipse 集合中，`Interval`类也是一个`LazyIterable<Integer>`。
*   一个`Interval`对于快速(并且有效地)创建一系列`Integer`值作为一个`List`非常有用。因为一个`Interval`是一个`List<Integer>`，它可以用来测试与其他`List`实例的相等性。

间隔测试

# 包

*   一个`[Bag](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/api/bag/Bag.html)`是一个无序的`Collection`，允许重复。
*   A `Bag`类似于 a `Map<K, Integer>`。
*   一个`Bag`对数数东西很有用。
*   方法`countBy`接受一个`Function`并返回一个`Bag`。
*   方法`toBag`将任何 Eclipse 集合类型转换成一个`Bag`。

[Nikhil Nanivadekar](https://medium.com/u/4285d8a2ca86?source=post_page-----f45b6d0ee969--------------------------------) 有一个很棒的博客，解释了 Eclipse 集合中的`Bag`实现。该博客包括内存和性能比较。

[](/oracledevs/bag-the-counter-2689e901aadb) [## 包——柜台

### 我经常遇到计算物体数量的需要。我体验到了数数的必要性…

medium.com](/oracledevs/bag-the-counter-2689e901aadb) 

袋子测试

# 多地图

*   `[Multimap](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/api/multimap/Multimap.html)`是一种类似于`Map`的数据结构，允许每个键有多个值。
*   一只`Multimap`类似于一只`Map<K, Collection<V>>`。
*   一个`Multimap`对分组很有用。
*   方法`groupBy`接受一个`Function`并返回一个`Multimap`。

Nikhil Nanivadekar 也有一个很棒的博客，解释了 Eclipse 集合中的`Multimap`实现。该博客还包括内存比较。

[](/oracledevs/multimap-how-it-works-a3430f549d35) [## 多地图——工作原理

### 在我以前的博客中，我解释了 Eclipse 集合 UnifiedMap 和 UnifiedSet 是如何工作的。在这个博客中，我们将采取…

medium.com](/oracledevs/multimap-how-it-works-a3430f549d35) 

多重映射测试

# 哈希策略+池

*   `[HashingStrategy](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/api/block/HashingStrategy.html)`是一个接口，帮助你将一个对象的唯一性定义具体化。
*   开发人员需要在接口上实现两个方法— `computeHashCode`和`equals`。
*   `HashingStrategy`对于创建基于散列的数据结构很有用，不需要专用的键对象来定义唯一性。
*   `Pool`是一个带有`put`和`get`方法的接口。它像`Set`一样保存唯一的值，像`Map`一样提供`get`方法来查找值。

今天在 Eclipse 集合中有几种可用的数据结构。

1.  `[UnifiedSetWithHashingStrategy](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/impl/set/strategy/mutable/UnifiedSetWithHashingStrategy.html)`——一个`MutableSet`和一个`Pool`
2.  `[UnifiedMapWithHashingStrategy](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/impl/map/strategy/mutable/UnifiedMapWithHashingStrategy.html)` — a `MutableMap`
3.  `[HashBagWithHashingStrategy](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/impl/bag/strategy/mutable/HashBagWithHashingStrategy.html)` —一个`MutableBag`
4.  `[Object<Primitive>HashMapWithHashingStrategy](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/impl/map/mutable/primitive/ObjectIntHashMapWithHashingStrategy.html)` [](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/impl/map/mutable/primitive/ObjectIntHashMapWithHashingStrategy.html)—适用于所有图元类型。

# BiMap

*   A `[BiMap](https://www.eclipse.org/collections/javadoc/10.4.0/org/eclipse/collections/api/bimap/BiMap.html)`允许基于键或值的查找。
*   键和值都必须是唯一的
*   通过调用`inverse`可以进行值查找。

# 但是等等，还有更多…

这些只是今天在 JDK 中找不到的一些数据结构。在这个博客系列的第 2 部分和第 3 部分中，我将讨论`MultiReader`集合和基本集合。

*我是*[*Eclipse Collections*](https://github.com/eclipse/eclipse-collections)*OSS 项目在*[*Eclipse Foundation*](https://projects.eclipse.org/projects/technology.collections)*的项目负责人。* [*月食收藏*](https://github.com/eclipse/eclipse-collections) *为* [*投稿*](https://github.com/eclipse/eclipse-collections/blob/master/CONTRIBUTING.md) *。如果你喜欢这个库，你可以在 GitHub 上让我们知道。*

其他 **Java 编程文章**你可能喜欢:
[Java 程序员应该学习的 10 件事](/javarevisited/9-things-java-programmers-should-learn-in-2018-3f0b2207dfc4)
[你可以学习的 10 种编程语言](/hackernoon/10-best-programming-languages-to-learn-in-2019-e5b05af4a972)
[10 个工具每个 Java 开发者都应该知道的](http://www.java67.com/2018/04/10-tools-java-developers-should-learn.html)
[2021 年学习 Java 编程语言的 10 个理由](http://javarevisited.blogspot.sg/2013/04/10-reasons-to-learn-java-programming.html)
[完整的 Java 开发者路线图](/javarevisited/the-java-programmer-roadmap-f9db163ef2c2)
[我最喜欢的初学者免费编程课程](/javarevisited/top-10-free-interactive-programming-courses-from-educative-for-beginners-to-learn-in-2021-713cbf96d4eb)

7 最佳数据结构与算法初学者课程
[50+数据结构与算法面试题](/hackernoon/50-data-structure-and-algorithms-interview-questions-for-programmers-b4b1ac61f5b0)