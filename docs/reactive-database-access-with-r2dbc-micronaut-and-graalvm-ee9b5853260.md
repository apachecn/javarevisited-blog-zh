# 使用 R2DBC、Micronaut 和 GraalVM 进行反应式数据库访问

> 原文：<https://medium.com/javarevisited/reactive-database-access-with-r2dbc-micronaut-and-graalvm-ee9b5853260?source=collection_archive---------2----------------------->

![](img/cd620a227c1899e5e0f33dff86548778.png)

照片由 [Guillaume Jaillet](https://unsplash.com/@i_am_g?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这个故事中，我展示了如何使用 R2DBC 以一种被动的方式从 Java 访问关系数据库。因此，我将创建一个示例项目，包含一个简单的 rest 接口来创建、读取、更新和删除 PostgreSQL 数据库中的行。示例项目将使用 [Micronaut](https://micronaut.io) 框架作为本地 [GraalVM](https://www.graalvm.org) 映像运行。我希望这种结合能极大地改善…