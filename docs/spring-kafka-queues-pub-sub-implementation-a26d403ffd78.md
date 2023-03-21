# Spring Kafka:队列和发布订阅实现

> 原文：<https://medium.com/javarevisited/spring-kafka-queues-pub-sub-implementation-a26d403ffd78?source=collection_archive---------1----------------------->

## 春天卡夫卡入门指南

[![](img/69ebe307eab2befcf3884649b6299bfe.png)](https://javarevisited.blogspot.com/2022/03/spring-boot-kafka-example-single-and-multiple-consumers.html)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Deb Dowd](https://unsplash.com/@fin777?utm_source=medium&utm_medium=referral) 拍摄的照片

## **简介**

在这个快速教程中，我们将从创建最简单的设置开始——仅够生成和使用来自 *Kafka Broker* 的消息。

然后，我们将看到在这两种方法中消费消息是多么简单，例如*队列*和*发布订阅*。

之后，我们将探索一些操作方法、常见问题和核心概念——这对于更复杂的场景可能是必要的。

所以，事不宜迟，现在让我们开始在本地建立[卡夫卡](/javarevisited/top-10-apache-kafka-online-training-courses-and-certifications-621f3c13b38c)。

## **阿帕奇卡夫卡快速入门**

我们可以在这里看到官方指南:[https://kafka.apache.org/quickstart](https://kafka.apache.org/quickstart)。

然而，为了简明起见，我们可以单独执行下面的步骤(这些步骤足够了):

1.  下载阿帕奇卡夫卡[这里](https://www.apache.org/dyn/closer.cgi?path=/kafka/3.1.0/kafka_2.13-3.1.0.tgz)
2.  提取并 cd 入其中:
    2.1 tar -xzf 卡夫卡 _ 2.13–3 . 1 . 0 . tgz
    2.2 CD 卡夫卡 _ 2.13–3 . 1 . 0
3.  启动 Kafka 环境
    3.1 **运行 Zookeeper:**bin/Zookeeper-server-start . sh config/Zookeeper . properties
    3.2**运行 Kafka 代理:**bin/Kafka-server-start . sh config/server . properties

有了这个，Kafka broker 现在就可以运行了！我们需要做的下一件事是设置生产者和消费者。

## **在 Spring Boot 创造生产者和消费者服务**

虽然可以使用单个服务来产生和消费事件；然而，为了更有趣，我们将创建两个独立的项目——一个用于*生产者*，一个用于*消费者*。

首先，让我们为两个项目在 pom.xml 上添加这种依赖关系:

此外，对于生产者服务，让我们添加这个依赖关系:

接下来，让我们创建一个简单的*MessageProducerController*，它使用主题名“test-topic”向 [Kafka Broker](https://javarevisited.blogspot.com/2021/12/5-free-courses-to-learn-apache-kafka.html) 发送消息:

转到消费者服务，为了简单起见，让我们使用相同的“测试主题”在主类上创建一个监听器方法:

现在我们有了最基本的设置，让我们试着创建一个到/send-message 端点的 [POST 请求](https://javarevisited.blogspot.com/2016/10/difference-between-put-and-post-in-restful-web-service.html):

```
curl -X POST http://localhost:8080/send-message \
--header 'Content-Type: application/json' \
--data '{"message": "sample message"}'
```

如果我们要检查消费者服务的日志，我们可以验证我们确实处理了消息。

然而，在这一点上，我们仅仅触及了表面——我们还不知道代理如何存储或处理消息。到目前为止，我们刚刚消费了它。

在深入研究之前，最好先看看实现广为人知的概念有多简单，比如卡夫卡[中的*队列*和*发布订阅*。](https://javarevisited.blogspot.com/2018/04/top-5-apache-kafka-course-to-learn.html)

## **卡夫卡中的队列和酒馆**

从队列开始，假设我们有多个消费者，但是我们只希望每条消息处理一次。然后，我们用相同的值设置消费者的 groupIds。

另一方面，对于发布-订阅，如果我们想要处理每个消费者的消息，我们为每个消费者的 groupId 设置一个惟一的值。

这两种情况都有一个例子:

注意 *:* 在我们当前的设置中，我们还没有为“测试主题”创建配置；因此，如果只有一个分区，但同一个组有两个用户，其中一个用户将会空闲。我们还没有触及分区的概念，所以稍后会详细介绍。

## **常见问题解答/操作方法/概念**

**1。Kafka Broker 如何存储消息？**

**简答:**穿越日志。

Kafka broker 使用主题名(以及作为命名约定的分区号)创建一个目录。然后，它在该目录中创建一个日志文件。

要找到这些日志所在的位置，我们可以检查 config/server.properties，然后查找" *log.dir* "属性。

**2。消费者如何知道下一步要处理什么消息？**

**简答:**通过偏移

使用我们的简单实现，我们可以尝试在每次处理消息时检查偏移量。

以下是使用终端进行检查的快速方法:

```
bin/kafka-run-class.sh kafka.tools.GetOffsetShell — broker-list localhost:9092 — topic test-topic
```

**3。什么是分区？为什么要用？**

简短回答:一个分区保存日志。用例:可伸缩性(并行)和高可用性(冗余)

如前所述，代理通过日志存储事件/消息——这些日志位于特定的分区中。

拥有多个分区允许多个用户并行处理消息。

为了实现高可用性，如果我们有多个代理实例，那么我们可以相对于代理实例的数量来设置主题的复制——这将导致代理的追随者实例的分区复制。

**4。如何将一个主题配置为具有多个分区和复制？**

这里有一个[样品豆](https://javarevisited.blogspot.com/2022/02/how-to-fix-autowired-no-qualifying-bean.html)。其中参数分别表示主题名称、分区数量和复制数量。

**5。如何发送/产生自定义事件/消息？**

为此，我们需要创建一个生产者配置。以下是使用 JsonSerializer.class 的大多数用例的示例:

**6。如何接收/消费自定义事件/消息？此外，我们如何处理反序列化错误？在这种失败的情况下，偏移量会发生什么变化？**

为此，我们需要创建一个消费者配置。下面是一个示例配置，我们在 Kafka 侦听器容器上添加了一个自定义错误处理程序，同时还将 AckMode 配置为 MANUAL_IMMEDIATE，以便只更新“acknowledge”方法调用的偏移量:

我们做到了！和往常一样，源代码可以在 [GitHub](https://github.com/emyasa/medium-articles/tree/master/spring-boot-kafka) 上获得。