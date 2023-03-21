# 面试问题:在 Java 中反转不可变列表

> 原文：<https://medium.com/javarevisited/interview-question-reversing-an-immutable-list-in-java-57a891a521c5?source=collection_archive---------0----------------------->

![](img/7307ee3e7823e63aa2671e0214073207.png)

在最近的一系列 Java 访谈中，我准备了一个关于如何反转不可变列表的问题。我发现对大多数候选人来说，这个问题并不那么简单。因此，我决定分享它。

# 问题

我们必须实现以下接口来反转输入列表:

预期结果的一个例子是:

```
Input: 1, 2, 3
Output: 3, 2, 1
```

唯一的约束是输入`list`是**不可变的。**

当前的实现如下:

> 这个解决方案将产生预期的结果。然而，从性能角度来看，问题很少。你能认出他们吗？
> 
> 你会用不同的方式实现它吗？

我们考虑访问输入列表的给定索引是在一个常数时间内完成的。

作为基础，在 Oracle JDK 9 和带有 1M 元素的英特尔酷睿 i7–7700(3.6 GHz)上运行此函数会产生以下响应时间:

```
problem          avgt     403.000 us/op
```

# 可能的解决方案

## 迭代模式

首先，我们来检查一下在输入列表上迭代的方式。众所周知，对于经典的 for 有不同的替代方案。

在 Java 5 中，使用 for-each 循环:

真的更快吗？在[有效的 Java](https://www.java67.com/2018/01/effective-java-3rd-edition-by-joshua-bloch-must-read-book-for-java-develoeprs.html) 中，约书亚·布洛克说:

> for-each 循环]在某些情况下可能比普通的 for 循环提供一点性能优势，因为它只计算一次数组索引的限制。虽然您可以手工完成这项工作，但程序员并不总是这样做。

换句话说，如果我们已经预先计算了列表大小(就像我们在最初的实现中所做的那样)，那么与传统的 for 应该没有任何区别。

Java 8 的另一个选项是使用[流 API](https://javarevisited.blogspot.com/2015/03/parsing-large-json-files-using-jackson.html) :

同样，这不会提供任何主要的性能优势。使用 for-each 和 stream 解决方案运行基准测试得到了大致相同的结果:

```
problem          avgt     403.000 us/op
**for-each         avgt     403.000 us/op
stream           avgt     403.000 us/op**
```

## 数组列表分配

一个`ArrayList`是一个可调整大小的数组实现。在底层，它管理一个**容量**，它是用来存储元素的数组的大小。

每个`ArrayList`实施都有一个增长策略，该策略可能因 JDK 而异(例如，一旦阵列满了，就将容量乘以 50%)。

如果我们想摆脱这种动态增长，我们可以用给定的**初始容量**初始化`ArrayList`。这是可能的，因为我们已经知道输出列表的大小。

这是这样完成的:

让我们来看看结果:

```
problem          avgt     403.000 us/op
**initial-capacity avgt     403.000 us/op**
```

什么都没有改变。我们该如何解释呢？

最有可能的是，JIT 编译器所做的运行时优化导致人们认为丢失这个初始容量并不那么重要。如果我们禁用预热阶段，我们会注意到两个测试之间的响应时间差要重要得多。

## 元素转移

让我们仔细看看我们在生成的`ArrayList`中插入元素的方式:

这个简单表达式的时间复杂度是**线性**，不是常数。事实上，在一个`ArrayList`的零位置插入一个元素需要将所有元素向右移动。

这是**实施的主要问题**。那么有哪些不同的选择呢？

第一种选择是使用具有恒定时间的数据结构在位置 0 插入元素。在 Java 中，我们可以使用`LinkedList`,因为它管理第一个元素上的指针。

使用方法`addFirst()`在第一个位置插入:

这次有什么重大改进吗？

```
problem          avgt     403.000 us/op
**linked-list      avgt         541 us/op**
```

好多了。🍾

第二种选择是保留一个`ArrayList`结构，并将元素插入到末尾(这一次，以恒定的时间复杂度进行管理)。

因此，我们必须以相反的顺序迭代，如下所示:

```
problem          avgt     403.000 us/op
linked-list      avgt         541 us/op **insert-last-pos  avgt         281 us/op**
```

响应时间甚至比使用`LinkedList`解决方案还要短。

实际上，由于动态内存分配(每个元素都包装在一个节点对象中)，使用`LinkedList`会增加一点开销。所以，如果我们已经知道了结构的大小，那么使用像`ArrayList`这样的结构显然更快。

## 内部数组复制

`ArrayList`背靠一个阵。如果我们试图复制这个内部数组，并对这个副本进行就地反转，会怎么样呢？

`swap()`函数简单地交换给定数组中的两个索引。

有性能提升吗？

```
problem          avgt     403.000 us/op
linked-list      avgt         541 us/opinsert-last-pos  avgt         281 us/op **copy-swap-array  avgt         254 us/op**
```

有趣的是，这个解决方案甚至比前几个更快。有一些可能的解释:

*   数组副本由`System.arraycopy()`管理，这是一个本地的优化函数
*   代码对 CPU 缓存更加友好

## 视角

最后一种解决方案依赖于输入列表是不可变的这一事实。因此，我们可以通过创建我们自己的`AbstractList`在这个列表上创建一个视图:

显然，在这种情况下，比较`reverseList()`基准并不那么有趣，因为输出列表是延迟构建的:

```
problem          avgt     403.000 us/op
linked-list      avgt         541 us/opinsert-last-pos  avgt         281 us/opcopy-swap-array  avgt         254 us/op **view             avgt       0,002 us/op**
```

当我们需要处理`list.size() - 1 - index`时，调用`get()`会有很小的开销。在现实生活中，可能值得考虑如何使用这个列表。如果访问非常频繁，也许复制一份是最好的解决方案。

最后要提一件事。一些人倾向于强调函数式编程和不变性将**总是**对应用程序的性能产生负面影响。这种说法不能一概而论。

实际上，在我们的例子中，最佳解决方案依赖于问题本身的**约束**。

**进一步学习**
[10 本书准备技术编程/编码求职面试](http://www.java67.com/2017/06/10-books-to-prepare-technical-coding-job-interviews.html)
[10 本算法书每个程序员都应该读的](http://www.java67.com/2015/09/top-10-algorithm-books-every-programmer-read-learn.html)
[Java 开发人员的前 5 本数据结构和算法书](http://javarevisited.blogspot.sg/2016/05/5-free-data-structure-and-algorithm-books-in-java.html#axzz4uXETWjmV)
[从 0 到 1:Java 中的数据结构&算法](https://www.java67.com/2019/07/top-10-online-courses-to-learn-data-structure-and-algorithms-in-java.html)
[数据结构和算法分析— —求职面试](https://click.linksynergy.com/fs-bin/click?id=JVFxdTr9V80&subid=0&offerid=323058.1&type=10&tmpid=14538&RD_PARM1=https%3A%2F%2Fwww.udemy.com%2Fdata-structure-and-algorithms-analysis%2F)
[20+面试中基于字符串的编码问题](https://dev.to/javinpaul/top-20-string-coding-problems-from-programming-job-interviews-493m)

</javarevisited/top-21-string-programming-interview-questions-for-beginners-and-experienced-developers-56037048de45>  ![](img/b6578984dc018c44134738b9e16e76e8.png)![](img/ec9b9710329661db6caf375f69308521.png)

在 Twitter 上关注我 [@teivah](https://twitter.com/teivah)