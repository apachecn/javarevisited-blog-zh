# 我们如何发现 Apache Tomcat 不能处理大型请求

> 原文：<https://medium.com/javarevisited/how-we-found-apache-tomcat-couldnt-handle-large-requests-fd41b8b5f8e7?source=collection_archive---------2----------------------->

## 这一切都始于我们内部库的错误报告。

![](img/8da091e78b158147979988587c2059d1.png)

由[路](https://unsplash.com?utm_source=medium&utm_medium=referral)上[车头](https://unsplash.com/@headwayio?utm_source=medium&utm_medium=referral)拍摄

# 错误报告

几周前，一个团队报告了一个问题。他们的 Zuul 代理停止向后端系统转发请求体。在他们的 Windows 机器上，开发团队无法重现该问题。我们也不能。