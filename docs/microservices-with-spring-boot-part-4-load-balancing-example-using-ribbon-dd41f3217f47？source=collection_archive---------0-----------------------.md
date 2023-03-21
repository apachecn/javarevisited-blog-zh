# Spring Boot 微服务—第 4 部分—负载平衡示例—使用 Ribbon

> 原文：<https://medium.com/javarevisited/microservices-with-spring-boot-part-4-load-balancing-example-using-ribbon-dd41f3217f47?source=collection_archive---------0----------------------->

我们来学习一下微服务和微服务架构的基础知识。我们还将开始研究 Spring Boot 微服务的基本实现。我们将创建几个微服务，并让它们使用 Eureka 命名服务器和 Ribbon 进行客户端负载平衡。

以下是微服务系列大纲:Spring Boot 微服务

*   第 1 部分— [微服务架构入门](https://www.springboottutorial.com/creating-microservices-with-spring-boot-part-1-getting-started)
*   第二部分— [打造外汇微服务](https://www.springboottutorial.com/creating-microservices-with-spring-boot-part-2-forex-microservice)
*   第 3 部分— [创建货币兑换微服务](https://www.springboottutorial.com/creating-microservices-with-spring-boot-part-3-currency-conversion-microservice)
*   当前零件-零件 4-使用 Ribbon 进行负载平衡
*   第 5 部分— [使用 Eureka 命名服务器](https://www.springboottutorial.com/microservices-with-spring-boot-part-5-eureka-naming-server)

这是本系列的第 4 部分。在这一部分中，我们将重点介绍如何使用 Ribbon 进行负载平衡。

# 你会学到的

*   负载平衡的需求是什么？
*   什么是丝带？
*   你是如何给你的 Spring Boot 项目添加彩带的？
*   如何启用和配置 Ribbon 来实现负载平衡？

# 免费课程—10 步学会

*   [免费 5 天挑战——学习春天和 Spring Boot](https://links.in28minutes.com/SBT-Page-Top-LearningChallenge-SpringBoot)
*   [用 10 个步骤学习 Spring Boot](https://links.in28minutes.com/in28minutes-10steps-springboot)
*   [10 步学会 Docker](https://links.in28minutes.com/in28minutes-10steps-docker)
*   [十步学会 Kubernetes](https://links.in28minutes.com/in28minutes-10steps-k8s)
*   [用 10 个步骤学习 AWS](https://links.in28minutes.com/in28minutes-10steps-aws-beanstalk)

# 微服务概述

在前两部分中，我们创建了微服务，并在它们之间建立了通信。

转到[http://localhost:8100/currency-converter-feign/from/EUR/to/INR/quantity/10000](http://localhost:8100/currency-converter-feign/from/EUR/to/INR/quantity/10000)

```
{
  id: 10002,
  from: "EUR",
  to: "INR",
  conversionMultiple: 75,
  quantity: 10000,
  totalCalculatedAmount: 750000,
  port: 8000,
}
```

> 当我们执行上述服务时，您会看到一个请求也被发送到外汇服务。

太酷了！

我们现在已经创建了两个微服务，并在它们之间建立了通信。

![](img/34a0aab24f4e5308a62629e8e9b2aca9.png)

然而，我们在 CCS 组件`CurrencyExchangeServiceProxy`中硬编码 FS 的 url。

```
@FeignClient(name="forex-service" url="localhost:8000")
public interface CurrencyExchangeServiceProxy {
  @GetMapping("/currency-exchange/from/{from}/to/{to}")
  public CurrencyConversionBean retrieveExchangeValue
    (@PathVariable("from") String from, @PathVariable("to") String to);
}
```

这意味着当外汇服务的新实例启动时，我们没有办法向它们分配负载。

在这一部分中，现在让我们使用 Ribbon 来启用客户端负载分布。

# 您将需要的工具

*   Maven 3.0+是您的构建工具
*   你最喜欢的 IDE。我们使用 Eclipse。
*   JDK 1.8 以上

# 用代码示例完成 Maven 项目

> 我们的 Github 存储库中有所有的代码示例—[https://Github . com/in28 minutes/spring-boot-examples/tree/master/spring-boot-basic-microservice](https://github.com/in28minutes/spring-boot-examples/tree/master/spring-boot-basic-microservice)

# 启用功能区

向 pom.xml 添加功能区依赖项

```
<dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
    </dependency>
```

在 CurrencyExchangeServiceProxy 中启用 RibbonClient

```
@FeignClient(name="forex-service")
@RibbonClient(name="forex-service")
public interface CurrencyExchangeServiceProxy {
```

在 application.properties 中配置实例

```
forex-service.ribbon.listOfServers=localhost:8000,localhost:8001
```

# 在 8001 上推出外汇服务

在上面的步骤中，我们配置了 ribbon 来将负载分配给实例。但是，我们没有在 8001 上运行外汇服务的任何实例。

我们可以通过配置启动配置来启动它，如下图所示。

![](img/300c67f25635ad7edad7f5bb81577652.png)

# 丝带在行动

目前，我们已经启动并运行了以下服务

*   8100 上的货币兑换微服务(CCS)
*   8000 和 8001 上的两个 Forex 微服务实例

现在，您会看到对 CCS 的请求将通过 Ribbon 在 Forex 微服务的两个实例之间分发

# 请求 1

转到[http://localhost:8100/currency-converter-feign/from/EUR/to/INR/quantity/10000](http://localhost:8100/currency-converter-feign/from/EUR/to/INR/quantity/10000)

```
{
  id: 10002,
  from: "EUR",
  to: "INR",
  conversionMultiple: 75,
  quantity: 10000,
  totalCalculatedAmount: 750000,
  port: 8000,
}
```

# 请求 2

转到[http://localhost:8100/currency-converter-feign/from/EUR/to/INR/quantity/10000](http://localhost:8100/currency-converter-feign/from/EUR/to/INR/quantity/10000)

```
{
  id: 10002,
  from: "EUR",
  to: "INR",
  conversionMultiple: 75,
  quantity: 10000,
  totalCalculatedAmount: 750000,
  port: 8001,
}
```

您可以看到两个响应中的端口号是不同的。

# 摘要

我们现在已经创建了两个微服务，并在它们之间建立了通信。

![](img/f1b0be73b713f163222faff0aaf9e60b.png)

我们使用 Ribbon 在 Forex 服务的两个实例之间分配负载。

然而，我们在 CCS 中硬编码了 FS 的两个实例的 URL。这意味着每当有一个新的 FS 实例时，我们都需要更改 CCS 的配置。那不酷。

在下一部分中，我们将使用 Eureka 命名服务器来解决这个问题。

# 后续步骤

继续微服务系列—Spring Boot 微服务

*   第 1 部分— [微服务架构入门](https://www.springboottutorial.com/creating-microservices-with-spring-boot-part-1-getting-started)
*   第 2 部分-[创建外汇微服务](https://www.springboottutorial.com/creating-microservices-with-spring-boot-part-2-forex-microservice)
*   第 3 部分-[创建货币兑换微服务](https://www.springboottutorial.com/creating-microservices-with-spring-boot-part-3-currency-conversion-microservice)
*   当前部分—第 4 部分—使用功能区进行负载平衡
*   第 5 部分— [使用尤里卡命名服务器](https://www.springboottutorial.com/microservices-with-spring-boot-part-5-eureka-naming-server)