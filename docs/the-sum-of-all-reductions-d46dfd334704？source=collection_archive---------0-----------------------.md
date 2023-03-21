# 所有缩减的总和

> 原文：<https://medium.com/javarevisited/the-sum-of-all-reductions-d46dfd334704?source=collection_archive---------0----------------------->

所有总和的减少

![](img/913f6bf15d30bf11a6ce5e436f4f169a.png)

新泽西州贝尔马，2018 年深度冰冻期间

在 2018 年的深冻期间，世间万物仿佛都化为冰雪，包括天空。就连太阳也似乎缩小了，因为它颤抖着从我的相机旁跑开了。

这让我想到了我们在 Java 中可用的不同种类的缩减，包括 Eclipse 集合和 Java 流。我想知道我们可以用多少种方法来定义`sum`。

## 对整数数组求和

让我们考虑几种在 Java 中对 int 数组中的值求和的方法。

这是我们将使用的数据。

```
int[] ints = {1, 2, 3, 4, 5};

// This supplier will generate an IntStream on demand
IntStream intStream = Arrays.stream(ints);

// This creates an IntList from Eclipse Collections
IntList intList = IntLists.immutable.with(ints);
```

**为循环**

```
int sumForLoop = 0;
for (int i = 0; i < ints.length; i++)
{
    sumForLoop += ints[i];
}
Assertions.assertEquals(15, sumForLoop);
```

