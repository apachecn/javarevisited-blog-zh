# 用 Java 登录—Log4j vs . log back vs . SLF4J

> 原文：<https://medium.com/javarevisited/logging-in-java-log4j-vs-logback-vs-slf4j-88c533088d2a?source=collection_archive---------0----------------------->

[![](img/e0fce36c04c84f740cdb4b71e0a856d8.png)](https://www.java67.com/2021/10/how-to-set-logging-level-in-spring-boot-.html)

图片来源:[https://www . overops . com/WP-content/uploads/2013/12/blog-running . jpg](https://www.overops.com/wp-content/uploads/2013/12/blog-running.jpg)

大家好。我将讨论另一个重要但令人困惑的话题，即[登录 Java](https://javarevisited.blogspot.com/2011/05/top-10-tips-on-logging-in-java.html) 。

## 什么是日志记录？

日志记录是打印或记录应用程序中的活动的过程，有助于开发人员了解和分析系统中何时出现任何意外错误。在引入日志框架之前…