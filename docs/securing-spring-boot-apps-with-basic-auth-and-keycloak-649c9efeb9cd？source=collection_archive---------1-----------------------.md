# 使用基本身份验证和密钥锁保护 Spring boot 应用

> 原文：<https://medium.com/javarevisited/securing-spring-boot-apps-with-basic-auth-and-keycloak-649c9efeb9cd?source=collection_archive---------1----------------------->

![](img/85c07b01e79b079e6f5bcec47239f4d5.png)

在本教程中，我将介绍如何在基本认证的基础上将安全的 Spring Boot 应用程序与 Keycloak 集成在一起。这种集成在许多情况下可能会变得很方便。

我前段时间的一个任务就是这种情况，我们需要用基本授权来保护“[致动器](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html)”端点，以便 [Kubernetes](/javarevisited/top-15-online-courses-to-learn-docker-kubernetes-and-aws-for-fullstack-developers-and-devops-d8cc4f16e773) 能够与 pod 通信以获取健康、信息等指标。

同时，我们希望用 keycloak 保护其他端点，以便与服务进行正常交互。这有助于服务的松散耦合设计，因为如果 keycloak 服务器由于任何原因关闭，服务仍将运行。

# 问题

为什么这种集成不能像预期的那样顺利工作的问题是两种安全机制相互重叠。过滤器**T5【KeycloakAuthenticationProcessingFilter.java】T6**拦截每一个带有 [HTTP 授权头](https://javarevisited.blogspot.com/2018/01/how-http-basic-authentication-works-in.html)的请求。因此，如果请求没有报头“Bearer: **** ”,它将被拒绝，并被重定向到 keycloak 进行身份验证，即使请求是用任何其他机制进行身份验证的，例如 Basic-auth。

# 解决办法

这里的解决思路是简单的在 key cloak 过滤器之前注册 Basic-Auth ***的过滤器。这可以使用 [@order](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/annotation/Order.html) 注释表单 spring 轻松完成。它只允许我们根据需要安排配置的顺序。***

我们将从第一个配置类开始，这是基本的身份验证过滤器:

为了使这个类尽可能方便，我们添加了这个“endPointMathcher ”,以便能够注册尽可能多的端点，以便在 Basic-Auth 下进行身份验证。它从配置文件(例如 Yaml)中获取一个逗号分隔的列表，并对其进行匹配。

然后我们将注册如下所示的 Keycloak 过滤器。此筛选器将用指定的客户端角色对任何其他端点进行身份验证。

现在我们已经完成了 java 配置。然而，我们仍然需要创建 keycloak 所需的配置，例如，创建一个客户端并为其分配一个角色，然后根据此信息修改 YAML/属性文件。这样，我们可以部署我们的服务，并且仍然能够更改任何配置，而不需要重新部署它。

本教程的完整工作示例可以在[这里](https://github.com/elmodeer/tutorials/tree/main/keycloak-basic-auth)找到。