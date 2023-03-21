# 关于 Maven 属性参考的所有内容

> 原文：<https://medium.com/javarevisited/all-about-maven-property-reference-b16aabb708f8?source=collection_archive---------1----------------------->

*在 Maven 中，它公开了一些与设置、底层 Java、操作系统等相关的内置属性。此外，它还可以在 pom 上定义自定义属性。在本文中，让我们讨论 maven 的内置属性以及自定义属性定义的重要性。*

![](img/4ffe3973e26145975a15db8741733e57.png)

照片由[蒂埃拉·马略卡](https://unsplash.com/@tierramallorca?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

在 maven `pom.xml`中，通过使用`${property_name}`来访问属性。您可以在 Maven 中定义您的自定义属性。此外，maven…