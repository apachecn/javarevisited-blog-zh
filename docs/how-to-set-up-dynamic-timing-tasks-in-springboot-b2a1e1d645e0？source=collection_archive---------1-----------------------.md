# 如何在 SpringBoot 中设置动态计时任务

> 原文：<https://medium.com/javarevisited/how-to-set-up-dynamic-timing-tasks-in-springboot-b2a1e1d645e0?source=collection_archive---------1----------------------->

![](img/8d7b6d383dda6b1e9fc5bf2aaafaec64.png)

[约翰·普莱斯](https://unsplash.com/@johnprice?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

本文介绍了在 [SpringBoot 中设置动态计时任务的三种方法。](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e)

要在 SpringBoot 项目中使用定时任务，您可以使用 cron 表达式，并在配置文件中预先定义它们。项目运行过程中不能动态修改任务执行时间，非常不灵活。学习后，特此记录如何在中实现动态计时任务…