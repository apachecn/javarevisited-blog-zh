# 用 Feign 和 Hystrix 处理 HTTP 客户端错误

> 原文：<https://medium.com/javarevisited/handling-http-client-errors-with-feign-and-hystrix-523e2dd8eb35?source=collection_archive---------0----------------------->

## 如何结合使用 Feign 和 Hystrix 来控制返回的错误

![](img/6c23005322e32e4b228311b49793fe99.png)

> *“任何可能出错的事情都会出错”——墨菲定律*

我认为这是 Feign 最好的特性之一。同样，我们可以定义`Encoder`、`Decoder` …