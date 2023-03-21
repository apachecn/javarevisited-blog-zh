# 避免 Java 中方法引用的这种错误用法

> 原文：<https://medium.com/javarevisited/a-case-of-a-wrong-method-reference-6705c299a7ce?source=collection_archive---------5----------------------->

![](img/af5145b11fc9585e1df7e9b6f4f16068.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 拍摄的照片

看看这个。

```
// returns an array of given length with random values.
int[] randomArray(@Min(1) final int length) {
    return range(0, length).map(current()::nextInt).toArray();
}
```

当我看到下面的堆栈跟踪时，我简直惊呆了。

```
java.lang.IllegalArgumentException: bound must be positive
at java.base/java.util.concurrent.ThreadLocalRandom.nextInt(ThreadLocalRandom.java:310)
...
```

那么，怎么了？

最初的意图是用[threadlocalrrandom # nextInt()](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadLocalRandom.html#nextInt--)映射每个索引。但是，通过使用方法引用，它实际上调用了[threadlocalrrandom # nextInt(int bound)](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadLocalRandom.html#nextInt-int-)。

代码实际上是这样工作的，

```
map(i -> current().nextInt(i))
```

我不得不像这样修理。

```
map(i -> current().nextInt())
```