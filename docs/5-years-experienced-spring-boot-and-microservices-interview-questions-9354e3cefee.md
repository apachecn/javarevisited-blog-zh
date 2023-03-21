# 5 年以上经验丰富的春季开机和微服务面试问题

> 原文：<https://medium.com/javarevisited/5-years-experienced-spring-boot-and-microservices-interview-questions-9354e3cefee?source=collection_archive---------0----------------------->

正如我在之前的帖子中承诺的，我将分享一些高级主题的面试问题，如 spring boot 和 microservices，这里有一些我在许多面试中反复遇到的问题…

**Spring Boot:**

1.  为什么要用弹簧靴？spring boot 和 spring MVC 的区别？
2.  [@service 和@repository 注解的区别](/@mktechm/difference-between-service-and-repository-annotation-in-spring-boot-bc260f49a3a1)？@controller vs @RestController 注释？试着准备并理解 spring boot 中使用的所有注释。有时人们会要求解释所有关于 spring boot 的重要注释。[(答案)](/@mktechm/difference-between-service-and-repository-annotation-in-spring-boot-bc260f49a3a1)
3.  解释@SpringBootApplication 注释，这是一个非常基本的问题，但有多个后续问题，因此请尝试在内部理解该注释。有时面试官也会问 main 方法中 run()方法的内部工作原理。
4.  你是如何在你的 spring boot 应用程序中进行概要分析的？这个问题基本上是想知道你真的做过 spring boot 项目。虽然这是一个非常简单的问题，但对面试来说却非常重要。
5.  在 spring boot 中实现 saveEmployee()方法。现在，一天的面试官试图通过给出一个小的用例来检查实际操作，比如从控制器到存储层实现 getEmployee()或 saveEmployee()，在后续工作中，他们还会要求处理异常。所以要为 spring boot 中的异常处理场景做好准备，比如如何使用@ControllerAdvice 和@ExceptionHandler 注释。
6.  如何用 spring 数据 jpa 实现分页？
7.  春季开机如何更改默认服务器和端口？
8.  PUT 和 PATCH http 方法的区别？
9.  @PathParam 和@RequestParam 和@QueryParam 批注的区别？尝试理解使用这些注释的场景。
10.  使用不同的 http 状态代码，如 404、403、401、500、502 等。

**微服务:**

1.  什么是微服务？所有的微服务面试都是从这个问题开始的。这非常简单，但很难解释，所以请准备好关于微服务架构的解释，您也可以通过解释单片和微服务架构之间的差异来解决这个问题。
2.  您将采用什么方法将整体应用迁移到微服务？

3.微服务服务 A 调用服务 B，服务 B 调用服务 C，如果服务 B 关闭或没有响应，那么在微服务中如何处理这种情况，或者哪种设计模式适合这种情况？

4.如何为你的微服务实现服务发现和 Api 网关？尤里卡服务器内部是如何工作的？服务发现可能会有多个后续问题，如 Eureka server 中的心跳是什么？一个服务如何自动注册到 Eureka 服务器？eureka 服务器的默认端口是什么？如何关闭从尤里卡服务器获取注册表？为什么我们需要关闭从服务器端获取注册表？Api 网关在微服务中有什么意义。？

5.如何在微服务架构中实现分布式日志记录？

6.微服务架构如何处理事务？

7.在微服务中，我们在哪一层实现安全性？

在这里，我添加了一些我最近遇到的关于 spring boot 和微服务的问题，在下一篇帖子中，我将尝试创建一个与编码问题相关的帖子。谢谢，祝学习愉快。

其他 **Spring Boot 和微服务文章** yo 可能喜欢:

[](/javarevisited/10-best-java-microservices-courses-with-spring-boot-and-spring-cloud-6d04556bdfed) [## 2021 年 Spring Boot 和 Spring Cloud 的 10 个最佳 Java 微服务课程

### 我最喜欢的 2021 年用 Spring Boot 和 Spring Cloud 为初学者学习 Java 微服务的在线课程来自…

medium.com](/javarevisited/10-best-java-microservices-courses-with-spring-boot-and-spring-cloud-6d04556bdfed) [](https://javarevisited.blogspot.com/2021/09/microservices-design-patterns-principles.html) [## 十大微服务设计模式和原则-示例

### 让我们看看微服务架构构建的原则。1.可扩展性 2。灵活性 3…

javarevisited.blogspot.com](https://javarevisited.blogspot.com/2021/09/microservices-design-patterns-principles.html) [](/javarevisited/10-advanced-spring-boot-courses-for-experienced-java-developers-5e57606816bd) [## 面向有经验的 Java 开发人员的 10 门高级 Spring Boot 课程

### 高级 Spring Boot 课程为有经验的 Java 开发人员学习 Spring Boot 测试，云和容器…

medium.com](/javarevisited/10-advanced-spring-boot-courses-for-experienced-java-developers-5e57606816bd)