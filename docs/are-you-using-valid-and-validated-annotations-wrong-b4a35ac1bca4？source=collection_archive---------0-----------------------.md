# 使用@Valid 和@Validated 注释是否有误？

> 原文：<https://medium.com/javarevisited/are-you-using-valid-and-validated-annotations-wrong-b4a35ac1bca4?source=collection_archive---------0----------------------->

## 在 Spring Boot 验证输入

![](img/54fab531dbcdf37c8f9dda919fb0f61e.png)

[M·泽夫扎夫](https://unsplash.com/@mouriaghli?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

## 介绍

当我刚开始开发生涯时，我经常看到在控制器的方法参数上应用注释`@Validated`和`@Valid`。有些人用`@Validated`，而有些人更喜欢`@Valid`。我经常搞不清楚什么时候使用哪种注释。在这个…