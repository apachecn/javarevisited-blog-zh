# 如何在本地运行 spring-data 和 GCP 数据存储的测试。

> 原文：<https://medium.com/javarevisited/local-testing-with-spring-data-and-gcp-datastore-8e1015b571fa?source=collection_archive---------2----------------------->

![](img/18f815e715252b43bd51f2fccdaae3d3.png)

普里西拉·杜·普里兹在 [Unsplash](https://unsplash.com/s/photos/local?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在云中开发与传统开发有很大不同。一个经常出现的挑战是编写在本地或 CI 系统上工作的测试，而不访问云。

本文介绍了一种测试使用 Spring Boot 和谷歌的[数据存储库](https://cloud.google.com/datastore)的服务的方法。

## 先决条件

*   Java 11(它可以轻松地…