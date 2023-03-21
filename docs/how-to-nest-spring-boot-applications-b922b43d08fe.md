# 如何嵌套 Spring Boot 应用程序

> 原文：<https://medium.com/javarevisited/how-to-nest-spring-boot-applications-b922b43d08fe?source=collection_archive---------3----------------------->

## 将 web 内容放在哪里，如何让控制器自动加载，以及如何将应用程序作为 Maven 依赖项导出/导入

![](img/bf95a673196ac4bbc60771b8a5e0e9bd.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) 拍摄

我参与的一个项目需要通过在 pom.xml 中配置一个依赖项来创建一个 [Spring Boot](https://spring.io/projects/spring-boot) 应用程序，以便在另一个应用程序中使用