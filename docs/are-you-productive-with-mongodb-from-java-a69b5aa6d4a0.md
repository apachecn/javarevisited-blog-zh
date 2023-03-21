# 你用 Java 的 MongoDB 有效率吗？

> 原文：<https://medium.com/javarevisited/are-you-productive-with-mongodb-from-java-a69b5aa6d4a0?source=collection_archive---------2----------------------->

MongoDB 做得很好，在[的 db-engines](https://db-engines.com/en/ranking) 排名网站上稳坐第五名。这意味着我们开发人员正在大量使用它。

当查看官方的[驱动 API](https://mongodb.github.io/mongo-java-driver/3.11/driver/tutorials/perform-read-operations/#filters-helper) 时，我们可以看到一个尽可能流畅的 API 的努力:

```
collection.find(and(gte(**“stars”**, 2), lt(**“stars”**, 5), eq(**“categories”**, **“Bakery”**)));
```

然而，还有许多问题:

*   所有字段都是没有自动完成的字符串。
*   操作员没有类型安全(是数字吗？！`categories`是集合(？！)的字符串还是…？！).
*   我们真的喜欢用`gte`代替`>=`吗？！我的意思是，这种表达方式容易阅读并且*理解*吗？
*   最后，当模式改变时，我们又回到 80 '的查找-替换…

幸运的是，对于这些问题有一个简单而有效的解决方案——FluentMongo，它为 API 添加了缺失的成分——类型安全和 Java 集成。它允许你使用普通 Java 编写过滤器、投影、更新、分类和索引。例如:

```
collection.find(builder.filter(r -> r.getStars() >= 2 && r.getStars() < 5 && r.getCategories().contains("Bakery")));
```

现在你可以用 [MongoDB](https://javarevisited.blogspot.com/2019/01/top-5-mongodb-online-training-courses.html) 同时拥有 [**生产力**](https://github.com/streamx-co/FluentMongo) 和编写**可维护的**代码！

![](img/11415e33ca3e5d7ff175366fb7c889a1.png)

其他**编程篇**你可能喜欢的
[2019 年 Java 程序员应该学会的 10 件事](https://javarevisited.blogspot.com/2017/12/10-things-java-programmers-should-learn.html#axzz5atl0BngO)
[2019 年你可以学会的 10 种编程语言](http://www.java67.com/2017/12/10-programming-languages-to-learn-in.html)
[每个 Java 开发者都应该知道的 10 个工具](http://www.java67.com/2018/04/10-tools-java-developers-should-learn.html)
[学习 Java 编程语言的 10 个理由](http://javarevisited.blogspot.sg/2013/04/10-reasons-to-learn-java-programming.html)
[2019 年 Java 和 Web 开发者应该学会的 10 个框架](http://javarevisited.blogspot.sg/2018/01/10-frameworks-java-and-web-developers-should-learn.html)
[成为更好的 Java 开发者的 10 个小技巧 2019 年要学的框架](http://javarevisited.blogspot.sg/2018/05/10-tips-to-become-better-java-developer.html)
[2019 年要学 Python 的 10 个理由](https://javarevisited.blogspot.com/2018/05/10-reasons-to-learn-python-programming.html)
[每个 Java 开发者都应该知道的 10 个测试库](https://javarevisited.blogspot.sg/2018/01/10-unit-testing-and-integration-tools-for-java-programmers.html)

<https://javarevisited.blogspot.com/2019/10/the-java-developer-roadmap.html> 