# 为什么你应该选择使用 Java 的 4 个理由？

> 原文：<https://medium.com/javarevisited/4-reasons-why-you-should-use-java-optional-or-not-4e44d51a9645?source=collection_archive---------1----------------------->

![](img/1596f7601e70e286f217d978d80824e7.png)

由 [Pinterest](https://ro.pinterest.com/beerdom/) 上的[马克·比尔登](https://ro.pinterest.com/beerdom/)拍摄的照片

战斗很激烈。开发者们已经筋疲力尽了。

争论是双向的。每一方都有成堆的尸体躺在地上，被烧毁或倒塌。

围绕电子邮件、合并请求和 Zoom Calls 展开了战争，但没有明显的赢家。**让我们一劳永逸地解决可选之战吧！**

# 概观

**我们可以使用** [**可选类**](https://javarevisited.blogspot.com/2017/04/10-examples-of-optional-in-java-8.html) **包装我们的数据，避免经典的*空检查*和一些*试捕*块。**

因此，我们将能够链接方法调用，并拥有更流畅的功能性代码。

另一方面，滥用它会导致性能下降和代码混乱。

在本文中，我们将探索一些需要防范空值的常见用例。对于它们中的每一个，我们将决定是否可以使用[](https://www.java67.com/2018/06/java-8-optional-example-ispresent-orElse-get.html)*来简化我们的代码，或者我们是否应该坚持使用[经典的空检查](https://javarevisited.blogspot.com/2017/01/how-to-check-for-null-values-in-sql.html)。*

**PS:对于代码片段，我们将通过以下方法使用 AccountRepository 类:**

```
*public *Account* **get**(String username);

public *Optional<Account>* **find**(String username);*
```

## *1.可选的方法返回类型👍*

*使用 Optional 的一种方法是在返回之前包装数据——如果它可能为 null 的话。这种方式很快被[*springdata JPA*](/javarevisited/5-best-spring-data-jpa-courses-for-java-developers-45e6438be3c9)*等开发者和框架采用。**

*因此，调用者会意识到结果可能是空的。此外，这给了调用者一些灵活性:例如，如果可选的是空的，它允许他容易地抛出一个定制的异常。*

*此外，**如果我们需要对检索到的数据执行额外的检查，Optional 为它提供了一个很好的 API**。*

*让我们来看一些场景，并比较这两种解决方案:*

*我们可以注意到 if 语句的表达式越来越长，越来越难读。对于这些用例，我们可以利用可选 API 中的 [*过滤器*和*映射*方法](https://www.java67.com/2014/04/java-8-stream-examples-and-tutorial.html)。*

## *2.包装吸气剂👍*

*从上面的代码片段中我们可以注意到， *Account* 类公开了一个*getmembershipopoptional()*方法。*

```
*Optional<Membership> getMembershipOptional() {
    return Optional.*ofNullable*(membership);
}*
```

**当然，这不是实际的 getter 或者 membership 字段。但这只是因为我对所有的例子都使用了相同的数据模型。在一个真实的项目中，我们要么有经典的 getter，要么有返回可选的 getter，但不能两者都有。**

***从商业角度来看，让 getters 返回可选对于那些 *null* 是有效值的字段来说是一个很好的实践。***

*因为这将帮助我们丰富我们的领域模型，我们不应该把它应用到 DTO 对象。这样，我们也将避免潜在的[序列化问题。](https://www.java67.com/2020/05/15-java-serialization-interview-questions-answers.html)*

## *3.为非常简单的逻辑包装局部变量👎*

***在*可选的*中包装变量仅仅是为了利用其 API 进行简单的操作，这已经开始成为一种** [**常见的反模式。**](http://javarevisited.blogspot.sg/2015/10/what-is-double-brace-initialization-in-java-example-anti-pattern.html)*

*例如，使用 Optional just 来内联 if 语句:*

```
*Optional.*ofNullable*(account)
    .ifPresent(acc -> processAccount(acc));*
```

*这里使用*可选*并没有带来任何价值。在这种情况下，我们应该无耻地使用 [*经典的空值检查*](https://javarevisited.blogspot.com/2016/01/how-to-check-if-string-is-not-null-and-empty-in-java-example.html) :*

```
*if (account != null) {
    processAccount(account);
}*
```

*让我们再看一个不需要使用 Optional 的例子:*

```
*var accountType = Optional.*ofNullable*(account)
    .map(acc -> acc.getAccountType())
    .orElse(*DEFAULT_ACCOUNT_TYPE*);*
```

*类似地，我们可以使用经典的 if 语句或三元运算符来[使这段代码更具可读性](https://www.java67.com/2020/03/how-to-write-clean-code-using-java-8.html)。*

## *4.可选字段和方法参数👎*

*Optional 并不意味着用来包装类字段。这将导致在不需要的地方创建对象，如果重复使用，会导致性能下降。*

*况且*可选*类不是*可序列化*。因此，**具有*可选的*字段会导致序列化问题**。*

*此外，使用*可选*作为方法参数的**也可以被认为是反模式**。*

*假设我们有以下方法签名:*

```
*createAccount(String userName, Optional<Membership> membership)*
```

*如果是这种情况，并且*成员资格*参数可以为空，我相信简单地用它的两个版本重载该方法会更好。*

*这样，我们可以在需要的地方正确地检查和验证方法的*成员资格*参数:*

```
*createAccount(String userName);
createAccount(String userName, Membership membership);*
```

# *结论*

*正如我们在本文中一起看到的，*可选的*类可以为我们的域模型*(通过 getters)* 带来表现性和一定程度的安全性。*

*但是，如果使用不当，会导致糟糕的设计和代码混乱。因此，开发人员都认为应该完全避免这种情况。*

*我的观点是，通过在团队中建立一些*可靠的*规则和一些适当的代码审查，*可选的*对于 Java 开发人员来说是一个很好的特性。*

*![](img/289501a5cc4276492ea7f23de79c27bf.png)*

*Photo by [卡晨](https://unsplash.com/@awmleer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

## *资源*

*有很多关于这个话题的文章。如果你想更深入地研究这个主题，在我看来，这里有一些最好的:*

*   *[避免 NullPointerException](https://victorrentea.ro/blog/avoiding-null-pointer-exception/) *作者 Victor Rentea**
*   *迈克尔·恩斯特的可选类型 *再好不过了**
*   *可选，所有自行车棚之母*斯图尔特·马克斯**
*   *Java 中可选的 10 个例子*