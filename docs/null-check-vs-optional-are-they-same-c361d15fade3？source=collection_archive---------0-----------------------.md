# 空检查 vs 可选？它们一样吗？

> 原文：<https://medium.com/javarevisited/null-check-vs-optional-are-they-same-c361d15fade3?source=collection_archive---------0----------------------->

![](img/b159143d45bae890c171169dd24638ee.png)

这个问题在采访中被问到的时候，我和大家一样回答了“ **Java 8 在 java.util 包中引入了这个新类** [**可选**](https://javarevisited.blogspot.com/2017/04/10-examples-of-optional-in-java-8.html) **。通过使用 Optional，我们避免了空检查，并可以指定返回替代值，从而使我们的代码更具可读性。**”

但是接下来面试官写了下面的代码(见例 1)并问“**为什么 optional 更好，如果它和 null check 做了几乎相同的事情，因此没有达到任何特殊的目的。”**

# 示例 1:

```
String value = null;
**Optional<String> optionalValue = someFunction();**

if(**optionalValue.isPresent()**) 
   value = **optionalValue.get().toUpperCase();**
else
   value = **"NO VALUE";**
```

对

```
String value = null; 
String **possibleNull = someFunction(); 
if(possibleNull != null)** 
   value = **possibleNull.toUpperCase()**;
else
   value = **"NO VALUE";**
```

# 示例 2:

```
private boolean validAddress(NullPerson person) 
{
  **if****(person != null)** 
  {
    **if****(person.getAddress() != null)** 
    {
     final Instant validFrom = person.getAddress().getValidFrom();
     return **validFrom != null** && validFrom.isBefore(now());
    }
    else
      return false;
   } 
   else
     return false;
}
```

对

```
private boolean validAddress(NullPerson person) 
{
  return **personOptional.isPresent()** &&   **person.getAddress().isPresent()** && **person.getAddress().getValidFrom().isPresent()** && person.getAddress().getValidFrom().isBefore(now());
}
```

## **真正的**优势`Optional`

这是一个库类，它有一个相当丰富的 API 来以一种更安全的方式处理空的情况。

因此例 1 的[可选](https://javarevisited.blogspot.com/2018/08/top-5-free-java-8-and-9-courses-for-programmers.html#axzz6ccm5KWKs)方式应为:

```
**String value = someFunction().map(String::toUpperCase)
          .orElse("NO VALUE");**
```

示例 2 应该写成

```
private boolean validAddress(NullPerson person) 
{
  return **person.flatMap(Person::getAddress).flatMap(Address::getValidFrom).filter(x -> x.before(now())).isPresent();**
}
```

# 不足之处

当然，没有灵丹妙药…

1)在性能上:将原始值包装到可选实例中，会在紧循环中表现出明显的性能下降。在 Java 10 [中，值类型](http://cr.openjdk.java.net/~jrose/values/values-0.html)可能会进一步减少或消除损失。

2)关于序列化:`[Optional](http://blog.codefx.org/jdk/dev/why-isnt-optional-serializable/)` [不可序列化](http://blog.codefx.org/jdk/dev/why-isnt-optional-serializable/)但是一个[变通方法](http://blog.codefx.org/jdk/serialize-optional/)并不过分复杂。

# 结论

`Optional`的主要目的是在为函数返回值时指出缺少值(即空值)。这是为了向打电话的人阐明意图。

我相信，当可选的使用方式与早期的 NPE 检查相同时，问题就出现了。可选类是指包含值或不包含值的容器。**因此我认为 Java 不应该提供** [**get()和 ifPresent()方法**](https://www.java67.com/2018/06/java-8-optional-example-ispresent-orElse-get.html?fbclid=IwAR3_3ir-IAia8frQHZetwSqQaN3hQerLvhpq7Jukh1zD_EbPtaRjHn0rkhk#ixzz5mMbVkkfm) **或者应该提供替代机制来方便检索(和检查行为**)。

参考资料:

1.  [https://www . nurkiewicz . com/2013/08/optional-in-Java-8-cheat-sheet . html](https://www.nurkiewicz.com/2013/08/optional-in-java-8-cheat-sheet.html)
2.  [https://ni pafx . dev/intention-revealing-code-Java-8-optional #](https://nipafx.dev/intention-revealing-code-java-8-optional#)
3.  [https://dev . to/piczmar _ 0/Java-optional-in-class-fields-why-not-40df](https://dev.to/piczmar_0/java-optional-in-class-fields-why-not-40df)
4.  [https://ionutbalosin . com/2018/05/optional-API-vs-explicit-null-check-race/](https://ionutbalosin.com/2018/05/optional-api-vs-explicit-null-check-race/)
5.  [https://www . Oracle . com/technical-resources/articles/Java/Java 8-optional . html](https://www.oracle.com/technical-resources/articles/java/java8-optional.html)
6.  [https://Java re visited . blogspot . com/2017/04/10-examples-of-optional-in-Java-8 . html](https://javarevisited.blogspot.com/2017/04/10-examples-of-optional-in-java-8.html)

有问题吗？评论？建议？

下一步是什么？

[**在媒体上关注我**](/@vaibhav0109) 成为第一个阅读我的故事的人。