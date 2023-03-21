# 使用 Keycloak 管理 API 在 Spring Boot 设置 Keycloak

> 原文：<https://medium.com/javarevisited/set-up-keycloak-in-spring-boot-using-the-keycloak-admin-api-4d0d97e024d5?source=collection_archive---------0----------------------->

[![](img/6637dce425125b1ee6a0a1b3d6cabcfa.png)](https://javarevisited.blogspot.com/2021/02/spring-security-interview-questions-answers-java.html)

在这篇博文中，我们将使用 Keycloak 管理 API 以编程方式设置 Keycloak。本文的目标是通过 Keycloak 管理控制台一次性设置我们的 Keycloak 领域，然后随意重新创建这个设置，而不需要重新进行所有的手动设置。为此，我们将使用 [Spring Boot](/javarevisited/10-advanced-spring-boot-courses-for-experienced-java-developers-5e57606816bd) 和 java Keycloak 管理 API 客户端来自动化这个过程。