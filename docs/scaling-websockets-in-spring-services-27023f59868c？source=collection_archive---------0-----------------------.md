# 在 Spring 服务中扩展 WebSockets

> 原文：<https://medium.com/javarevisited/scaling-websockets-in-spring-services-27023f59868c?source=collection_archive---------0----------------------->

![](img/27f00e81741166a0aec86961fdb3f81f.png)

由[卡尔·内曾·洛文](https://unsplash.com/@archduk3?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

假设我们有一个简单的聊天应用程序，其中前端通过 rest 和用于聊天的 WebSockets 与后端通信。我们意识到应用程序的一个实例开始无法处理负载。

扩展使用 WebSockets 的微服务不是一件简单的事情。在默认的循环负载平衡器下简单地启动另一个实例，我们可能会遇到这样的情况:一个用户连接到实例 A，第二个用户连接到实例 b。现在，我们的后端必须以某种方式知道将传入的消息发送到哪里。

# **定制负载平衡器**

想到的第一个选择是编写一个智能负载平衡器，将用户从同一个聊天重定向到同一个实例。

这里会出现几个问题:

1.  如果用户同时与许多人通信，那么对于每个聊天，您需要打开一个新的 WebSocket 连接。
2.  如果聊天的人太多，那么一个后端实例可能无法应付。对于聊天来说，这不太可能，但是聊天的例子过于简单了。现实生活中，这个问题并不少见。

# **消息代理**

所以我们走一条不同的路。幸运的是，除了 WebSockets 的内存代理之外， [Spring](/javarevisited/10-best-spring-framework-books-for-java-developers-360284c37036) 还有一个代理中继，它将队列的处理委托给第三方代理。

现在是选择经纪人的时候了。选项很多，我们不会面面俱到。最受欢迎的:

1.  Apache Kafka 不太适合，因为它不是为大量动态生成的队列而设计的。
2.  Redis PUB/SUB 能最好地处理这种负载，但是你不能把它从 Spring 的盒子里拿出来。
3.  其他选项有 RabbitMQ 和 ActiveMQ。

设置代理中继

# **路线命名**

如果我们选择 RabbitMQ，路由命名可能会有问题。如果我们使用标准斜线路径，那么我们可以看到消息:

重点是 RabbitMQ 不允许标准路由后面有斜杠。因此，如果我们向路由`/topic/`或`/queue/`发送消息，那么名称中不应该有其他斜杠。这里最简单的方法是通过用点替换斜线来重命名前端和后端的所有目的地。

如果前端(或另一个通过 WebSockets 连接的应用程序)有不同的发布周期，情况会变得更加复杂。或者您需要保持与其他版本的兼容性。在这种情况下，您可以编写一个拦截器来替换消息中的目的地。

但是您应该记住，Spring 在将订阅发送到 MessageChannel 之前会保存订阅，因此您必须在发送到代理本身和从代理接收的阶段替换目的地。您可以在 [Spring 文档](https://docs.spring.io/spring-framework/docs/4.3.x/spring-framework-reference/html/websocket.html#websocket-stomp-message-flow)中看到完整的通信模式。

这里的另一个解决方案是使用不同的代理。比如 ActiveMQ 就没有这个问题。

# **订阅映射**

如果您的应用程序将控制器中的***@ subscribe mapping***注释用于委托给外部代理(`/topic/`，`/queue/`)的路由，这也将是一个问题。

当 [Spring](/javarevisited/top-10-free-courses-to-learn-spring-framework-for-java-developers-639db9348d25) 接收到带有这种目的地的消息时，它保存订阅，然后绕过控制器将消息发送给代理。因此，用***@ subscribe mapping***标注的方法将永远不会被调用。

这里有两个解决方案。第一个解决方案不需要改变业务逻辑，而第二个解决方案需要:

1.  您可以添加一个 BeanPostProcessor，它将扫描带有该注释的所有方法，并在***session subscribeevent***事件上调用它们。这里的主要问题是，***SessionSubscribeEvent***是在订阅消息发送到代理之前引发的。因此，当我们调用一个向主题发送内容的方法时，可能会出现一种情况，但用户自己还没有订阅这个主题。这个问题的解决方案很棘手，需要通过拦截器等待调度事件。
2.  可以用***@ get mapping***REST 注释替换控制器中的***@ subscribe mapping***。因此，初始状态不是由 WebSockets 获得，而是由 [REST](/javarevisited/top-5-books-and-courses-to-learn-restful-web-services-in-java-using-spring-mvc-and-spring-boot-79ec4b351d12?source=---------17------------------) 获得。这样的解决方案也需要前端方面的改变。

# **结论**

如果在构建应用程序时没有考虑到这一点，那么扩展使用 WebSockets 的微服务可能会更加复杂。主要的解决方案是使用外部消息代理。当您第一次连接消息代理时，应用程序可能无法正常工作。但主要问题只有两个(路线命名，***@ subscribe mapping***)，而且都有解决方案。