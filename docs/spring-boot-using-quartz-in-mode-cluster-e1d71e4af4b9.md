# 在模式群中使用石英的 Spring Boot

> 原文：<https://medium.com/javarevisited/spring-boot-using-quartz-in-mode-cluster-e1d71e4af4b9?source=collection_archive---------1----------------------->

# TL；速度三角形定位法(dead reckoning)

默认情况下，如果并行启动两个服务实例，Quartz 的 Spring Boot 库就不能正常工作。在这种情况下，每个服务开始执行相同的作业。预期的行为是其中一个实例被选择来执行作业，而其他实例在后台等待，并且在第一个服务失败的情况下，另一个被选择来承担作业。

在本文中，我将展示如何配置 [Spring Boot](/javarevisited/10-advanced-spring-boot-courses-for-experienced-java-developers-5e57606816bd?source=collection_home---4------0-----------------------) 来并行使用 Quartz 和服务…