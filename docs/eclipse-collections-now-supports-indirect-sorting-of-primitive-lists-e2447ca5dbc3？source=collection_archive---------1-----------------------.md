# Eclipse 集合现在支持原语列表的间接排序

> 原文：<https://medium.com/javarevisited/eclipse-collections-now-supports-indirect-sorting-of-primitive-lists-e2447ca5dbc3?source=collection_archive---------1----------------------->

## 有时候获得结果的最好方式是间接的

![](img/9e09505c6edd19e796f9663258970c71.png)

我最近在基本列表类型上实现了间接排序功能，这是由 [Eclipse 集合](https://www.eclipse.org/collections/) [项目](https://github.com/eclipse/eclipse-collections)支持的许多集合类型中的一部分。

之前在原始集合上有一个排序功能，即`sortThis()`方法，它是 Java JDK `Arrays.sort`方法的包装器。原始列表实现的局限性是，它只能通过元素和元素的自然顺序对数组进行排序，而不是通过元素的函数和/或一些定制的比较器。

除了元素值的自然顺序之外，为什么还要进行排序呢？以下是一些可能的原因:

*   反向排序
*   按绝对值排序
*   将索引构建到不可能或不切实际重新排序的有序数据结构中(例如，在内存列存储中实现的表格数据集)

这也支持在旧的开放特征请求中描述的用于 JDK 的功能(参见" *(coll) Arrays.sort 不能用比较器*"[https://bugs.openjdk.java.net/browse/JDK-4709823](https://bugs.openjdk.java.net/browse/JDK-4709823))对原始数组进行排序)。

`sortThis`方法有三个新版本:支持比较器的常规版本，支持采用提取器函数的“by”模式的版本，以及两者的组合。这填补了原语和对象集合 API 之间存在的对称空白。

如果单词“by”pattern 和“symmetry gap”对你来说没什么意义，请查看唐纳德·拉布的博客以获得解释:

*   介绍对称的概念:[对称共鸣](/@donraab/symmetric-sympathy-2c59d4541d60)
*   关于对称性的更多信息:[寻找对称性](/oracledevs/finding-symmetry-27944c74b6d4)
*   【靠】方法:[靠自己一些时间](/javarevisited/by-yourself-some-time-e16c0f488847)

下面是在旧的`sortThis()`方法之后列出的`MutableIntList`上的三个新的排序 API 方法:

除了不支持排序的`MutableBooleanList`之外，其他所有可变原始列表类型都引入了等效的 API。

以下是一些使用新 API 的示例:

新的`sort`方法从 [Eclipse 集合](https://www.eclipse.org/collections/)的 10.3 版本开始可用。