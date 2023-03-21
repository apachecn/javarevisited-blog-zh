# 让您的微服务独立

> 原文：<https://medium.com/javarevisited/make-your-microservices-independent-7e4e48173bac?source=collection_archive---------1----------------------->

![](img/c74c9f3a3ec660539d4bbd94c4010957.png)

基特·苏曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

微服务架构现在被广泛使用。许多公司使用它，但不是每个人都百分之百地使用它。要让微服务大放异彩，三个方面必不可少:

1.  独立进化
2.  独立测试
3.  独立部署

微服务真正的热闹是在所有这些点都满足的时候。否则，部署新版本可能需要与其他团队协调，级联所有依赖服务的更新，并祈祷所有更新都能成功滚动。级联回滚是一次冒险。

# 独立进化

我们的应用程序会随着时间的推移而发展。这是一个不可避免的过程，因为企业无法提前预测所有必要的功能，开发人员也无法在一周内完成所有工作。

如果我们谈论 [REST](/javarevisited/top-5-books-and-courses-to-learn-restful-web-services-in-java-using-spring-mvc-and-spring-boot-79ec4b351d12?source=---------17------------------) ，那么有一百万篇关于 API 进化的文章告诉我们如何在不影响消费者的情况下更新它们。

使用消息队列(如 [Kafka](/javarevisited/top-10-apache-kafka-online-training-courses-and-certifications-621f3c13b38c) )时，要注意 Avro，不要快速启动 JSON。Avro 支持模式进化和兼容性，这将在未来为您节省大量时间。

专有协议仍然是你的良心。你可能要重新发明轮子。

# 独立测试

有了测试，一切通常听起来很简单，但实际上，在一个请求的过程中，服务不得不拉一个又一个。测试不再如此独立。

我建议查看[Spring Cloud Contract wire mock](https://cloud.spring.io/spring-cloud-contract/2.1.x/multi/multi__spring_cloud_contract_wiremock.html)和 [Spring REST Docs](https://docs.spring.io/spring-restdocs/docs/current/reference/html5/) ，它允许您:

1.  基于测试编写文档
2.  生成一个清除服务的工件
3.  在其他微服务的测试中使用这个存根来模拟对它的调用

Spring Cloud 合同使用示例

因此，您可以不太担心依赖服务在现实中会如何反应，因为您将使用它们的实际存根。

# 独立部署

不是每个人都能承受更新时的服务停机时间。但如果更新过程中出现循环依赖(新 A 需要新 B，新 B 需要新 C，新 C 需要新 A)，这是不可避免的。我相信没有人愿意和其他团队协调发布新版本的微服务。在关于独立进化的第一节中，我们谈到了如何解决这个问题。但是还有一个问题——服务必须与它自己的先前版本相处。

你可能听说过蓝绿色部署和金丝雀部署这两个术语。这些方法假设新旧版本将共存一段时间。这意味着使用相同的数据库、分布式缓存等。

我们所需要的是保持与 1 版本的兼容性。

在关系数据库的情况下，新版本可能会在升级期间将一些迁移转移到数据库上。
因此，如果您需要从版本 1 中更改一个字段，那么最好分三步完成:

1.  在版本 2 中添加一个新字段，并支持这两个字段
2.  在版本 3 中，开始只使用新版本
3.  删除版本 4 中的旧版本

所以，如果新版本出现问题，以前的版本仍然可以正常工作，您不需要停止服务并恢复数据库。

类似的方法也适用于分布式缓存和 NoSQL 数据库

# 结论

以上都需要努力。同时支持不同的 API 版本，准备测试存根(尤其是第一次)，分几个步骤改变数据结构——所有这些都需要时间。因此，发布可能会延迟。但是为了充分利用微服务架构，需要做这些额外的工作。