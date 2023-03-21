# 开发人员必须避免的 3 个线程池陷阱

> 原文：<https://medium.com/javarevisited/3-thread-pool-pitfalls-you-must-avoid-as-a-developer-f59d4832de95?source=collection_archive---------2----------------------->

![](img/307707c1b1722d506c54cf1cd14cff09.png)

1.  **线程池中创建的线程过多，导致 OOM**

对于我的大多数朋友来说，他们都思考过一个问题。如果由[执行器组件](https://javarevisited.blogspot.com/2013/07/how-to-create-thread-pools-in-java-executors-framework-example-tutorial.html) t 创建的线程池 newFixedThreadPool 使用了一个无限队列，可能会导致 OOM。那么，Executors 组件还可以创建其他线程池，比如 newCachedThreadPool，这样可行吗？
在这里，我们需要…