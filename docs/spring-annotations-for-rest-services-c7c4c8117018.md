# REST 服务的 Spring 注释

> 原文：<https://medium.com/javarevisited/spring-annotations-for-rest-services-c7c4c8117018?source=collection_archive---------1----------------------->

*在本文中，我们将讨论 Spring 中不同的 REST 特定注释。*

![](img/874b7a1c9fd06306247ffff50e4515f8.png)

照片由[杰斯·贝利](https://unsplash.com/@jessbaileydesigns?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# @控制器

我们可以用`*@Controller*`注释来注释传统的控制器。这只是对`*@Component*`类的专门化，它允许我们通过类路径扫描自动检测实现类。