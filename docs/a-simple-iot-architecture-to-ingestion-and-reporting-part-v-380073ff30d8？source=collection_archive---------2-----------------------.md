# 摄取和报告的简单物联网架构—第五部分

> 原文：<https://medium.com/javarevisited/a-simple-iot-architecture-to-ingestion-and-reporting-part-v-380073ff30d8?source=collection_archive---------2----------------------->

第一部分 | [第二部分](/@rafaelfaita/a-simple-iot-architecture-to-ingestion-and-reporting-part-ii-60dfb7d8ac52) | [第三部分](/@rafaelfaita/a-simple-iot-architecture-to-ingestion-and-reporting-part-iii-71f37d730c20) | [第四部分](/@rafaelfaita/a-simple-iot-architecture-to-ingestion-and-reporting-part-iv-a79678bf6b30) | [第六部分](/@rafaelfaita/a-simple-iot-architecture-to-ingestion-and-reporting-part-vi-4cba7c240a1b)

# 语境

在我们关于[物联网](/javarevisited/my-favorite-courses-to-learn-internet-of-things-iot-in-2020-best-of-lot-8517aa9fc838)架构师的系列文章的第五部分中，我们将涵盖架构中最复杂的服务之一，即**规则服务**。为了解决这个服务的问题，我选择使用一种不同的架构方法。在这个具体的服务中，我选择了使用少量的[](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/ref=sr_1_1)****(领域驱动设计)和**六边形架构**使用大量的**测试自动化(单元测试— TDD)** ，同时没有忘记[](/javarevisited/10-oop-design-principles-you-can-learn-in-2020-f7370cccdd31)**…******