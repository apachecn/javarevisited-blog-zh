# 向 Keycloak 中的 JWT 令牌添加“外部 API 调用”属性

> 原文：<https://medium.com/javarevisited/adding-an-external-api-call-attribute-to-jwt-token-in-keycloak-b417ec10274d?source=collection_archive---------0----------------------->

# TL；速度三角形定位法(dead reckoning)

默认情况下，Keycloak 没有提供一个映射器来为 JWT 令牌添加“外部 API 调用”属性，在这篇文章中，我将展示如何使用 Keycloak 库创建一个自定义映射器，并使用这个映射器通过 REST API 为 JWT 令牌添加新属性。

![](img/0dc9f81b9bcd28048700129455cb66f7.png)

照片由[弗兰克](https://unsplash.com/@franckinjapan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 语境