# Kafka 客户→仅获取特定消息

> 原文：<https://medium.com/javarevisited/kafka-customer-get-what-needs-only-45d95f9b1105?source=collection_archive---------0----------------------->

> 这不是传统的 spring Kafka 消费者设置，在那里消费者积极地倾听特定的话题。Kafka 客户可以从主题的指定偏移量检索消息。

[![](img/cabc600f1d769c5428d5b1a2190b2f29.png)](https://javarevisited.blogspot.com/2018/04/top-5-apache-kafka-course-to-learn.html)

[https://www.google.com/search?q=customer+in+shop&sxsrf = APq-WBsg _ e 6 hxncfg 260 tkzk 23 cgevwe 7 q:1643427593917&source = lnms&TBM = isch&sa = X&ved = 2 ahukewix4 svvhnb 1 ahvooswwghvdjaekq _ AUoAXoECAEQAw&biw = 1920&BIH = 981【T3](https://www.google.com/search?q=customer+in+shop&sxsrf=APq-WBsg_E6hXNCFG260TkZk23CGEVWE7Q:1643427593917&source=lnms&tbm=isch&sa=X&ved=2ahUKEwix4svvhNb1AhVoSWwGHVDjAekQ_AUoAXoECAEQAw&biw=1920&bih=981&dpr=1#imgrc=xD3SnYX7-8NpmM)

## 为什么我们需要

春天的卡夫卡有一个主动听话题的模式(**卡夫卡听众**)。这是我们大多数时候需要的。有些时候，我们不需要主动去听话题。

*   为了测试我们发给[卡夫卡](/javarevisited/top-10-apache-kafka-online-training-courses-and-certifications-621f3c13b38c)的信息是否正确。
*   重试丢失的 Kafka 偏移消息。

所以在上面两种情况下，我们需要得到指定偏移量的消息。

## 现有方法

这可以通过 **Kafka 控制台消费者**(https://Kafka-tutorials . confluent . io/Kafka-console-consumer-read-specific-offsets-partitions/confluent . html)实现。但是在 spring Kafka 中没有简便的方法，实际上也不难编码。

## 春天的卡夫卡路

我们必须用我们的实现来处理这个问题

***剧透预警***

> ***我们没有做任何花哨的工作，我们只是利用春天的卡夫卡消费来获得特定的信息***

我们使用现有的`SpringKafkaConsumerFactory`，并且我们还用 Spring `AbstractConsumerSeekAware` 抽象类扩展了我们的 KafkaCustomer。我们使用 Spring `AbstractConsumerSeekAware` 来管理`ConsumerSeekAware`。侦听器的 ConsumerSeekCallback s。它可以很容易地寻找任意主题/分区，而不必跟踪回调本身。

因为卡夫卡消费者不是线程安全的。我们为此添加了 ReentrantLock。

## KafkaCustomerImplementation

首先，我们试着拿到锁。

```
*final boolean* isAvailable = reentrantLock.tryLock(lockTimeOut, TimeUnit.***SECONDS***);
```

如果我们能够锁定，那么我们将`SpringKakfaConsumer` 分配给一个特定的主题分区。

```
*final Map*<TopicPartition, Long> topicOffsetMap = Collections.*singletonMap*(*new* TopicPartition(topic, partition), topicOffset);
consumer.assign(topicOffsetMap.keySet());
```

然后，我们在`SpringKafkaConsumer` 需要查看主题的位置设置查找偏移量。

```
topicOffsetMap.forEach(consumer::seek);
```

我们恢复消费者投票。

```
consumer.resume(topicOffsetMap.keySet());
*final* ConsumerRecords<K, V> poll = consumer.poll(Duration.*ofSeconds*(maxPollSeconds));
```

一旦`SpringKafkaConsumer` 获得记录，我们验证偏移是否正确。

```
*for* (ConsumerRecord<K, V> kvConsumerRecord : poll) {
    *if* (kvConsumerRecord.offset() == topicOffset) {
        consumerRecord = kvConsumerRecord;
    }
}
```

之后，我们以同步的方式提交消费者。我们暂停主题分区的消费者。

```
consumer.commitSync();
consumer.pause(topicOffsetMap.keySet());
```

一旦发生这种情况，我们释放锁。

```
reentrantLock.unlock();
```

因此，我们将在 KafkaCustomer 的帮助下接收指定的抵销消费者记录。

## 关于代码的信息

**Github
T3【https://github.com/rcvaram/KafkaCustomer】T4**

**Maven 知识库|**

```
<dependency><groupId>io.github.rcvaram</groupId><artifactId>KafkaCustomer</artifactId><version>1.0</version></dependency>
```

## 如何使用

在 spring Kafka maven 或 Gradle 项目中导入依赖项。

使用您的`SpringKafkaConsumerFactory` 创建一个 KafkaCustomer，如下所示。为了简单起见，我使用字符串类作为键和值类，你可以使用你的 Kafka 类。

```
ConsumerFactory<String, String> consumerFactory;            
KafkaCustomer<String, String> kafkaCustomer = new KafkaCustomer<>(consumerFactory, 1000, 5);
```

创建主题分区和偏移映射器

```
TopicPartition topicPartitionZero = new TopicPartition("test",1);
 TopicPartition topicPartitionOne = new TopicPartition("test",0);
 Map<TopicPartition, Long> map= new HashMap();
 map.put(topicPartitionZero, 5);
 map.put(topicPartitionOne, 6);
 Map<TopicPartition, ConsumerRecord<String, String>> OffsetValueMapper = kafkaCustomer.get(map);
```

现在，您可以使用 offsetValueMapper 来获取值。

## 参考

<https://kafka-tutorials.confluent.io/kafka-console-consumer-read-specific-offsets-partitions/confluent.html>    </javarevisited/top-10-apache-kafka-online-training-courses-and-certifications-621f3c13b38c> 