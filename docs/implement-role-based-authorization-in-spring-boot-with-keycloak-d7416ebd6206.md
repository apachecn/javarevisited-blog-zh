# 用 Keycloak 在 Spring Boot 实现基于角色的授权

> 原文：<https://medium.com/javarevisited/implement-role-based-authorization-in-spring-boot-with-keycloak-d7416ebd6206?source=collection_archive---------1----------------------->

[![](img/220d0da7fdc099b09c98f64d9227933c.png)](https://javarevisited.blogspot.com/2013/07/role-based-access-control-using-spring-security-ldap-authorities-mapping-mvc.html)

基于角色的访问控制对于任何与用户打交道的应用程序都是必不可少的，这些用户可以根据其组织的角色来访问资源。

在之前的[文章](https://gauthier-cassany.com/posts/spring-boot-keycloak)中，我们已经了解了如何通过使用 OpenID Connect 认证协议，用 Keycloak 保护我们的 [Spring Boot REST API](/javarevisited/top-5-books-and-courses-to-learn-restful-web-services-in-java-using-spring-mvc-and-spring-boot-79ec4b351d12?source=---------17------------------) 。