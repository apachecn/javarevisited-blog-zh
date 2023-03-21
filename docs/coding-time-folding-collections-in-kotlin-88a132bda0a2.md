# 编码时间:科特林的折叠收藏

> 原文：<https://medium.com/javarevisited/coding-time-folding-collections-in-kotlin-88a132bda0a2?source=collection_archive---------5----------------------->

![](img/8e100881b901a3aec5a619cbc700d33b.png)

照片由[马克西米利安·魏斯贝克尔](https://unsplash.com/@maxweisbecker?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

最近，我和我的队友一起完成了一项任务，其中包括匹配不同列表中的值。在这个过程中，我意识到我并不熟悉 [Kotlin](/javarevisited/top-5-courses-to-learn-kotlin-in-2020-dfc3fa7706d8?source=---------16------------------) 中的`Iterable<T>.fold()`操作……嗯，我的队友意识到了这一点，并就此取笑了我一会儿。

因此，第二天早上我做的第一件事就是打开 [Kotlin Koan](https://play.kotlinlang.org/koans/overview) 进行`fold`操作！

## **设置**

有一个`Shop`跟踪它的`Customers`的历史，这些`Customers`有某些`Product`的`Order`:

任务是:对于给定的商店，获取每个顾客订购的产品列表。当然，使用`fold`功能。

## 用我已经知道的知识解决问题

我的第一步是简单地解决这个问题——我需要获得每个客户的订单列表，并找到这些列表的交集:

a `List <Customer>`到`List<List<Product>>`的初始映射感觉没有必要，所以我把它去掉了。此外，我在`Customer`中添加了一个`getter`，返回该客户订购的所有`Product`。：

这两个都通过了 Koan 测试，所以是时候使用`fold`函数了。

## 用`fold function`解决问题

该函数的定义如下:

太好了，我的`initial`值将是第一个客户订购的产品，而`operation`与我一直使用的`intersect`相同:

看起来非常整洁简洁！让我们为客户列表为空的情况添加一个防护:

## 与解决方案相比

最后，科特林学院自己的解决方案:

万岁，我真的很接近了！我的解决方案更加简洁，因为它没有第一次迭代就获得所有购买产品的列表。

我希望你觉得这很有趣！

你自己试一试怎么样？