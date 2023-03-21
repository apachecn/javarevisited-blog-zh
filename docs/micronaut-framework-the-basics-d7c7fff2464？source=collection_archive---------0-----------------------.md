# Micronaut 框架—基础知识

> 原文：<https://medium.com/javarevisited/micronaut-framework-the-basics-d7c7fff2464?source=collection_archive---------0----------------------->

## 一个基于 JVM 的现代全栈框架，用于构建模块化、易于测试的微服务。

![](img/cf25497ee95d872829c52d17d4a416da.png)

来源:https://micronaut.io/

# 什么是 Micronaut

Micronaut 是一个基于 JVM 的现代框架，用于构建高效、轻量级的微服务应用程序。它提供了一种独特的方法，通过使用提前(AOT)编译来构建微服务，与传统的基于 JVM 的框架相比，这种方法允许更快的启动时间和更低的内存消耗。

Micronaut 的 AOT 编译是通过使用 GraalVM 实现的，这是一个高性能、多语言的虚拟机，支持多种语言，包括 [Java](/javarevisited/10-free-courses-to-learn-java-in-2019-22d1f33a3915) 、 [JavaScript](/javarevisited/10-best-online-courses-to-learn-javascript-in-2020-af5ed0801645) 和 [Ruby](/javarevisited/10-best-ruby-on-rails-courses-for-beginners-dca4d66e9f7b) 。GraalVM 允许 Micronaut 提前编译应用程序，从而加快启动速度，降低内存消耗。

Micronaut 的一个关键特性是它对反应式编程的支持。反应式编程是一种专注于异步数据流和数据变化传播的编程范式。这可以极大地提高微服务应用程序的性能和可伸缩性，因为它允许它们处理大量并发请求，而不会遇到内存或 CPU 限制。

[Micronaut](/javarevisited/10-best-free-dropwizard-vert-x-micronaut-and-quarkus-online-courses-for-java-developers-9c2b4161f17) 还为云原生环境提供一流的支持，使其成为构建旨在 Kubernetes 等平台上运行的微服务的绝佳选择。它提供了与流行的云原生工具和服务的集成，例如用于服务发现的网飞 Eureka 和 Consul，以及用于部署和编排的 Kubernetes 和 Docker。

Micronaut 的一个突出特性是它能够在云本地环境和传统的基于 JVM 的环境中运行。这使得开发人员可以轻松构建可以在内部、云中甚至混合环境中运行的微服务。

总的来说，Micronaut 是构建微服务应用程序的强大而高效的框架。它对反应式编程、云原生环境和 AOT 编译的支持使其成为构建可伸缩、高性能微服务的绝佳选择。

# 你好世界

这个示例创建了一个简单的“hello world”微服务，它侦听指定的端口，并用“hello world”消息响应 HTTP 请求。

首先，我们需要将 Micronaut 依赖项添加到我们的`build.gradle`文件中:

```
dependencies {
  compile "io.micronaut:micronaut-http-server-netty"
}
```

接下来，我们可以通过在我们的主类上使用`@MicronautApplication`注释来创建一个 Micronaut 应用程序:

```
import io.micronaut.runtime.Micronaut;

@MicronautApplication
public class HelloWorldApplication {

    public static void main(String[] args) {
        Micronaut.run(HelloWorldApplication.class);
    }
}
```

接下来，我们可以创建一个简单的控制器，它侦听 HTTP 请求并返回“hello world”消息:

```
import io.micronaut.http.MediaType;
import io.micronaut.http.annotation.Controller;
import io.micronaut.http.annotation.Get;

@Controller("/hello")
public class HelloWorldController {

    @Get(produces = MediaType.TEXT_PLAIN)
    public String index() {
        return "Hello World!";
    }
}
```

最后，我们可以通过将`micronaut.server.port`属性添加到我们的`application.yml`文件来指定微服务应该监听的端口:

```
micronaut:
  server:
    port: 8080
```

有了这个配置，我们的微服务将启动并在端口 8080 上监听 HTTP 请求。当收到一个请求时，`HelloWorldController`将处理它并返回一个“hello world”消息。

# 测试

Micronaut 的一个主要优点是它能够在一个“瘦”JVM 中运行测试，这意味着测试可以快速有效地执行。这是通过使用反射和其他性能增强技术来实现的。此外，Micronaut 还提供了一系列测试实用程序，使得为您的应用程序编写测试变得非常容易。

