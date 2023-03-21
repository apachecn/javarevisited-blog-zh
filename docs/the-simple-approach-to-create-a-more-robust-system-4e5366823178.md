# 创建更强大系统的简单方法

> 原文：<https://medium.com/javarevisited/the-simple-approach-to-create-a-more-robust-system-4e5366823178?source=collection_archive---------1----------------------->

![](img/6a64f9bd03de78067e44920038ecab3a.png)

*“系统不应该失败，应用程序不应该崩溃。”*这一原则已经在软件开发和设计分布式系统中得到广泛应用。各个公司都实施了某种方法来防止他们的应用程序崩溃。例如，亚马逊通过使用“[高可用系统](https://www.inc.com/kevin-j-ryan/the-crazy-ways-amazon-will-prevent-your-site-from-crashing.html)”来保护其云基础设施免于崩溃，高可用系统是指没有单点故障的系统——通过创建冗余，如果一个组件出现故障，系统可以继续运行。

脸书在刚开始使用 MYSQL 数据库时，将其分区到每个区域，以防止应用程序崩溃。然而，就像所有人都会犯错一样，所有系统都会遭遇失败。

有各种方法来限制应用程序不崩溃，如延迟失败，找到一个解决方法的错误，使系统可以继续运行，只是失败了。这些方法不是处理系统故障的最佳方式。

为什么？因为做那三件事并没有解决问题。bug 依然存在。隐藏问题只会导致问题在将来变得更大，从而花费更多的时间和资源来解决它。

**那么，如果申请失败了，我们该怎么办？**

答案是快速失败，然后早点回来。我们需要改变我们对待软件开发的方式。如果有任何问题，我们应该给出异常并返回它，而不是在方法中避免任何类型的异常并优雅地返回。

> *“软件开发最讨厌的方面，对我来说，就是调试。我讨厌的错误是那些在不寻常的情况下，每小时成功操作后出现的错误。”—吉姆·肖尔*

# 快速失败，而不是自动防故障

当软件快速失败时，它不会减少系统中的 bug 数量；但是会更容易发现错误并解决问题。快速失败会鼓励我们立即、明显地让系统失败。看起来它会通过立即暴露错误而使我们的系统变得脆弱。

但是，它创建了一个更健壮的系统，可以在投入生产之前解决问题。当错误更容易被发现和复制时，就更容易修复。例如，让我们创建一个自动防故障方法来计算租金金额:

```
public int maxRent() {
  RentPortfolio rent = Portfolio.get("rent-portfolio");
  if(rent == null) {
    return 1000;
  } 
  return rent.getMaxPrice();
} 
```

该方法通过从`Portfolio`对象获取`rent-portfolio`属性来计算最大租金。当您在这个方法中输入任何值时，它都会优雅地返回。

现在，想象一下在`Portfolio`类中有一个小的升级，一个正在升级代码库的开发人员在`rent-portfolio`到`rent-porttfolio`上创建了一个错别字。

软件仍然可以成功交付，应用程序仍然可以工作。然而，用户每次都会得到错误的值，因为它将返回一个默认值。当客户意识到它不准确时，将很难调试这个问题。

相反，如果你写一个快速失败的方法:

```
public int maxRent() {
  RentPortfolio rent = Portfolio.get("rent-portfolio");
  if(rent == null) {
    throw new PortfolioNotFoundException("rent-portfolio is not found in " + this.portfolioPath);
  }
  return rent.getMaxPrice();
}
```

这一次，开发人员会在发布软件之前检测到 bug，因为它会抛出一个`PortfolioNotFoundException`。

故障快速和故障安全方法的另一个例子:

```
*public division(int numerator, int denominator) {
  */* some code here ........ */*
  return numerator / denominator;
}

public static void main(String...args) {
  division(2,0);
}*
```

*`Exception: java.lang.ArithmeticException: / by zero …`*

****快速失效****

```
*public division(int numerator, int denominator) {
  if(denominator == 0) throw new IllegalArgumentException("denominator cannot be 0");
  */* some code here ..... */*
  return numerator / denominator;
}

public static void main(String...args) {
  division(2,0);
}*
```

*`Exception: java.lang.IllegalArgumentException: denominator cannot be 0 ….`*

*在这个例子中，fail-fast 确实抛出了一个错误。但是，应用程序没有抛出正确的消息。bug 的根本原因是因为是`InvalidArgumentException`而不是`ArithmeticException`。因此，[开发人员](https://javarevisited.blogspot.com/2018/05/10-tips-to-become-better-java-developer.html#axzz5jwmmAbXI)需要多次搜索代码才能知道问题出在哪里。Fail-fast 将帮助维护程序的开发人员立即发现错误。*

# *小贴士——一对夫妇没有通过快速检查:*

*   *空*
*   *空字符串*
*   *空列表*
*   *大多数先决条件检查*

# *识别可见故障的方法:*

*   *使用[测试驱动开发](https://javarevisited.blogspot.com/2019/04/top-5-junit-and-unit-testing-courses-java-programmers.html)。甚至在实现方法之前，您就可以通过使用断言来陈述成功的用例以及失败的用例。*
*   *在您的软件开发过程中有一个持续的集成构建将确保在将功能分支合并到主分支时任何必要的失败。*
*   *为健全性检查创建断言——断言和异常之间的区别可以在这里找到(链接)。*
*   *当系统设置出现任何问题时，不使用后备功能，而是让系统出现故障，以便您可以得到通知并立即解决问题。*
*   *当前端向服务器请求对集合的 GET 调用时，不是在没有找到集合时返回空列表，而是返回 404 not found 异常。*

# *早点回来*

*早归与快败相伴。当您发现一个 bug 时，您可以将它抛出到函数上方的系统堆栈中，或者立即返回该错误。例如，当您想在函数参数中创建一个前置条件检查时，您可以这样做:*

```
*public String getPersonalData(Person person) {
  if(!systemIsUp) {
   if(person.getName() == "") {
    if(person.age == 0) {
      if(person.pin == "") {
        return "Person doesn't have a pin"; 
      }
      return "Person age cannot be 0";
    }
    return "Person Doesn't have a name"
   }
   return "System is Down"
  }
  return person.getName();
}*
```

*如果你正在读这段代码，当你读到第三个语句时，你可能会忘记前两个参数的语句是什么。这种检查的主要问题是，当出现故障时，不仅很难调试，而且很难阅读。*

*这会导致很高的圈复杂度。圈复杂度是一个度量你的软件系统有多复杂的指标。你如何衡量复杂性？每当有`if`、`else`、`try`、`catch`、`for`、`and`、`or`、`others`、`while`的时候，你都在增加一点复杂性。圈复杂度越高，意味着系统越复杂，越难维护。*

*此外，记住所有的检查和嵌套的 if/else 语句很难。相反，应该尽早返回预处理的 if/else 语句，首先从简单的条件开始。*

```
*public String getPersonal(Person person) {
  if(!systemIsUp) return "System is Down";

  if(person.getName() == "") return "Person Doesn't have a name";

  if(person.age == 0) return "Person age cannot be 0";

  if(person.pin == 0) return "Person' doesn't have a pin";

  return person.getName();
}*
```

*如果你正在阅读这段代码，你就会明白先决条件应该是什么。你在[调试应用](https://www.java67.com/2018/01/how-to-remote-debug-java-application-in-Eclipse.html)的时候，不需要有一张纸，写下所有的 if/else 语句。此外，函数中编写的代码更少。*

# *外卖食品*

> **“失败，一次又一次的失败，是成就道路上的标杆。一个人朝着成功前进。”刘易斯。**

*为了使系统更加健壮，开发人员应该抛出任何出现的错误，以便很容易立即捕捉并修复它们。处理前提条件和健全性检查的一个好方法是抛出一个[异常](https://www.java67.com/2019/06/top-25-java-exception-interview-questions-answers.html)或[断言](https://javarevisited.blogspot.com/2012/01/what-is-assertion-in-java-java.html)。一旦条件不满足，立即退货。通过快速失败和早期返回，我们可以降低调试成本并提高系统质量。*

*感谢阅读！如果你喜欢这篇文章，请随意[订阅](https://edward-huang.com/subscribe/)我的时事通讯，每周都会收到关于科技职业的文章、有趣的链接和内容！*

*[如何在你的项目中保持干净的代码](https://edward-huang.com/software-best-practice/tech/software-development/2019/08/25/how-to-maintain-clean-code-in-your-projects/)*

*你也可以关注我在[媒体](/@edwardgunawan880)上的更多帖子。*

**原载于*[*https://edward-huang.com*](https://edward-huang.com/tech/software-development/2019/09/16/the-simple-approach-to-create-a-more-robust-system/)*。**

# *参考:*

*   *[马丁-福勒快速失效](https://www.martinfowler.com/ieeeSoftware/failFast.pdf)*
*   *[软件开发中的快速失败原则——DZone Agile](https://dzone.com/articles/fail-fast-principle-in-software-development)*
*   *[干代码与湿代码\-code mentor](https://www.codementor.io/joshuaaroke/dry-code-vs-wet-code-89xjwv11w)*

*学习 Java 的其他**有用资源**你可能喜欢的
[2019 年 Java 程序员应该学习的 10 件事](https://javarevisited.blogspot.com/2017/12/10-things-java-programmers-should-learn.html#axzz5atl0BngO)
[从零开始学习 Java 的 10 门免费课程](http://www.java67.com/2018/08/top-10-free-java-courses-for-beginners-experienced-developers.html)
[深入学习 Java 的 10 本书](https://medium.freecodecamp.org/must-read-books-to-learn-java-programming-327a3768ea2f)
[10 个工具每个 Java 开发者都应该知道的](http://www.java67.com/2018/04/10-tools-java-developers-should-learn.html)
[学习 Java 编程语言的 10 个理由](http://javarevisited.blogspot.sg/2013/04/10-reasons-to-learn-java-programming.html)
[2019 年 Java 和 Web 开发者应该学习的 10 个框架【t 成为 2019 年更优秀的 Java 开发者](http://javarevisited.blogspot.sg/2018/01/10-frameworks-java-and-web-developers-should-learn.html)
[2019 年要学习的 5 大 Java 框架](http://javarevisited.blogspot.sg/2018/04/top-5-java-frameworks-to-learn-in-2018_27.html)
[每个 Java 开发者都应该知道的 10 个测试库](https://javarevisited.blogspot.sg/2018/01/10-unit-testing-and-integration-tools-for-java-programmers.html)*

*</javarevisited/10-free-courses-to-learn-java-in-2019-22d1f33a3915> *