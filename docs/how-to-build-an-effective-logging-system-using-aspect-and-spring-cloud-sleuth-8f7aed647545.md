# 如何使用 Aspect 和 Spring Cloud Sleuth 构建有效的日志系统

> 原文：<https://medium.com/javarevisited/how-to-build-an-effective-logging-system-using-aspect-and-spring-cloud-sleuth-8f7aed647545?source=collection_archive---------1----------------------->

[![](img/524513204f55aa4716022bd84b4c1948.png)](https://javarevisited.blogspot.com/2011/05/top-10-tips-on-logging-in-java.html)

Nubelson Fernandes 在 [Unsplash](https://unsplash.com/s/photos/debugging?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

良好的日志实践是软件开发最重要的方面之一。它有助于在调试生产问题时快速提取上下文并隔离问题。

然而，最大的挑战是当团队没有一个[记录系统](https://javarevisited.blogspot.com/2016/06/why-use-log4j-logging-vs.html)时。如果每个人都遵循他/她认为正确的记录方式…