例如，Micronaut 提供了对[单元测试](https://javarevisited.blogspot.com/2021/04/junit-interview-questions-with-answers.html)的支持，这允许您单独测试应用程序的各个组件。这是通过使用`@MicronautTest`注释来实现的，该注释可以应用于一个测试类来启用 Micronaut 测试基础设施。

Micronaut 还提供了对集成测试的支持，这允许您测试应用程序中多个组件的集成。这是通过使用`@MicronautTest`注释和`@MicronautDataTest`注释来实现的，后者可用于从各种来源加载测试数据，包括内存数据库和外部文件。

除了这些测试功能，Micronaut 还支持编写与外部系统交互的测试，例如[数据库](https://javarevisited.blogspot.com/2018/05/top-5-sql-and-database-courses-to-learn-online.html)和 [HTTP 服务器](https://javarevisited.blogspot.com/2015/06/how-to-create-http-server-in-java-serversocket-example.html)。这是通过使用`@MockBean`注释实现的，它允许您轻松地创建外部依赖的模拟实现，允许您在受控环境中测试您的应用程序。

## 单元测试

单元测试是孤立地测试应用程序的单个组件或模块的测试。在 Micronaut 中，您可以通过用`@MicronautTest`注释来注释您的测试类来编写单元测试，这将启用 Micronaut 测试基础设施。

然后，您可以使用`[@Inject](https://javarevisited.blogspot.com/2017/04/difference-between-autowired-and-inject-annotation-in-spring-framework.html)`注释来注入您想要测试的组件，并使用标准的 JUnit 或 TestNG 断言来验证组件的行为。这里有一个简单的例子:

```
@MicronautTest
public class MyServiceTest {

    @Inject
    MyService myService;

    @Test
    public void testSomeMethod() {
        // Use the @Inject MyService instance to test the myService.someMethod() method
        assertEquals("expected result", myService.someMethod());
    }
}
```

这个测试将使用 Micronaut 测试基础设施来创建一个`MyService`类的实例，然后调用`myService.someMethod()`方法来测试它的行为。`assertEquals()`方法用于验证该方法是否返回预期的结果。

## 集成测试

[集成测试](https://www.java67.com/2022/08/springboottest-integration-test-example.html)是测试应用程序中多个组件或模块的集成的测试。在 Micronaut 中，您可以通过使用`@MicronautTest`和`@MicronautDataTest`注释来注释您的测试类来编写集成测试，这将启用 Micronaut 测试基础设施，并允许您从外部来源加载测试数据。然后，您可以使用`@Inject`注释来注入您想要测试的组件，并使用标准的 JUnit 或 TestNG 断言来验证集成组件的行为。这里有一个简单的例子:

```
@MicronautTest
@MicronautDataTest
public class MyServiceTest {

    @Inject
    MyService myService;

    @Inject
    DataSource dataSource;

    @Test
    public void testSomeMethod() {
        // Use the @MicronautDataTest annotation to load test data from a file
        loadTestData("test-data.sql");

        // Use the @Inject MyService instance and @Inject DataSource instance to test the integration of these components
        assertEquals("expected result", myService.someMethod(dataSource));
    }
}
```

这个测试将使用 Micronaut 测试基础设施来创建`MyService`和`DataSource`类的实例，使用`loadTestData()`方法从指定的文件中加载测试数据，然后调用`myService.someMethod()`方法来测试这些组件的集成。`assertEquals()`方法用于验证该方法是否返回预期的结果。

# 摘要

我希望这个例子能够帮助您了解如何使用 Micronaut 创建一个简单的微服务。当然，Micronaut 的内容远不止这里所展示的，所以请务必查看官方文档以获取更多信息。

Micronaut 的一个关键特性是强调可测试性，这使得为您的应用程序编写测试变得很容易。Micronaut 提供了对单元测试、集成测试和外部系统测试的支持，并提供了一系列测试实用程序，使得编写有效的测试变得容易。通过使用 Micronaut 的测试功能，您可以确保您的应用程序经过良好的测试和可维护性。

*您是否在自己的项目中使用过 Micronaut 及其测试功能？*

你的经历是怎样的？