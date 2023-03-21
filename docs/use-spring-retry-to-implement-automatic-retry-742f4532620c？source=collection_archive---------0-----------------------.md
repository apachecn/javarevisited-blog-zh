# 使用 Spring Retry 实现自动重试

> 原文：<https://medium.com/javarevisited/use-spring-retry-to-implement-automatic-retry-742f4532620c?source=collection_archive---------0----------------------->

使用 spring retry 来优雅地实现重试代码。

![](img/84c62d33ca853d179cf731adfd6e73fa.png)

一个运行在互联网环境下的系统，每时每刻都有出错的可能。比如磁盘已满，第三方接口调用失败，资源竞争失败，消息发送失败。其中有些问题需要运维部门来修复。有些可能是由…