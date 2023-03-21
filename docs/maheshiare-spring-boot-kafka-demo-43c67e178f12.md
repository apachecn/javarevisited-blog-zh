# 春靴+阿帕奇卡夫卡

> 原文：<https://medium.com/javarevisited/maheshiare-spring-boot-kafka-demo-43c67e178f12?source=collection_archive---------2----------------------->

#春天-靴子-卡夫卡-演示

这是一个简单的 spring 应用程序，使用 Spring boot 开发，演示了 Apache Kafka 的集成，它在内部用于将不同的 Kafka 实例作为一个集群进行管理。

**主题**:主题是生产者发布数据流的唯一表示或类别。一个主题将被零个或多个消费者订阅以接收数据。

**生产者**:生产者负责向一个或多个主题发送数据，并将数据分配给主题内的分区。

**消费者**:当生产者将数据发送给主题时，消费者负责消费来自一个或多个主题的数据。

**Apache Kafka 有一个内置的系统，如果在处理数据时出现任何故障，它会重新发送数据，通过这个内置的机制，它具有高度的容错能力。**

**此应用程序需要 Kafka 和 Zookeeper 设置。**可以从这里下载 Kafka 设置 Kafka[**-2 . 8 . 0-src**](https://kafka.apache.org/downloads)。下载后，解压到您的本地机器。如果您使用的是 Mac，请导航到提取的文件夹，并使用 windows Powershell/terminal 运行以下命令。

运行 zookeeper 服务器

```
.\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties (Windows) .\bin\zookeeper-server-start.sh .\config\zookeeper.properties (linux)
```

运行 Kafka 实例

```
.\bin\windows\kafka-server-start.bat .\config\server.properties (Windows) .\bin\kafka-server-start.sh .\config\server.properties (linux)
```

运行上述命令时，您可能会遇到以下问题:

将以下依赖项添加到 pom.xml 中

Spring boot 消除了为 Kafka 消费者/生产者编写配置类的所有锅炉模板代码，只需将下面的配置添加到您的 application.properties/application.yml 文件中

```
spring.kafka.topic=<topic-name> 
spring.kafka.topic.group.id=<consumer-group-id> ## kafka consumer config ## 
spring.kafka.consumer.bootstrap-servers=localhost:9092 spring.kafka.consumer.group-id=${spring.kafka.topic.group.id} spring.kafka.consumer.auto-offset-reset=earliest spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer 
spring.kafka.consumer.value-deserializer=org.springframework.kafka.support.serializer.ErrorHandlingDeserializer spring.kafka.consumer.properties.spring.deserializer.value.delegate.class: org.springframework.kafka.support.serializer.JsonDeserializer spring.kafka.consumer.properties.spring.json.value.default.type=com.java.techhub.kafka.demo.model.UserDetails ## kafka producer config ## 
spring.kafka.producer.bootstrap-servers=localhost:9092 spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer spring.kafka.producer.value-serializer=org.springframework.kafka.support.serializer.JsonSerializer 
## Optional configuration ## 
spring.kafka.producer.client-id=producer-${random.uuid} spring.kafka.consumer.client-id=consumer-${random.uuid}
```

项目的核心组成部分

```
MessagePublishController - > POST - /api/publish - To publish message to a topic with payload as user details object MessagePublisher - > Service used for publishing messages to Kafka topic using KafkaTemplate MessageConsumer - > Listener for consuming the messages from kafka topic, uses @KafkaListener annotation @EnableKafka - > Annotation used for activating methods annotated with @KafkaListener annotation as message consumers
```

如果您有与项目相关的问题，请在此处用您的问题创建票证[创建问题](https://github.com/MaheshIare/spring-boot-kafka-demo/issues)

马赫什·库马尔·古塔姆

请随时给我发送一些反馈或问题！

*原载于*[*https://github.com*](https://github.com/MaheshIare/spring-boot-kafka-demo)*。*