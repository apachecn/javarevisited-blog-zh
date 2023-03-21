# 如何在 Java 项目中引入 SPI

> 原文：<https://medium.com/javarevisited/how-to-introduce-spi-in-java-project-5232e02a587b?source=collection_archive---------1----------------------->

![](img/12129b5ebf0e2b0a202eb4f93322c776.png)

照片由[克里斯蒂安·卡斯蒂略](https://unsplash.com/@castillcc?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 什么是 SPI

`SPI`的全称是**服务提供者接口**，是一种将服务接口和服务实现分离的机制，实现解耦，提高程序可扩展性。服务提供者的介绍就是 SPI 接口实现者的介绍。具体实现类通过本地注册发现获得，而