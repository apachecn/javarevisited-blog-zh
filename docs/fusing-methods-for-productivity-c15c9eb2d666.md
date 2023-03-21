# 提高生产率的融合方法

> 原文：<https://medium.com/javarevisited/fusing-methods-for-productivity-c15c9eb2d666?source=collection_archive---------3----------------------->

当一个迭代模式如此普遍时，你给它一个名字。

![](img/27064ff54caeb9c81abc644fde172b28.png)

欢迎来到天空湾，这里的海湾与天空融合在一起，用码头做拉链

# 当任何满足都不满足时

有时，您希望查看集合中是否包含与某个属性值相匹配的元素。在下面的例子中，我使用来自 [Eclipse 集合](https://github.com/eclipse/eclipse-collections)的方法`anySatisfy`来查看是否有任何对在`getTwo`属性中具有“2”或“3”。

```
@Test
public void anySatisfyContains()
{
    MutableList<Pair<Integer, String>> list =
            Lists.***mutable***.with(
                    Tuples.*pair*(1, **"1"**), 
                    Tuples.*pair*(2, **"2"**));

    Assert.*assertTrue*(
            list.anySatisfy(pair -> pair.getTwo().equals(**"2"**)));
    Assert.*assertFalse*(
            list.anySatisfy(pair -> pair.getTwo().equals(**"3"**)));
}
```

我们可以使用将`Function`指定为方法引用的`collect`方法来提取`pair.getTwo()`的值。

```
@Test
public void eagerCollectAnySatisfy()
{
    MutableList<Pair<Integer, String>> list =
            Lists.***mutable***.with(
                    Tuples.*pair*(1, **"1"**),
                    Tuples.*pair*(2, **"2"**));

    Assert.*assertTrue*(
            list.collect(Pair::getTwo)
                    .anySatisfy(each -> each.equals(**"2"**)));
    Assert.*assertFalse*(
            list.collect(Pair::getTwo)
                    .anySatisfy(each -> each.equals(**"3"**)));
}
```

不幸的是，这是更多的代码，并且在调用`collect`之后创建了一个临时的`List`。方法`anySatisfy`用于等式测试时，可简化为`contains`。我就用`contains`代替`anySatisfy`。

```
@Test
public void eagerCollectContains()
{
    MutableList<Pair<Integer, String>> list =
            Lists.***mutable***.with(
                    Tuples.*pair*(1, **"1"**),
                    Tuples.*pair*(2, **"2"**));

    Assert.*assertTrue*(
            list.collect(Pair::getTwo).contains(**"2"**));
    Assert.*assertFalse*(
            list.collect(Pair::getTwo).contains(**"3"**));
}
```

现在代码更加简洁易读，但是我们仍然有额外的临时`List`创建要删除。这可以通过添加对`asLazy`的调用来解决。

```
@Test
public void lazyCollectContains()
{
    MutableList<Pair<Integer, String>> list =
            Lists.***mutable***.with(
                    Tuples.*pair*(1, **"1"**),
                    Tuples.*pair*(2, **"2"**));

    Assert.*assertTrue*(
            list.asLazy()
                    .collect(Pair::getTwo)
                    .contains(**"2"**));
    Assert.*assertFalse*(
            list.asLazy()
                    .collect(Pair::getTwo)
                    .contains(**"3"**));
}
```

现在我们有了一个可读的解决方案，它不会产生太多垃圾，但有三个方法调用来查看集合是否包含具有等于特定值的属性的元素。

我们可以做得更好。

# 融合方法

让我们给`[RichIterable](https://www.eclipse.org/collections/javadoc/10.2.0/org/eclipse/collections/api/RichIterable.html)`添加一个默认方法来提供这个功能。

但是首先，让我们写一个测试。

```
@Test
public void collectContains()
{
    MutableList<Pair<Integer, String>> list =
            Lists.***mutable***.with(
                    Tuples.*pair*(1, **"1"**),
                    Tuples.*pair*(2, **"2"**),
                    Tuples.*pair*(3, null));

    Assert.*assertTrue*(
            list.collectContains(Pair::getTwo, **"2"**));
    Assert.*assertFalse*(
            list.collectContains(Pair::getTwo, **"3"**));
    Assert.*assertTrue*(
            list.collectContains(Pair::getTwo, null));
    Assert.*assertFalse*(
            list.collectContains(Pair::getOne, null));
}
```

我们想要融合`collect`和`contains`方法，所以我一开始就称之为`collectContains`。我还添加了一些测试，以确保我们可以与`null`进行比较。

`RichIterable`上的方法`collectConains`将是急切的，因为它返回一个`boolean`。我将使用`anySatisfy`实现该方法，这也是一个返回`boolean`的方法。

```
default <V> boolean collectContains(
        Function<? super T, ? extends V> function,
        V value)
{
    if (null == value)
    {
        return this.anySatisfy(
                each -> null == *function*.valueOf(each));
    }
    return this.anySatisfy(
            each -> *value*.equals(*function*.valueOf(each)));
}
```

这是我对支持`null`值的实现的初步尝试。测试通过了。赢了！

让我看看使用三元运算符是什么样子的。

```
default <V> boolean collectContains(
        Function<? super T, ? extends V> function,
        V value)
{
        return this.anySatisfy(null == value ?
                each -> null == *function*.valueOf(each) :
                each -> *value*.equals(*function*.valueOf(each)));
}
```

测试通过。赢了！

我更喜欢三元运算符方法，但通过将`Predicate`提取到一个变量中，会使它在下一次迭代中更容易解析。

**更新:**另一位 Eclipse Collections 的贡献者([弗拉基米尔·扎哈罗夫](https://medium.com/u/7db07b72520d?source=post_page-----c15c9eb2d666--------------------------------))阅读了博客的最初版本，并建议对`collectContains`方法可能有一个更好的名字，基于我们以前对采用`Function`的融合方法使用的类似模式。如果我们添加`By`作为方法的后缀，去掉`collect`前缀，我们将得到`containsBy`。

**2020 年 7 月 23 日更新**:我今天想到了一个更简单的解决方案，更新如下。

```
default <V> boolean containsBy(
        Function<? super T, ? extends V> function,
        V value)
{
    Objects.*requireNonNull*(function);
    return this.anySatisfy(
            each -> Objects.*equals*(*value*, *function*.valueOf(each)));
}
```

这将导致测试中的代码如下所示。

```
@Test
public void containsBy()
{
    MutableList<Pair<Integer, String>> list =
            Lists.***mutable***.with(
                    Tuples.*pair*(1, **"1"**),
                    Tuples.*pair*(2, **"2"**),
                    Tuples.*pair*(3, null));

    Assert.*assertTrue*(
            list.containsBy(Pair::getTwo, **"2"**));
    Assert.*assertFalse*(
            list.containsBy(Pair::getTwo, **"3"**));
    Assert.*assertTrue*(
            list.containsBy(Pair::getTwo, null));
    Assert.*assertFalse*(
            list.containsBy(Pair::getOne, null));
}
```

我觉得这样好多了。谢谢你的建议[弗拉基米尔扎哈罗夫](https://medium.com/u/7db07b72520d?source=post_page-----c15c9eb2d666--------------------------------)！

带有`By`后缀并以`Function`作为参数的其他方法的例子有`aggregateBy`、`groupBy`、`countBy`、`minBy`、`maxBy`等。几年前，我写了一篇博客，谈论介词`By`带来的生产率提高。

[](/javarevisited/by-yourself-some-time-e16c0f488847) [## 一个人呆一会儿

### 介词 By，及其对生产力的积极贡献。

medium.com](/javarevisited/by-yourself-some-time-e16c0f488847) 

# 那么该不该加这个方法呢？

我认为我们应该将`containsBy`添加到 [Eclipse 集合](https://github.com/eclipse/eclipse-collections) `[RichIterable](https://www.eclipse.org/collections/javadoc/10.2.0/org/eclipse/collections/api/RichIterable.html)`类型中。这是一种常见的模式，其好处将大大超过实施和支持成本。除了添加一些额外的测试之外，没有什么需要做的，所以我可能很快就会提交一个 pull 请求。

# 我们能把这个加入 Java 流吗？

答案既是“当然”，也是“看情况”。我不认为我们今天可以在`Stream`上调用`contains`，因为它不存在。你自己看看吧。您当然可以使用`anyMatch`实现相同的代码。最大的问题是，这种方法是否被认为有足够高的价值来增加 JDK。方法名可以是`mapContains`或`containsBy`，也可以简单到将这个默认方法添加到流中。

```
default <V> boolean containsBy(
        Function<? super T, ? extends V> function,
        V value)
{
    Objects.*requireNonNull*(function);
    return this.anyMatch(
            each -> Objects.*equals*(*value*, *function*.apply(each)));
}
```

这可能是添加到 Java `Stream`中的更有价值的方法，而不仅仅是`contains`本身。

你怎么想呢?

*我是*[*Eclipse Collections*](https://github.com/eclipse/eclipse-collections)*OSS 项目在*[*Eclipse Foundation*](https://projects.eclipse.org/projects/technology.collections)*的项目负责人。* [*月食收藏*](https://github.com/eclipse/eclipse-collections) *开作* [*投稿*](https://github.com/eclipse/eclipse-collections/blob/master/CONTRIBUTING.md) *。如果你喜欢这个库，你可以在 GitHub 上让我们知道。*