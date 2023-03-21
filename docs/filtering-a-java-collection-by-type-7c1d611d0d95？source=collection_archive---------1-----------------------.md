# 按类型过滤 Java 集合

> 原文：<https://medium.com/javarevisited/filtering-a-java-collection-by-type-7c1d611d0d95?source=collection_archive---------1----------------------->

了解如何在混合的 Java 集合中过滤特定的类型

![](img/7b3e2a4abd4286a893a15af303d15dd3.png)

亚历克斯·钱伯斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 问题是

有时你在 Java 中有一个混合的集合。一个简单的例子是拥有一个`List<Number>`，其中列表可能包含`Integer`、`Float`、`Long`和`Double`实例。如何轻松过滤掉`List<Integer>`或者`List<Double>`？我将演示如何用传统的 Java 实现这一点，在 Java 16 中使用 instanceof 的模式匹配，并使用两种不同的方法处理 [Eclipse 集合](https://github.com/eclipse/eclipse-collections)。我还在 Switch 中添加了一个使用 JDK 17 EA 和模式匹配的预览解决方案。

# 解决方案:经典 Java

这是使用标准 Java foreach 循环解决问题的一种方法。

```
@Test
public void classicJava()
{
    List<Number> numbers = List.*of*(1, 2L, 3.0, 4.0f);
    List<Integer> integers = new ArrayList<>();
    List<Long> longs = new ArrayList<>();
    List<Double> doubles = new ArrayList<>();
    List<Float> floats = new ArrayList<>();
    for (Number each : numbers)
    {
        if (each instanceof Integer)
        {
            integers.add((Integer) each);
        }
        else if (each instanceof Long)
        {
            longs.add((Long) each);
        }
        else if (each instanceof Double)
        {
            doubles.add((Double) each);
        }
        else if (each instanceof Float)
        {
            floats.add((Float) each);
        }
    }

    Assertions.*assertEquals*(List.*of*(1), integers);
    Assertions.*assertEquals*(List.*of*(2L), longs);
    Assertions.*assertEquals*(List.*of*(3.0), doubles);
    Assertions.*assertEquals*(List.*of*(4.0f), floats);
```

这种方法的好处是我们只对数字列表迭代一次。缺点是它非常冗长，并且需要强制转换。

# 解决方案:Java 16 —实例的模式匹配

另一个解决问题的方法是在 Java 16 中使用 instanceof 的模式匹配。

```
@Test
public void patternMatchingForInstanceOfJava16()
{
    List<Number> numbers = List.*of*(1, 2L, 3.0, 4.0f);
    List<Integer> integers = new ArrayList<>();
    List<Long> longs = new ArrayList<>();
    List<Double> doubles = new ArrayList<>();
    List<Float> floats = new ArrayList<>();
    numbers.forEach(number ->
    {
        if (number instanceof Integer each)
        {
            *integers*.add(each);
        }
        else if (number instanceof Long each)
        {
            *longs*.add(each);
        }
        else if (number instanceof Float each)
        {
            *floats*.add(each);
        }
        else if (number instanceof Double each)
        {
            *doubles*.add(each);
        }
    });
    Assertions.*assertEquals*(List.*of*(1), integers);
    Assertions.*assertEquals*(List.*of*(2L), longs);
    Assertions.*assertEquals*(List.*of*(3.0), doubles);
    Assertions.*assertEquals*(List.*of*(4.0f), floats);
}
```

这样做的好处是，我们只对数字列表迭代一次，没有不安全的类型转换。缺点是还是比较啰嗦。这将在 Java 17 中得到改进，为 switch 提供模式匹配。

# 解决方案:Eclipse 集合选择实例

这是使用 Eclipse 集合中的`selectInstanceOf`解决问题的另一种方法。

```
@Test
public void selectInstancesOf()
{
    MutableList<Number> numbers = 
        Lists.***mutable***.with(1, 2L, 3.0, 4.0f);
    MutableList<Integer> integers =
        numbers.selectInstancesOf(Integer.class);
    MutableList<Long> longs = 
        numbers.selectInstancesOf(Long.class);
    MutableList<Double> doubles =
        numbers.selectInstancesOf(Double.class);
    MutableList<Float> floats =
        numbers.selectInstancesOf(Float.class);

    Assertions.*assertEquals*(Lists.***mutable***.with(1), integers);
    Assertions.*assertEquals*(Lists.***mutable***.with(2L), longs);
    Assertions.*assertEquals*(Lists.***mutable***.with(3.0), doubles);
    Assertions.*assertEquals*(Lists.***mutable***.with(4.0f), floats);
}
```

这种方法的好处是非常简洁。缺点是它需要对数字列表进行多次迭代。

# 解决方案:Eclipse 集合 CaseProcedure

这就是如何使用 Eclipse 集合中的 CaseProcedure 来解决这个问题。

```
@Test
public void caseProcedure()
{
    MutableList<Number> numbers =
            Lists.***mutable***.with(1, 2L, 3.0, 4.0f);
    MutableList<Integer> integers = Lists.***mutable***.empty();
    MutableList<Long> longs = Lists.***mutable***.empty();
    MutableList<Double> doubles = Lists.***mutable***.empty();
    MutableList<Float> floats = Lists.***mutable***.empty();

    numbers.forEach(new CaseProcedure<>()
            .addCase(Integer.class::isInstance,
                    each -> *integers*.add((Integer) each))
            .addCase(Float.class::isInstance,
                    each -> *floats*.add((Float) each))
            .addCase(Long.class::isInstance,
                    each -> *longs*.add((Long) each))
            .addCase(Double.class::isInstance,
                    each -> *doubles*.add((Double) each)));

    Assertions.*assertEquals*(Lists.***mutable***.with(1), integers);
    Assertions.*assertEquals*(Lists.***mutable***.with(2L), longs);
    Assertions.*assertEquals*(Lists.***mutable***.with(3.0), doubles);
    Assertions.*assertEquals*(Lists.***mutable***.with(4.0f), floats);
}
```

这种方法的优点是相对简洁，并且只需要迭代一次数字列表。缺点是它需要显式和不安全的强制转换。

# 解决方案:Java 17—Switch 中的模式匹配

当 Java 17 发布后，我们可以在 switch 中使用模式匹配来解决这个问题。

```
@Test
public void patternMatchingForSwitchJava17()
{
    List<Number> numbers = List.*of*(1, 2L, 3.0, 4.0f);
    List<Integer> integers = new ArrayList<>();
    List<Long> longs = new ArrayList<>();
    List<Double> doubles = new ArrayList<>();
    List<Float> floats = new ArrayList<>();
    numbers.forEach(number -> {
            switch(number)
            {
                case Integer each -> *integers*.add(each);
                case Float each -> *floats*.add(each);
                case Long each -> *longs*.add(each);
                case Double each -> *doubles*.add(each);
                default -> { break; }
            };
    });
    Assertions.*assertEquals*(List.*of*(1), integers);
    Assertions.*assertEquals*(List.*of*(2L), longs);
    Assertions.*assertEquals*(List.*of*(3.0), doubles);
    Assertions.*assertEquals*(List.*of*(4.0f), floats);
}
```

这种方法的优点是简洁、清晰和单次迭代。我想不出有什么坏处。

# 实验:Java 17—Switch 中的记录、模式匹配、Var 和 Eclipse 集合注入

这纯粹是我拿到 Java 17 EA 安装工作后加的一个思想实验。我想看看在 Switch 中使用 Java 记录和 Eclipse 集合注入模式匹配能做些什么。然后我扔进去`var`只是因为。

```
@Test
public void recordsPatternMatchingInSwitchAndInjectInto()
{
    record Numbers(List<Integer> integers, List<Long> longs,
                   List<Double> doubles, List<Float> floats)
    {
        public Numbers()
        {
            this(*mList*(), *mList*(), *mList*(), *mList*());
        }

        Numbers filter(Number number)
        {
            switch(number)
            {
                case Integer each -> integers.add(each);
                case Float each -> floats.add(each);
                case Long each -> longs.add(each);
                case Double each -> doubles.add(each);
                default -> { break; }
            }
            return this;
        }
    }

    var numbers = Lists.***mutable***.with(1, 2L, 3.0, 4.0f);
    var filtered = 
        numbers.injectInto(new Numbers(), Numbers::filter);

    Assertions.*assertEquals*(List.*of*(1), filtered.integers);
    Assertions.*assertEquals*(List.*of*(2L), filtered.longs);
    Assertions.*assertEquals*(List.*of*(3.0), filtered.doubles);
    Assertions.*assertEquals*(List.*of*(4.0f), filtered.floats);
}
```

# 一些想法

基于类型过滤一个混合的 Java 集合可能是痛苦的，但是作为今天的 Java 开发人员，我们有一些选择。Eclipse Collections 给出了当今最简洁的选项。`selectInstancesOf`方法结合了过滤和更具体的返回类型，缺点是需要对每个子类型进行单独的迭代。`CaseProcedure`给出了当今最简洁、性能最好的选择，但缺点是仍然需要演员阵容。当 Java 17 在 Switch 中发布模式匹配时，可用的选项将真正得到改进。Java 17 在发布时将提供最简洁和性能最好的选择。

*我是*[*Eclipse Collections*](https://github.com/eclipse/eclipse-collections)*OSS 项目在*[*Eclipse Foundation*](https://projects.eclipse.org/projects/technology.collections)*的项目负责人和提交人。* [*月食收藏*](https://github.com/eclipse/eclipse-collections) *开作* [*投稿*](https://github.com/eclipse/eclipse-collections/blob/master/CONTRIBUTING.md) *。如果你喜欢这个库，你可以在 GitHub 上让我们知道。*

其他 **Java 编程文章**你可能喜欢:
[Java 程序员应该学习的 10 件事](/javarevisited/9-things-java-programmers-should-learn-in-2018-3f0b2207dfc4)
[你可以学习的 10 种编程语言](/hackernoon/10-best-programming-languages-to-learn-in-2019-e5b05af4a972)
[10 个工具每个 Java 开发者都应该知道的](http://www.java67.com/2018/04/10-tools-java-developers-should-learn.html)
[2021 年学习 Java 编程语言的 10 个理由](http://javarevisited.blogspot.sg/2013/04/10-reasons-to-learn-java-programming.html)
[完整的 Java 开发者路线图](/javarevisited/the-java-programmer-roadmap-f9db163ef2c2)
[我最喜欢的初学者免费编程课程](/javarevisited/top-10-free-interactive-programming-courses-from-educative-for-beginners-to-learn-in-2021-713cbf96d4eb)