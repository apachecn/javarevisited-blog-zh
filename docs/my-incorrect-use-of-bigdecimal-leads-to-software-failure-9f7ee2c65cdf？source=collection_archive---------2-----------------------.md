# 我对 BigDecimal 的错误使用导致软件失败

> 原文：<https://medium.com/javarevisited/my-incorrect-use-of-bigdecimal-leads-to-software-failure-9f7ee2c65cdf?source=collection_archive---------2----------------------->

为了避免 bug，BigDecimal 应该这样使用。

![](img/0bab5fa2d5c5750a75295e7932093c8d.png)

这是我职业生涯中的真实事件，因为错误使用 [BigDecimal](https://javarevisited.blogspot.com/2012/02/java-mistake-1-using-float-and-double.html) 导致软件失败。事情是这样的，当时我们公司正在为一个网上商城开发软件。

我的团队负责支付模块的开发。用户提交订单后,…