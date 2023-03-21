# 反应式 Spring Boot 应用程序的体系结构

> 原文：<https://medium.com/javarevisited/architecture-of-a-reactive-spring-boot-application-197bbfe3af78?source=collection_archive---------0----------------------->

典型弹簧靴反应式应用中的元件。

![](img/d0f98fdedc54f1a4d98d0c61355e5701.png)

由[杰森·布里斯科](https://unsplash.com/@jsnbrsc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/architecture-modern?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

Spring boot 支持在引擎盖下使用 project reactor 构建反应式应用程序，这意味着与传统的非反应式 Spring Boot 应用程序相比，反应式 spring boot 应用程序的架构略有改变。在这篇文章中，我想谈谈反应式 Spring Boot 应用程序的不同组成部分，每一层的术语和不同选项。

# 路由器功能

路由器函数定义了应用程序的入口点。它定义了应用程序公开的路由(也称为 API ),这些路由反过来又作为单独的入口点。除了 API(由路径和 HTTP 动词组成)，它还指定了请求的处理，即当接收到与特定 API 匹配的请求时会发生什么。通常，API 请求的处理是由链中的下一个组件完成的，即处理程序。

# 处理者

Handler 方法处理 ServerRequest 对象并返回 ServerResponse 对象(在反应式应用程序的情况下，Handler 方法将返回一个`Mono<ServerResponse>`)。处理程序不需要执行将传入的 ServerRequest 转换为`Mono<ServerResponse>`的所有步骤，但是它是使用其他组件和服务进行处理的组件。

# 其他组件

处理程序下面是应用程序中的所有其他组件，即应用程序中可以在整个应用程序中重用的其他 beans。这一层中的组件通常相互交换使用。所有其他组件中的大多数通常属于以下类别之一:

## 服务

如果 API 请求的处理需要与多个组件对话，并且可以跨代码库重用，那么我喜欢创建一个服务类，它成为代码中的一个单独的位置，在这里所有这些组件可以组合起来并用于生成输出。服务类有助于隔离测试某些处理步骤，并有助于代码重用和维护。服务类可以使用本节中的其他类，包括客户端、计算单元、其他服务、储存库和计算类。

## *客户端*

当一个组件负责对其他服务/第三方 API 进行 API 调用时，我喜欢将这样的类称为客户端(因为它们是它们进行 API 调用的那些服务的客户端)。在反应式 Spring 的情况下，您可能会使用 Spring 的 WebClient 来进行 API 调用。本质上不需要创建一个专门用于与其他 API 通信的组件。我喜欢这样把它们分开，因为:

*   彻底测试这个组件的错误和快乐路径场景变得很容易。
*   特定 API 的所有错误处理都可以在该组件中以隔离的方式发生。
*   将来对第三方 API 所做的任何更改都意味着与该 API 交互的客户端将是唯一需要更改的组件。

它有助于为应用程序与之交互的每个第三方 API 提供一个客户端类，如果以这种方式建模，客户端类可以使用实用程序类、计算类和存储库(很少使用其他服务/客户端类)

## *实用程序/助手类*

无论您正在构建什么样的应用程序，您都有可能至少拥有一个负责所有验证的组件(例如)。有时，可能有多个验证器来验证应用程序处理的不同数据集。虽然验证器是一种实用程序类，但是还有其他类型属于这一类别(这不是唯一的列表):

*   变压器(例如:dto/svo->实体，反之亦然)
*   格式化程序(例如:在数据被发送到表示层之前在数据上运行)
*   错误处理程序

如果实用程序类可以分解成需要验证的数据(大多数情况下都可以这样做)，那么实用程序类可能不需要依赖于应用程序中的任何其他类(除了其他实用程序类)。

## 贮藏室ˌ仓库

存储库是 spring 接口/类，它们定义了与数据存储(可能是关系数据库、文档存储等)交互的契约(有时是实现)。这些与之前描述的任何组件都不相同，因为 spring-data-jpa 提供了一个选项来定义标记为存储库的接口中的函数/方法，这些方法的实现由 spring 负责(也称为[查询方法](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.query-methods.details))。定义存储库组件最简单的选择是从 spring-data 的 CrudRepository 扩展。

存储库类就像客户端一样，通常存在到实体(或数据库中的表)的一对一映射。仓库*通常*不依赖于任何其他 spring 组件。

博文中提到的组件只是提供一个总体示例，并没有涵盖所有可能的场景。Spring Boot 的反应式堆栈与非反应式堆栈的不同之处不仅在于可用于处理请求的操作符，还在于处理请求的方式。我希望这篇文章为您的 Spring Boot 支持的反应式应用程序提供一个良好的开端！