**forEach**(`IntStream`)/**each**(`[IntList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/primitive/IntList.html)`

```
// sumForEach will be effectively final
int[] sumForEach = {0};
intStream.forEach(e -> sumForEach[0] += e);

Assertions.assertEquals(15, sumForEach[0]);

// sumEach will be effectively final
int[] sumEach = {0};
intList.each(e -> sumEach[0] += e);

Assertions.assertEquals(15, sumEach[0]);
```

**注射** ( `[IntList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/primitive/IntList.html)`)

```
// injectInto boxes on IntList as there is no primitive version
int sumInject = 
    intList.injectInto(Integer.valueOf(0), Integer::sum).intValue();

Assert.assertEquals(15, sumInject);
```

**减少** ( `IntStream`)

```
// reduce does not box on IntStream
int sumReduce = 
    intStream.reduce(Integer::sum).getAsInt();

Assertions.assertEquals(15, sumReduce);
```

**总和** ( `IntStream` / `[IntList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/primitive/IntList.html)`)

```
int sum1 = intStream.sum();

Assertions .assertEquals(15, sum1);

long sum2 = intList.sum();

Assertions.assertEquals(15, sum2);
```

显然，`IntStream`和`IntList`上可用的`sum`方法是最简单的解决方案。与`IntList`的微小区别是结果被扩大为`long`，这意味着您可以添加非常大的`int`值而不会溢出。

## 汇总整数数组

当我们使用 Java 8 中添加的`IntSummaryStatistics`类进行汇总时，我们得到的是同时计算的`count`、`sum`、`min`、`max`和`average`。这样可以避免多次迭代。我们将使用和以前一样的数据。

**为循环**

```
IntSummaryStatistics statsForLoop = new IntSummaryStatistics();
for (int i = 0; i < ints.length; i++)
{
    statsForLoop.accept(ints[i]);
}

Assertions.assertEquals(15, statsForLoop.getSum());
Assertions.assertEquals(1, statsForLoop.getMin());
Assertions.assertEquals(5, statsForLoop.getMax());
```

**forEach**(`IntStream`)/**each**(`[IntList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/primitive/IntList.html)`)

```
IntSummaryStatistics statsForEach = new IntSummaryStatistics();
intStream.forEach(statsForEach::accept);

Assertions.assertEquals(15, statsForEach.getSum());
Assertions.assertEquals(1, statsForEach.getMin());
Assertions.assertEquals(5, statsForEach.getMax());

IntSummaryStatistics statsEach = new IntSummaryStatistics();
intList.each(statsEach::accept);

Assertions.assertEquals(15, statsEach.getSum());
Assertions.assertEquals(1, statsEach.getMin());
Assertions.assertEquals(5, statsEach.getMax());
```

**注射液** ( `[IntList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/primitive/IntList.html)`

```
IntSummaryStatistics statsInject =
        intList.injectInto(
            new IntSummaryStatistics(), 
            (iss, each) -> {iss.accept(each); return iss;});

Assertions.assertEquals(15, statsInject.getSum());
Assertions.assertEquals(1, statsInject.getMin());
Assertions.assertEquals(5, statsInject.getMax());
```

**收藏** ( `IntStream`)

```
IntSummaryStatistics statsCollect =
        intStream.collect(
            IntSummaryStatistics::new, 
            IntSummaryStatistics::accept, 
            IntSummaryStatistics::combine);

Assertions.assertEquals(15, statsCollect.getSum());
Assertions.assertEquals(1, statsCollect.getMin());
Assertions.assertEquals(5, statsCollect.getMax());
```

注意:我不能使用`reduce`，因为两个参数必须是同一类型。我不得不使用`collect`来代替，这是一个可变的归约。原语流上的`collect`方法不采用`Collector`，而是采用`Supplier`、`ObjectIntConsumer`(累加器)和`BiConsumer`(合并器)。

**汇总统计** ( `IntStream` / `[IntList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/primitive/IntList.html)`)

```
IntSummaryStatistics stats1 = intStream.summaryStatistics();

Assertions.assertEquals(15, stats1.getSum());
Assertions.assertEquals(1, stats1.getMin());
Assertions.assertEquals(5, stats1.getMax());

IntSummaryStatistics stats2 = intList.summaryStatistics();

Assertions.assertEquals(15, stats2.getSum());
Assertions.assertEquals(1, stats2.getMin());
Assertions.assertEquals(5, stats2.getMax());
```

同样，`summaryStatistics`方法是最简单的解决方案。

## 对字符串数组的长度求和

假设我们想对一个数组中的字符串长度求和。这种方法可以用来对一个对象的任何 int 属性求和。

这是我们将使用的数据。

```
String[] words = {"The", "Quick", "Brown", "Fox", "jumps", "over", "the", 
    "lazy", "dog"};

Stream<String> stream = Stream.of(words);

ImmutableList<String> list = Lists.immutable.with(words);
```

**为**循环**为**

```
int sumForLoop = 0;
for (int i = 0; i < words.length; i++)
{
    sumForLoop += words[i].length();
}

Assertions.assertEquals(35, sumForLoop);
```

**为**每个**为** ( `Stream` ) / **每个** ( `[ImmutableList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/ImmutableList.html)`)

```
int[] sumForEach = {0};
stream.forEach(e -> sumForEach[0] += e.length());

Assertions.assertEquals(35, sumForEach[0]);

int[] sumEach = {0};
list.each(e -> sumEach[0] += e.length());

Assertions.assertEquals(35, sumEach[0]);
```

**采集剂** ( `[ImmutableList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/ImmutableList.html)` )+ **注射液** ( `[IntList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/primitive/IntList.html)`)

```
int sumInject = list
        .collectInt(String::length)
        .injectInto(Integer.valueOf(0), Integer::sum)
        .intValue();

Assertions.assertEquals(35, sumInject);
```

**收** ( `Stream` ) + **减** ( `Collectors`)

```
int sumReducing = 
    stream.collect(Collectors.reducing(0,
                String::length,
                Integer::sum)).intValue();

Assertions.assertEquals(35, sumReduce);
```

**mapToInt**(`Stream`)+**Reduce**(`IntStream`

```
int sumReduce = stream
        .mapToInt(String::length)
        .reduce(Integer::sum)
        .getAsInt();

Assertions.assertEquals(35, sumReduce);
```

**映射输入** ( `Stream` ) + **总和** ( `IntStream`)

```
int sum1 = stream
        .mapToInt(String::length)
        .sum();

Assertions.assertEquals(35, sum1);
```

**集合** ( `[ImmutableList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/ImmutableList.html)` ) + **总和** ( `[IntList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/primitive/IntList.html)`)

```
long sum2 = list
        .collectInt(String::length)
        .sum();

Assertions.assertEquals(35, sum2);
```

**收集** ( `Stream` ) + **总和** ( `Collectors`)

```
Integer summingInt = stream
    .collect(Collectors.summingInt(String::length));

Assertions.assertEquals(35, summingInt.intValue());
```

**sumOfInt** ( `[ImmutableList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/ImmutableList.html)`)

```
long sumOfInt = list.sumOfInt(String::length);

Assertions.assertEquals(35, sumOfInt);
```

我认为在这些例子中，`sumOfInt`是最简单的解决方案。

## 对按第一个字符分组的字符串的长度求和

在这个问题中，我们将根据字符串的第一个字符和每个字符的长度对字符串进行分组。如果可能的话，我更喜欢在这里使用原始贴图进行分组。

这是数据。

```
String[] words = {"The", "Quick", "Brown", "Fox", "jumps", "over", "the", 
    "lazy", "dog"};

Stream<String> stream = Stream.of(words).map(String::toLowerCase);

ImmutableList<String> list =
    Lists.immutable.with(words).collect(String::toLowerCase);
```

分别使用`map`和`collect`将`Stream`和`ImmutableList`字符串转换成小写。在 for 循环示例中，我们将手动执行此操作。

**为**循环**为**

```
MutableCharIntMap sumByForLoop = new CharIntHashMap();
for (int i = 0; i < words.length; i++)
{
    String word = words[i].toLowerCase();
    sumByForLoop.addToValue(word.charAt(0), word.length());
}

Assertions.assertEquals(35, sumByForLoop.values().sum());
Assertions.assertEquals(6, sumByForLoop.get('t'));
```

**为**每个**为** ( `Stream` ) / **每个** ( `[ImmutableList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/ImmutableList.html)`)

```
MutableCharIntMap sumByForEach = new CharIntHashMap();
stream.forEach(e -> sumByForEach.addToValue(e.charAt(0), e.length()));

Assertions.assertEquals(35, sumByForEach.values().sum());
Assertions.assertEquals(6, sumByForEach.get('t'));

MutableCharIntMap sumByEach = new CharIntHashMap();
list.each(e -> sumByEach.addToValue(e.charAt(0), e.length()));

Assertions.assertEquals(35, sumByEach.values().sum());
Assertions.assertEquals(6, sumByEach.get('t'));
```

**注入** ( `[ImmutableList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/primitive/ImmutableIntList.html)`)

```
MutableCharIntMap sumByInject =
        list.injectInto(
                new CharIntHashMap(),
                (map, each) -> {
                    map.addToValue(each.charAt(0), each.length());
                    return map;
                });

Assertions.assertEquals(35, sumByInject.values().sum());
Assertions.assertEquals(6, sumByInject.get('t'));
```

**减少** ( `Stream`)

```
MutableCharIntMap sumByReduce = stream
        .reduce(
                new CharIntHashMap(),
                (map, e) -> {
                    map.addToValue(e.charAt(0), e.length());
                    return map;
                },
                (map1, map2) -> {
                    map1.putAll(map2);
                    return map1;
                });

Assertions.assertEquals(35, sumByReduce.values().sum());
Assertions.assertEquals(6, sumByReduce.get('t'));
```

**聚合者** ( `[ImmutableList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/ImmutableList.html)`)

```
ImmutableMap<Character, Long> aggregateBy = list.aggregateBy(
        word -> word.charAt(0),
        () -> new Long(0),
        (sum, each) -> sum + each.length());

Assertions.assertEquals(35,
    aggregateBy.valuesView().sumOfLong(Long::longValue));
Assertions.assertEquals(6, aggregateBy.get('t').longValue());
```

**聚集地点由** ( `[ImmutableList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/ImmutableList.html)`)

```
ImmutableMap<Character, LongAdder> aggregateInPlaceBy = 
        list.aggregateInPlaceBy(
                word -> word.charAt(0),
                LongAdder::new,
                (adder, each) -> adder.add(each.length()));

Assertions.assertEquals(35,
    aggregateInPlaceBy.valuesView().sumOfLong(LongAdder::longValue));
Assertions.assertEquals(6, aggregateInPlaceBy.get('t').longValue());
```

**收藏** ( `Stream`)

```
MutableCharIntMap sumByCollect = stream.collect(
        CharIntHashMap::new,
        (map, e) -> map.addToValue(e.charAt(0), e.length()),
        CharIntHashMap::putAll);

Assertions.assertEquals(35, sumByCollect.values().sum());
Assertions.assertEquals(6, sumByCollect.get('t'));
```

**收集** ( `Stream` ) + **分组，按** ( `Collectors` ) + **求和** ( `Collectors`)

```
Map<Character, Integer> sumByCollectSummingInt =
        stream.collect(Collectors.groupingBy(
                        word -> word.charAt(0),
                        Collectors.summingInt(String::length)));
Assertions.assertEquals(35,
    sumByCollectSummingInt.values()
        .stream().mapToInt(Integer::intValue).sum());
Assertions.assertEquals(Integer.valueO(6), sumByCollectSummingInt.get('t'));
```

**收藏**(`Stream`)+**sumByInt**(`[Collectors2](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/impl/collector/Collectors2.html)`)

```
ObjectLongMap<Character> sumByCollectors2 =
        stream.collect(
                Collectors2.sumByInt(
                    word -> word.charAt(0), String::length));

Assertions.assertEquals(35, sumByCollectors2.values().sum());
Assertions.assertEquals(6, sumByCollectors2.get('t'));
```

**reduce in place**(`[ImmutableList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/ImmutableList.html)`)+**sumByInt**(`[Collectors2](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/impl/collector/Collectors2.html)`)

```
ObjectLongMap<Character> reduceInPlaceCollectors2 =
        list.reduceInPlace(
                Collectors2.sumByInt(
                    e -> e.charAt(0), String::length));

Assertions.assertEquals(35, reduceInPlaceCollectors2.values().sum());
Assertions.assertEquals(6, reduceInPlaceCollectors2.get('t'));
```

**sumByInt** ( `[ImmutableList](https://www.eclipse.org/collections/javadoc/11.1.0/org/eclipse/collections/api/list/ImmutableList.html)`)

```
ObjectLongMap<Character> sumByInt =
        list.sumByInt(e -> e.charAt(0), String::length);

Assertions.assertEquals(35, sumByInt.values().sum());
Assertions.assertEquals(6, sumByInt.get('t'));
```

这里最简单的解决方法是`sumByInt`。

# 结论

我们已经介绍了许多不同的方法，您可以使用 Java 和 Eclipse 集合来获取`sum`或`summarize`值。在求和的情况下，使用名称中带有 sum 的方法可能会给出最简单的解决方案。使用像`injectInto`和`reduceInPlace` (Eclipse 集合)或 collect (Java 流)这样的方法，几乎可以解决任何问题。当你的结果需要不同于你的输入时，像`reduce`这样的方法就没那么有用了。像`aggregateBy`和`aggregateInPlaceBy`这样的方法给你一个比`collect`更具体的结果，因为它们总是返回一个`Map`。如果您想迭代一个`Stream`并使用`collect`轻松获得原始地图结果，使用`Collectors2`会很有帮助。

*我是*[*Eclipse Foundation*](https://projects.eclipse.org/projects/technology.collections)*的*[*Eclipse Collections*](https://github.com/eclipse/eclipse-collections)*OSS 项目的项目负责人。* [*月食收藏*](https://github.com/eclipse/eclipse-collections) *为* [*投稿*](https://github.com/eclipse/eclipse-collections/blob/master/CONTRIBUTING.md) *打开。如果你喜欢这个库，你可以在 GitHub 上让我们知道。*