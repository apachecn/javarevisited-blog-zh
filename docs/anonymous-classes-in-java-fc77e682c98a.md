# Java 中的匿名类

> 原文：<https://medium.com/javarevisited/anonymous-classes-in-java-fc77e682c98a?source=collection_archive---------2----------------------->

![](img/063ca8b07b6fabe5a512cf25d02588f6.png)

## 对于不知道什么是匿名类的人来说，让我给你一个简单的介绍。匿名类是没有名称的内联类，它重载方法。这是通过一个类似构造器的创建过程来实现的…通过下面的例子，一切都变得清晰了。

是的，这是一个相当不为人知的语言特性，在许多其他编程语言中都找不到。除了 [Java](/javarevisited/top-5-java-online-courses-for-beginners-best-of-lot-1e1e240a758) ，我只知道 [PHP7](/javarevisited/top-10-free-courses-to-learn-php-and-mysql-for-web-development-e96e69982675) 有这个语言特性。

当我第一次了解这个特性的时候，我并没有看到使用它的吸引力，但是不久之后，我从事的一个项目在覆盖小的接口类时使用了这种技术。

能够做到这一点在短期内是一个快速轻松的胜利。然而，从长远来看，您很可能需要在代码中的其他地方使用完全相同的实现。这就出现了一个问题，要么你在你的代码库的两个地方有相同的匿名类，要么你必须为它创建一个小类。

![](img/1c392e49b663058929fb377602008a7e.png)

蒂姆·高夫— [像素](https://www.pexels.com/photo/man-in-white-shirt-using-macbook-pro-52608/)

显然，最好的选择是创建一个小型的命名类。但是当我浏览这个项目时，我可以发现分散在各处的完全相同的类。这背后的原因是:

*   由于缺少时间或截止日期，所以这些匿名类经常被复制粘贴到代码中。
*   另一个原因是因为这个人不知道代码中的其他地方实现了这个，并且花时间再次实现它。

这导致了 bug 的增加，因为在实现一些新功能时，实现变得略有不同或者被忽略了。最好是创建该特性的一个严格的子类/实现，而不是让匿名类分散在代码中。将它们放在一个适当的类中还会显著提高代码的可调试性和可读性。

> 不管拥有小型内联类的想法有多好，相对于长期的灵活性，它不值得短期的轻松。

# 为什么要使用匿名类？

使用匿名子类也感觉非常不必要，因为大多数时候你可以用 Java 或其他语言中的 lambdas、delegates 或 events 来替换这个特性。当决定将整个 Java 代码库转移到 [C#](/@javinpaul?source=follow_footer--------------------------follow_footer-) 时，这变得非常明显，我们最终摆脱了所有这些较小的内联匿名类。

# 结论

我建议在你的项目中尽量远离匿名类，然而，我可以看到在特定情况下使用它的吸引力。然而，我建议不要在新项目开始时使用它们，只有在项目成熟时才开始使用它们。你也可以用它们来与 [libGDX](https://javarevisited.blogspot.com/2019/03/5-free-game-development-courses-unity-corona-libgdx-java.html) 中的插件和单件接口，我也亲眼见过。我认为这完全取决于程序员，但我已经吸取了教训，并将始终对这一语言特性保持谨慎。

![](img/0b0ca0ba6d6a4d1f23859c04129ff552.png)

封面图片由[巴图格泽尔](https://unsplash.com/@gezerbatu?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/t/technology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄。