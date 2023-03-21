# 创建一个简单的 Java 测试框架

> 原文：<https://medium.com/javarevisited/creating-a-simple-java-test-framework-bae32ba3fdea?source=collection_archive---------3----------------------->

## 使用反应流水线实现基于小黄瓜的集成测试

![](img/f745ac53f835ed6ed12749e5f8518ac3.png)

图片来自[Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=849269)Krzysztof Jaracz

关于集成测试的一件事是，你需要从一个步骤到另一个步骤保持状态。假设您正在测试一个实现了 [RESTful API](/javarevisited/top-5-books-to-learn-web-services-in-java-soap-rest-22d92adbefc1) 的应用程序。首先，您想要创建一些测试数据，并将其传递给创建端点。