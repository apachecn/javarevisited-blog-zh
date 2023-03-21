# ConnectTimeout 和 SocketTimeout 有什么区别

> 原文：<https://medium.com/javarevisited/what-is-the-difference-between-connecttimeout-and-sockettimeout-c2e9a0c89c60?source=collection_archive---------0----------------------->

![](img/ff094fef92697b16b7bc2d65568479c2.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) 拍摄

有时，由于业务的复杂性，在 JVM 中组装一些数据会造成资源的极大浪费。例如，从 [MySQL](/@javinpaul/top-5-courses-to-learn-mysql-in-2020-4ffada70656f) 中查询一个列表，然后在代码中循环通过数据库来填充一些字段。

这种数据组装方法，除了执行效率的问题之外，通常还会占用更多的内存，这就使得