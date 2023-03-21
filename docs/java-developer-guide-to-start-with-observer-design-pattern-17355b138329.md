# 从观察者设计模式开始的 JAVA 开发人员指南

> 原文：<https://medium.com/javarevisited/java-developer-guide-to-start-with-observer-design-pattern-17355b138329?source=collection_archive---------2----------------------->

![](img/f11188e12972c118e0adafd6e112ecb2.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Marten Newhall](https://unsplash.com/@laughayette?utm_source=medium&utm_medium=referral) 拍摄的照片

如果你使用过[卡夫卡](/javarevisited/top-10-apache-kafka-online-training-courses-and-certifications-621f3c13b38c)或者 [JMS](https://javarevisited.blogspot.com/2020/05/top-16-jms-java-messaging-service-interview-questions-answers.html) ，你很可能知道发布-订阅交互模型。在消息传递应用程序中，当发布者对象生成主题内容时，订阅主题或话题的订阅者对象会收到通知。理论上，订阅服务器可以订阅多个发布服务器。观察者设计模式有助于解决发布/订阅模型，该模型允许许多观察者对象看到主题事件。

# 观察者模式是什么？

[观察者模式](https://javarevisited.blogspot.com/2011/12/observer-design-pattern-java-example.html)是《设计模式:可重用面向对象软件的元素》一书中的**行为设计模式**之一。当对象之间存在一对多关系时，使用观察者模式，例如，如果一个对象被修改，它的依赖对象将被自动通知和更新。在这种模式中，观察另一个对象状态的对象称为**观察者**，被观察的对象称为**主体**。

Twitter 和脸书等所有社交媒体网站都是这种模式的典型例子。您关注一个用户，当该用户添加内容时，您和该用户的所有其他关注者都会收到通知。其他示例包括 RSS 提要和电子邮件订阅，您可以选择关注或订阅，并收到新提要的最新通知。

# 观察者模式 JAVA 示例

假设我们正在构建一个加密货币购买/销售应用程序，它允许用户根据收到的价格变化事件通知来触发购买或销售流程。对于这个例子，我将有几个观察者订阅加密货币。当加密货币价格下跌时，买方观察员将购买一些股票。同样，如果[加密货币](https://javarevisited.blogspot.com/2022/01/5-best-courses-to-learn-cryptocurrency.html)价格上涨，卖方观察员将卖出股票。

[![](img/eb463161824ed21fd436f92fa7e4153a.png)](https://javarevisited.blogspot.com/2022/01/5-best-courses-to-learn-cryptocurrency.html)

杰里米·贝赞格在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 主题定义

在 Subject 接口中，声明了三个方法。 **addObserver** 方法用于添加订阅者， **removeObserver** 方法用于删除现有订阅者， **notifyObservers** 方法用于向所有订阅者发送通知。

主题界面

## 具体主题

对于具体的 Subject，我创建了一个 **CryptoCurrency** 类，其中包含了来自 **Subject** 接口的所有实现方法，以及用于加密货币的名称和价格的新字段。

具体主题

## 观察者定义

在**观察者**接口中，只有一个方法被声明为**通知**，该方法将被具体的主体调用。

观察者界面

## 具体的观察者

对于具体的观察者，我创建了 3 个类。 **PriceWatcher** 类被创建用来告诉更新**加密货币**具体主体的当前和以前的价格。 **CryptoCurrencyBuyer** 类是为调用外部 API 在货币低于一定金额时购买加密货币而创建的。类似地，创建了 **CryptoCurrencySeller** 类，用于在货币达到一定金额以上时调用外部 API 来出售加密货币。

第一个具体的观察者

第二具体观察者

第三具体观察员

## 观察者模式在起作用

在下面的演示代码中，创建了一个比特币和五个观察者。当比特币价格更新时，所有订户都会收到通知。

带输出的主方法

源代码可从以下网址获得:[https://github . com/s3c-d43m0n/Desing-Patterns-in-JAVA/tree/main/Behavioral/Observer](https://github.com/s3c-d43m0n/Desing-Patterns-in-JAVA/tree/main/Behavioral/Observer)

# 实施中的关键点

*   这里的**主体**和**观察者**是接口。但是它也可以是一个抽象类，并不局限于只使用接口。
*   **具体主体**需要根据要求维护**集合**中的所有**观察者**。在这里，我使用了 **CopyOnWriteArrayList** (故障安全)集合来存储所有的观察器，因为我有一些**观察器**，它们将在**notify observators**方法中迭代时自行移除。
*   当**具体主体**的状态发生变化时，需要调用 **notifyObservers** 方法。
*   每个**具体观察者**需要实现**通知**方法，以便根据其需求采取行动，消耗**主题**的变化。
*   对于每个**具体观察者**，需要用**添加观察者**的方法与**主体**连接，用**移除观察者**的方法分离。

*你最喜欢的设计模式是什么，或者如果你想更好地理解任何其他模式或主题，请随时联系我，在*[*LinkedIn*](https://www.linkedin.com/in/ritvik92/)*或*[*Google Form*](https://forms.gle/XFsuo1ZbP35gfqAX7)*上，我下次会尽力介绍！*

请把这个分享给你所有的媒体朋友，然后点击那个👏按钮，以扩大它的范围。未来更新请关注 [*me*](/@ritvik.singh.chauhan) *。感谢阅读。*