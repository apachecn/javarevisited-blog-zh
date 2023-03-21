# Java 高效随机数生成器 Random VS ThreadLocalRandom

> 原文：<https://medium.com/javarevisited/java-efficient-random-number-generator-random-vs-threadlocalrandom-147b57aa87ac?source=collection_archive---------2----------------------->

![](img/af76145b9f57a243c89d570650732d80.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) 拍摄

如果需要生成随机数，你大多会选择使用 [Random](https://javarevisited.blogspot.com/2013/05/how-to-generate-random-numbers-in-java-between-range.html) 来实现。虽然它内部使用 CAS 来实现，但是在多线程并发的情况下，性能并不是很好。JDK1.7 之后，JDK 提供了更好的解决方案[threadlocalrandan](https://www.java67.com/2020/11/randam-vs-threadlocalrandom-vs.html)。

# 1.随意