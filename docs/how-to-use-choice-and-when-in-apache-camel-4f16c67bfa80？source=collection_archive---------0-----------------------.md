# 如何在阿帕奇骆驼中使用 Choice 和 When？

> 原文：<https://medium.com/javarevisited/how-to-use-choice-and-when-in-apache-camel-4f16c67bfa80?source=collection_archive---------0----------------------->

## 阿帕奇骆驼

Hello🖐我自己[罗汉·拉温德拉·卡达姆](https://medium.com/u/a1b33b7cda75?source=post_page-----4f16c67bfa80--------------------------------)，全栈开发者

欢迎，开发者社区！！在本文中，我们试图理解使用 [Apache Camel](https://javarevisited.blogspot.com/2022/02/top-5-courses-to-learn-apache-camel-in.html) 的异常处理。在直接进入文章之前，让我们试着理解与主题相关的某些问题或疑问。我们可能会问什么是阿帕奇骆驼？为什么用阿帕奇骆驼？如何在阿帕奇骆驼中使用 Choice 和 When？

[![](img/10f8fa82e444ed7f7125f87ad02086d7.png)](https://www.java67.com/2022/02/5-best-apache-camel-courses-for-java.html)

如何在阿帕奇骆驼中使用选择和时间？

嘿，社区，让我们深入到我们的问题中去，找到问题的答案。

## ⚡什么是阿帕奇骆驼？

Apache Camel 是一个基于已知的[企业集成模式](https://camel.apache.org/components/3.15.x/eips/enterprise-integration-patterns.html)的通用开源集成框架。

Camel 使您能够用各种特定于领域的语言( [DSL](https://camel.apache.org/manual/dsl.html) ，比如 [Java](/javarevisited/10-best-places-to-learn-java-online-for-free-ce5e713ab5b2) ，XML， [Groovy](/javarevisited/6-best-resources-to-learn-groovy-and-grails-for-java-developers-18c04e88fa8a) ， [Kotlin](/javarevisited/top-5-courses-to-learn-kotlin-in-2020-dfc3fa7706d8) ，以及 [YAML](https://javarevisited.blogspot.com/2021/11/top-5-courses-to-learn-yaml-in-2022.html) )来定义路由和中介规则。这意味着您可以在 IDE 中智能地完成路由规则，无论是在 Java 还是 XML 编辑器中。

Apache Camel 使用 [URIs](https://camel.apache.org/manual/uris.html) 来直接处理任何类型的传输或消息模型，如 [HTTP](https://camel.apache.org/components/3.15.x/http-component.html) 、 [ActiveMQ](https://camel.apache.org/components/3.15.x/activemq-component.html) 、 [JMS](https://camel.apache.org/components/3.15.x/jms-component.html) 、【JBI】SCA、 [MINA](https://camel.apache.org/components/3.15.x/mina-component.html) 或 [CXF](https://camel.apache.org/components/3.15.x/cxf-component.html) ，以及可插拔的[组件](https://camel.apache.org/manual/component.html)和[数据格式](https://camel.apache.org/manual/data-format.html)选项。Apache Camel 是一个很小的库，具有最少的[依赖性](https://camel.apache.org/manual/what-are-the-dependencies.html)，可以很容易地嵌入到任何 Java 应用程序中。Apache Camel 允许您使用相同的 [API](https://camel.apache.org/manual/exchange.html) 工作，不管使用哪种传输方式——所以学习一次 API，您就可以与开箱即用的所有[组件](https://camel.apache.org/components/3.15.x/index.html)进行交互。

Apache Camel 支持 Bean 绑定以及与 CDI、 [Spring](https://camel.apache.org/manual/spring.html) 等流行框架的无缝集成。Camel 也广泛支持[单元测试](https://camel.apache.org/manual/testing.html)你的路线。

## ⚡为什么使用阿帕奇骆驼？

让我们试着列出 Apache Camel 的一些用例

1.  为**企业集成模式(EIP)** 提供支持
2.  **随处运行** :- Apache Camel 是独立的，可以作为库嵌入到 [Spring Boot](/javarevisited/10-free-spring-boot-tutorials-and-courses-for-java-developers-53dfe084587e) 、 [Quarkus](/javarevisited/10-best-free-dropwizard-vert-x-micronaut-and-quarkus-online-courses-for-java-developers-9c2b4161f17) 、应用服务器和云中。Camel 子项目致力于简化您的工作。
3.  **挤满了组件** :-挤满了数百个组件，用于访问数据库、消息队列、API 或天底下的任何东西。帮助你融入一切。
4.  **支持超过 50 种数据格式** :- Camel 支持大约 50 种数据格式，允许翻译多种格式的消息，并支持金融、电信、医疗保健等行业标准格式。

## ⚡如何使用选择和当在阿帕奇骆驼？

嘿，🖐Community，在文章的前一部分，我们试图解释为什么&阿帕奇骆驼怎么样？在第节中，我们将尝试理解相同的实现。

在直接进入实现之前，不要忘记看看如何使用 Apache Camel 编写第一条路线？从下面的链接

[](https://rohankadam965.medium.com/apache-camel-how-to-write-first-route-7f7767c86c2a) [## 阿帕奇骆驼🦒:如何写第一条路线？

### 你好，我是罗汉·卡达姆😊,

rohankadam965.medium.com](https://rohankadam965.medium.com/apache-camel-how-to-write-first-route-7f7767c86c2a) 

## 步骤 1:如何使用 Spring Initializr 创建项目？

[](https://start.spring.io/) [## 弹簧初始化 r

### Initializr 生成一个 spring boot 项目，其中包含您快速启动所需的内容！

start.spring.io](https://start.spring.io/) 

## 步骤 2:项目中包括哪些依赖项？

[![](img/ba573deeffd99de9bc6a37f6001e038f.png)](https://javarevisited.blogspot.com/2019/03/top-5-course-to-learn-apache-maven-for.html)

功劳:[罗汉·拉温德拉·卡达姆](https://medium.com/u/a1b33b7cda75?source=post_page-----4f16c67bfa80--------------------------------)属地

## 步骤 3:如何在 apache camel 中使用 choice 和 when？

Apache Camel 包含一个强大的特性，叫做**基于内容的路由器。**这允许您根据内容对消息进行不同的处理。

它们与 Java 中的`if/else`语句非常相似。不管怎样，在 Camel 中，对应的词是`when`和`otherwise`。

```
package com.example.apachecameltest.route;

import org.apache.camel.builder.RouteBuilder;
import org.springframework.stereotype.Component;

@Component
public class ChoiceWhenRoute extends RouteBuilder {
    @Override
    public void configure() throws Exception {

        from("direct:startWhenChoiceRoute")
                .routeId("direct:startWhenChoiceRoute")
                .log( "${body}")
                .choice()
                    .when(body().isNull())
                        .log("Message body is empty.")
                .log("${body}")
                .end();
    }
}
```

## 第四步:测试我们的骆驼路线？

为了测试我们的骆驼路线，将使用控制器端点

```
package com.example.apachecameltest;

import org.apache.camel.CamelContext;
import org.apache.camel.Exchange;
import org.apache.camel.ProducerTemplate;
import org.apache.camel.builder.ExchangeBuilder;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class Controler {

    @Autowired
    private ProducerTemplate producerTemplate;

    @Autowired
    private CamelContext camelContext;

    @GetMapping
    public void function() {
        final Exchange
                requestExchange = ExchangeBuilder
                .*anExchange*(camelContext)
                .build();

      producerTemplate
         .send("direct:startWhenChoiceRoute",requestExchange);

    }
}
```

嘿，🖐，要了解更多关于 ProducerTemplate & Camel 的内容，请点击下面的链接

 [## Apache Camel 帮助您集成 300 多个不同的系统！

### ProducerTemplate 接口允许您以各种不同的方式向端点发送消息交换，以使…

camel.apache.org](https://camel.apache.org/manual/producertemplate.html) [](https://camel.apache.org/manual/camelcontext.html) [## Apache Camel 帮助您集成 300 多个不同的系统！

### CamelContext 是作为运行时 Camel 的容器。CamelContext 是运行时系统，它包含…

camel.apache.org](https://camel.apache.org/manual/camelcontext.html) 

## 第五步:骆驼路线输出？

```
 direct:startWhenChoiceRoute              : null
 direct:startWhenChoiceRoute              : Message body is empty.
 direct:startWhenChoiceRoute              : null
```

## 🖐Conclusion:-

在文章中，我们试图在 apache camel 中实现 choice 和 when。我们也试图了解一些关于阿帕奇骆驼的事情。请分享和喜欢💕如果你觉得文章有用。在媒体上关注我，在推特上关注我，关注我，关注我，关注我，关注我，关注我，关注我，关注我

## ⚡Bibliography

[](https://camel.apache.org/) [## 主页

### Camel 支持 Gregor Hohpe 和 Bobby Woolf 的优秀著作中的大多数企业集成模式，并且…

camel.apache.org](https://camel.apache.org/) [](/javarevisited/how-to-implement-exception-handling-using-apache-camel-26faad831e8f) [## 如何使用 Apache Camel 实现异常处理？

### 你好，🖐，我自己，罗汉·拉温德拉·卡达姆

medium.com](/javarevisited/how-to-implement-exception-handling-using-apache-camel-26faad831e8f) [](/javarevisited/how-to-consume-public-api-using-apache-camel-70f7382959ab) [## 如何使用 Apache Camel 消费公共 API？

### Hello🖐我自己 Rohan Ravindra Kadam，全栈开发者

medium.com](/javarevisited/how-to-consume-public-api-using-apache-camel-70f7382959ab) ![](img/8952bc045d848312bfba126ae001248a.png)

谢谢观众们— [罗汉·拉温德拉·卡达姆](https://medium.com/u/a1b33b7cda75?source=post_page-----4f16c67bfa80--------------------------------)