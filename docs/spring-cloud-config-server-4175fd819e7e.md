# Spring 云配置服务器

> 原文：<https://medium.com/javarevisited/spring-cloud-config-server-4175fd819e7e?source=collection_archive---------1----------------------->

*Spring Cloud 配置服务器开发*

![](img/966a385de05b8476ffbb49ed91b9936a.png)

[森迪·纪伯伦](https://unsplash.com/@sendi_r_gibran?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

有很多关于 Spring Boot 和 Spring Cloud 开发细节的网站和帖子，但这篇帖子将只讨论实现和一些关于我做了什么以及为什么这样做的笔记。

## 配置服务器项目

如果您不希望使用 git，而是使用本地路径作为属性文件的位置，那么您应该在 config server 项目中的 spring 概要文件中添加“ **native** ”:

```
spring.profiles.active=native,dev
```

这是我的配置服务器项目的最终 application.properties 文件:

```
server.port=8888
spring.application.name=senoritadev-config-service

spring.profiles.active=native,dev
```

使项目成为配置“服务器”的是这种依赖性:

```
<dependency>
   <groupId>org.springframework.cloud</groupId>
   <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

不要忘记将 [**@EnableConfigServer** 注释](https://www.java67.com/2018/12/top-5-spring-cloud-annotations-for-java.html)添加到您的应用程序类中:

```
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {

   public static void main(String[] args) {
      SpringApplication.*run*(ConfigServerApplication.class, args);
   }

}
```

## 您的主要项目:

在早期版本中，您需要创建一个名为“bootstrap.properties”的单独属性文件，并将下面添加的属性添加到该文件中。“bootstrap.properties”文件与“application.properties”不同，因为它是在主应用程序上下文之前加载的。

因此，转到您将使用这些属性的主项目，并将 spring.application.name 添加到您的“application.properties”中。

```
spring.config.import=optional:configserver:http://localhost:8888
spring.application.name=senoritadev-batch-jobs
spring.profiles.active=dev
```

您需要在 properties 中的 config url 前面添加“optional ”,这样即使出现故障，您的主应用程序仍将启动并运行。

之后，在您的主项目中，只需添加这个依赖项(因为您的主项目是配置服务器的客户端)。

```
<dependency>
   <groupId>org.springframework.cloud</groupId>
   <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

## 配置服务器中主项目的应用程序属性

在您的配置服务器项目中，添加一个名为“senoritadev-batch-jobs**-dev**的文件。属性”与配置服务器自己的 application.properties 文件处于同一级别。注意我是怎么给文件名的:my-batch-app-name+"**-dev**"+"属性”。您可以添加默认的“senoritadev-batch-jobs . properties”以及(环境)配置文件名称(如 senoritadev-batch-jobs**-test**)。属性，高级批处理作业**-预编程**。属性等。).

## 句法

你可以在这里看到属性键 application.properties [的语法示例。"."用于分组，而“-”相当于 Java 类中的大写(“job-count”映射到“jobCount”)。](https://mkyong.com/spring-boot/spring-boot-configurationproperties-example/)

## 配置类

最好有一个单独的 config 类来保存你的属性(在你的主项目中),而不是像在你的类中那样。

对于关注点分离和更新策略来说，这都是一个很好的实践。当您更新属性值并进行刷新以应用时(稍后将提到刷新)，使用这些值的类可能会在刷新期间互相阻塞。

然而，带有@ConfigurationProperties 注释的单个配置 bean 是更快刷新上下文的有效方式。

你可以在这里阅读背景[发生的事情。](https://gist.github.com/dsyer/a43fe5f74427b371519af68c5c4904c7)

## 获取/刷新值

要在配置服务器端更新后刷新项目使用的属性值(手动，不使用 Spring Cloud 配置):

获取项目属性

```
curl -L -X GET ‘http://localhost:8888/senoritadev-batch-jobs/dev'
```

在编辑 senoritadev-batch-jobs-dev . properties 之后，使用这个 [cURL 请求](https://www.java67.com/2017/10/how-to-test-restful-web-services-using.html)在你的项目端刷新(8081 是你的主项目的服务器端口)。作为对此 POST 请求的响应，发送添加/更新的属性列表。

```
curl -L -X POST ‘http://127.0.0.1:8081/actuator/refresh' \
-H ‘Content-Type: application/json’ \
 — data-raw ‘’
```

可以访问我的微服务项目(用 Spring Cloud 实现)GitHub url 获取样例代码:

<https://github.com/senoritadeveloper01/nils-spring-microservices>  

编码快乐！