# @lombok。数据和 MyBatis

> 原文：<https://medium.com/javarevisited/lombok-data-and-mybatis-f43bd8ba0c7?source=collection_archive---------4----------------------->

![](img/6200b00162b835632deaac357f2493bb.png)

由[马克西米利安·魏斯贝克尔](https://unsplash.com/@maximilianweisbecker?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

使用下面的结果类型，

```
@Data
public class Some {
    ...
    private List<Sting> keywords;
}
```

带有以下 ResultMap 的 MyBatis 抱怨创建结果。

```
<collection property="keywords" ofType="string" javaType="list">
  <result column="keyword"/>
</collection>
```

这是因为`@Data`注释作为`@RequiredArgsConstructor`工作，MyBatis 不能创建值。

这就是为什么我这样改变了这个类。

```
//@Data
@ToString
@Setter
@Gettter
```

其他**有趣的文章**你可能喜欢探讨的
[2019 年 Java 开发者应该学习的 10 件事](http://javarevisited.blogspot.sg/2017/12/10-things-java-programmers-should-learn.html#axzz53JaDYLsP)
[2019 年要探讨的 10 种编程语言](http://www.java67.com/2017/12/10-programming-languages-to-learn-in.html)
[2019 年 Java 和 Web 开发者应该学习的 10 种框架](http://javarevisited.blogspot.sg/2018/01/10-frameworks-java-and-web-developers-should-learn.html)
[2019 年你可以阅读的 20 本 Java 书籍](http://javarevisited.blogspot.sg/2017/12/top-20-java-books-of-2017-which-you-can-read-in-2018.html)
[10 本教程学习 Java 8 更好的](http://www.java67.com/2014/09/top-10-java-8-tutorials-best-of-lot.html)