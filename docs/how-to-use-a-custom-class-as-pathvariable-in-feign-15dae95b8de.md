# 如何在 Feign 中将自定义类用作@PathVariable👨‍🏫

> 原文：<https://medium.com/javarevisited/how-to-use-a-custom-class-as-pathvariable-in-feign-15dae95b8de?source=collection_archive---------3----------------------->

## 在本文中，您将了解我们如何使用两个佯概念来适应外部 API: @PathVariable 和 RequestInterceptor。

![](img/bd04e7d123686a687108ac52113eb014.png)

照片由 [**泰勒·拉斯托维奇**](https://www.pexels.com/@lastly?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**佩克斯**](https://www.pexels.com/photo/brown-wooden-dock-surrounded-with-green-grass-near-mountain-under-white-clouds-and-blue-sky-at-daytime-808465/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

你知道 [**假装**](https://github.com/OpenFeign/feign) 吗？它是一个 Java 库，允许你用最少的代码编写一个 REST 客户端。在我目前的工作项目中，我们目前正在使用它…