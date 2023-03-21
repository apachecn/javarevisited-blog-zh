# 有史以来最简单的 SpringCloud 教程|第 3 章:服务消费者(假装)

> 原文：<https://medium.com/javarevisited/the-most-easiest-springcloud-tutorial-ever-chapter-3-service-consumer-feign-finchley-version-dc47e1351feb?source=collection_archive---------1----------------------->

## 第 3 章:服务消费者(假装)

![](img/9680ce7f11bd21f6333817a4939af354.png)

上一篇文章描述了如何通过 RestTemplate+Ribbon 消费服务。本文主要描述如何通过 Feign 消费服务。

1.  **介绍假装**

Feign 是一个声明性的伪 [HTTP 客户端](https://javarevisited.blogspot.com/2015/06/how-to-create-http-server-in-java-serversocket-example.html)，它使得编写 HTTP 客户端更加容易。使用 Feign，您只需要创建一个接口并对其进行注释。它有一个可插入的注释特性，可以使用假造注释和 JAX-RS 注释。Feign 支持可插拔的编码器和解码器。Feign 默认集成 Ribbon，默认与 Eureka 结合实现负载均衡。

一句话:

*   Feign 使用基于接口的注释
*   Feign 将 ribbon 与负载平衡功能集成在一起
*   将 Hystrix 与融合能力相结合

**2。准备工作**

继续使用上一节的项目，启动 eureka-server，端口为 8761；启动 service-hi 两次，端口是 8762 和 8773。

**3。创建一个虚拟服务**

新建一个 [spring-boot 项目](https://javarevisited.blogspot.com/2022/01/spring-boot-reactjs-example-for-java.html)，命名为 serice-feign，并在其 pom 文件中引入 feign 的启动依赖项 spring-cloud-starter-feign，Eureka 的启动依赖项 spring-cloud-starter-网飞-eureka-client，Web Start 依赖于 spring-boot-starter-web，代码如下:

```
<project ae ko" href="http://maven.apache.org/POM/4.0.0" rel="noopener ugc nofollow" target="_blank">http://maven.apache.org/POM/4.0.0" xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
         xsi:schemaLocation="[http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0) [http://maven.apache.org/xsd/maven-4.0.0.xsd](http://maven.apache.org/xsd/maven-4.0.0.xsd)">
    <modelVersion>4.0.0</modelVersion><groupId>com.forezp</groupId>
    <artifactId>service-feign</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging><name>service-feign</name>
    <description>Demo project for Spring Boot</description><parent>
        <groupId>com.forezp</groupId>
        <artifactId>sc-f-chapter3</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent><dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
    </dependencies>

    </project>
```

在项目配置文件 application.yml 文件中，指定程序名为 service-feign，端口号为 8765，服务注册地址为[http://localhost:8761/eureka/](http://localhost:8761/eureka/)，代码如下:

```
eureka:
  client:
    serviceUrl:
      defaultZone: [http://localhost:8761/eureka/](http://localhost:8761/eureka/)
server:
  port: 8765
spring:
  application:
    name: service-feign
```

在程序的启动类 ServiceFeignApplication 中，添加 [@EnableFeignClients](https://www.java67.com/2018/12/top-5-spring-cloud-annotations-for-java.html) 注释来启用 Feign 的函数:

```
[@SpringBootApplicatio](http://twitter.com/SpringBootApplicatio)n
[@EnableEurekaClient](http://twitter.com/EnableEurekaClient)
[@EnableDiscoveryClien](http://twitter.com/EnableDiscoveryClien)t
[@EnableFeignClients](http://twitter.com/EnableFeignClients)
public class ServiceFeignApplication {public static void main(String[] args) {
        SpringApplication.run( ServiceFeignApplication.class, args );
    }
}
```

定义一个假接口，并指定通过@ FeignClient(“服务名”)调用哪个服务。比如代码中调用了 service-hi 服务的“/hi”接口，代码如下:

```
[@FeignClient](http://twitter.com/FeignClient)(value = "service-hi")
public interface SchedualServiceHi {
    [@RequestMapping](http://twitter.com/RequestMapping)(value = "/hi",method = RequestMethod.GET)
    String sayHiFromClientOne([@RequestParam](http://twitter.com/RequestParam)(value = "name") String name);
}
```

在 web 层的[控制器](https://javarevisited.blogspot.com/2022/05/how-to-validate-incoming-payload-on-spring-.html)层，对外公开了一个“/hi”的 API 接口，通过上面定义的 Feign client `ScheduleServiceHi` 消费服务。代码如下所示:

```
[@RestController](http://twitter.com/RestController)
public class HiController { [@Autowired](http://twitter.com/Autowired)
    SchedualServiceHi schedualServiceHi;[@GetMapping](http://twitter.com/GetMapping)(value = "/hi")
    public String sayHi([@RequestParam](http://twitter.com/RequestParam) String name) {
        return schedualServiceHi.sayHiFromClientOne( name );
    }
}
```

启动程序，访问 [http://localhost:8765/hi？name=forezp](http://localhost:8765/hi?name=forezp) 多次，浏览器会交替显示:

```
hi forezp,i am from port:8762hi forezp,i am from port:8763
```

[](/javarevisited/8-best-spring-and-hibernate-training-courses-for-java-developers-acf09aa0e244) [## 2022 年 Java 程序员学习 Spring 和 Hibernate 的 8 门最佳课程

### 这些是 Java 程序员学习 Spring、Spring Boot 和 Hibernate 的最佳在线培训课程。

medium.com](/javarevisited/8-best-spring-and-hibernate-training-courses-for-java-developers-acf09aa0e244) [](/javarevisited/top-10-rest-interview-questions-for-java-and-spring-developers-1611e3b78029) [## Java 和 Spring 开发人员的 10 大 REST 面试问题

### 这些是 Java 和 Spring 开发者快速准备的最好的 REST 面试问题

medium.com](/javarevisited/top-10-rest-interview-questions-for-java-and-spring-developers-1611e3b78029) [](/javarevisited/21-spring-mvc-rest-interview-questions-answers-for-beginners-and-experienced-developers-21ad3d4c9b82) [## 10 大春季 MVC + REST 面试问题解答适合初学者和有经验的开发者

### 大家好。如果你正在准备 Java 和 Spring 面试或 Spring 认证，并经常寻找一些…

medium.com](/javarevisited/21-spring-mvc-rest-interview-questions-answers-for-beginners-and-experienced-developers-21ad3d4c9b82)