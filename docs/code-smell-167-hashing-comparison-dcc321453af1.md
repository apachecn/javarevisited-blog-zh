# 代码气味 167 —哈希比较

> 原文：<https://medium.com/javarevisited/code-smell-167-hashing-comparison-dcc321453af1?source=collection_archive---------0----------------------->

## 散列保证两个对象是不同的。不是说他们是一样的

![](img/94a550daac9ca52fc062ad032a7f757e.png)

> *TL；DR:如果你检查散列，你也应该检查相等性*

# 问题

*   [双射故障](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)

# 解决方法

1.  检查哈希(快速)，然后检查相等性(慢速)

# 语境

2022 年 10 月 7 日，一个更大的[区块链](/javarevisited/the-blockchain-developer-architect-roadmap-d212d3bbbb00)必须停止。

这条新闻令人震惊，因为大多数区块链从定义上来说是分散的。

你可以在这里阅读全文:

[](https://mcsee.medium.com/how-a-hacker-stole-566m-usd-exploiting-a-code-smell-40409d80adfc) [## 黑客如何利用代码气味窃取了 5.66 亿美元

### 我不是安全专家。但是我喜欢干净的代码和代码的味道

mcsee.medium.com](https://mcsee.medium.com/how-a-hacker-stole-566m-usd-exploiting-a-code-smell-40409d80adfc) 

# 示例代码

## 错误的

```
public class Person {public String name;
// Public attributes are another smell   @Override
 public boolean equals(Person anotherPerson) {
   return name.equals(anotherPerson.name); 
 }@Override
 public int hashCode() {
   return (int)(Math.random()*256); 
 }
 // This is just an example of non-correlation   // When using HashMaps we can make a mistake 
 // and guess the object is not present in the collection}
```

## 对吧

```
public class Person {public String name;
// Public attributes are another smell   @Override
 public boolean equals(Person anotherPerson) {
   return name.equals(anotherPerson.name); 
 }@Override
 public int hashCode() {
   return name.hashCode(); 
 }
 // This is just an example of non-correlation }
```

# 侦查

[X]半自动

许多 linters 都有散列和相等重定义的规则。

通过突变测试，我们可以用相同的散列来播种不同的对象，并检查我们的测试。

*   身份
*   安全性

# 结论

每一种性能改进都有其缺点。

缓存和复制是显著的例子。

我们可以(必须)小心使用它们。

# 关系

[](https://blog.devgenius.io/code-smell-49-caches-d2e64373b838) [## 代码气味 49 —缓存

### 储藏处很性感。他们是一夜情。我们需要在长期关系中避开它们。

blog.devgenius.io](https://blog.devgenius.io/code-smell-49-caches-d2e64373b838) [](https://levelup.gitconnected.com/code-smell-150-equal-comparison-4a3d1eed823d) [## 代码气味 150 —同等比较

### 每个开发人员都平等地比较属性。他们错了。

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-150-equal-comparison-4a3d1eed823d) 

# 更多信息

[相等和散列](http://forum.world.st/Is-it-always-needed-to-redefine-hash-message-when-you-redefine-message-td4828721.html)

[Java 中的 Hashcode](https://stackoverflow.com/questions/3563847/what-is-the-use-of-hashcode-in-java)

[哈希码 vs 等于](https://www.digitalocean.com/community/tutorials/java-equals-hashcode)

# 放弃

代码气味只是我的[观点](https://mcsee.medium.com/i-wrote-more-than-90-articles-on-2021-here-is-what-i-learned-76c238f9936f)。

> 这可能会让你的一些读者感到惊讶，但是我的主要兴趣不是计算机安全。我主要对编写能按预期工作的软件感兴趣。

*Wietse Venema*

[](https://blog.devgenius.io/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](https://blog.devgenius.io/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://blog.devgenius.io/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](https://blog.devgenius.io/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)