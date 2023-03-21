# Vert.x:理解核心概念

> 原文：<https://medium.com/javarevisited/vert-x-understanding-core-concepts-1529917658b3?source=collection_archive---------0----------------------->

![](img/5c1798f0b72e083536aa21188e60d01b.png)

今天深入探讨在 JVM 上构建反应式应用程序的框架:Vert.x
在这篇文章中，我们将介绍 Vert.x 的主要概念，并为希望了解一些具体实现的开发人员提供示例。

每个例子的源代码都可以在[https://github.com/teivah/vertx-tutorial](https://github.com/teivah/vertx-tutorial)获得

# 概观

Vert.x 是 Tim Fox 在 2011 年开始的项目。它仍然是 Eclipse 基金会的一个官方项目，但是由红帽公司(T4)领导，该公司将 Vert.x 视为一个战略项目。

Vert.x 是一个基于 JVM 的框架，允许开发者实现**自主**组件。这些组件通过**事件总线**发送消息，以弱耦合的方式相互通信。

Vert.x 提供了一种**多语言**的方法，因为开发人员可以自由地在一系列语言中实现他们的组件: [Java](https://dev.to/javinpaul/10-free-courses-to-learn-java-in-depth-3ikn) 、 [JavaScript](https://javarevisited.blogspot.com/2018/06/top-10-courses-to-learn-javascript-in.html) 、 [Groovy](https://javarevisited.blogspot.com/2017/08/top-5-books-to-learn-groovy-for-java.html#axzz5dPh77Fzl) 、 [Ruby](http://www.java67.com/2018/02/5-free-ruby-and-rails-courses-to-learn-online.html) 和 Ceylon。

同样，Vert.x 提供了基于非阻塞和回调方法实现**反应式应用**的功能。

Vert.x 经常被比作 [JVM](http://www.java67.com/2016/08/10-jvm-options-for-java-production-application.html) 的 [Node.js](https://www.java67.com/2019/07/top-5-free-nodejs-courses-for-web-development.html) ，但是我们将看到两个框架之间的主要区别。

# 反应堆模式和事件循环

正如 Node.js 中实现和推广的那样，Vert.x 基于**反应器模式**。

反应器模式是一种解决方案:

*   处理**并发**事件(连接、请求或消息)。
*   基于同步和阻塞**解复用器**(也称为**事件循环**)监听输入事件。
*   **如果操作可以以**异步**方式执行，则将**事件分派给适当的处理程序。

通常实现一个处理程序来管理诸如 I/O、数据库连接、 [HTTP 请求](https://javarevisited.blogspot.com/2017/06/how-spring-mvc-framework-works-web-flow.html)等操作

这种模式经常被用来为 **C10k 问题**提供解决方案。这个问题可以非常简单地描述:一台服务器如何同时处理**万个连接**？

在这种情况下，由于物理和/或操作系统的限制(例如过多的线程上下文切换)，为每个请求创建一个专用线程的传统方法不再可行。
相比之下，反应器模式提供了一个带有**单线程**事件循环的模型。

是的。**一个线程**管理 10k 个连接。如果你不熟悉这样的概念，这是相当具有破坏性的，不是吗？
一个线程管理传入的事件并将它们分派给异步处理程序。

不过，Vert.x 方法略有不同。让我们想象一下，现在您想要通过添加更多 CPU 内核来扩展您的计算机。如果您只保留一个线程，您将无法从这些新资源中受益。

不过，使用 Node.js，您通常可以通过在每个内核上实现一个进程来绕过这一限制。

相比之下，Vert.x 允许您**配置**并定义实例化事件循环的数量。这显然是一个可爱而强大的选择。尽可能提高应用程序性能的最佳实践是设置尽可能多的可用 CPU 内核。

值得一提的是，作为一名开发人员，你必须**非常小心**管理事件循环的方式。使用传统方法，如果一个线程被阻塞；这当然不是一件好事，但它不会影响许多其他线程。

使用反应堆方法，一个阻塞的线程将简单地导致灾难，因为您将影响所有正在进行的和即将发生的事件。

此外，Vert.x 还提供了各种各样的**非阻塞 API** 。你可以使用 Vert.x 进行非阻塞的 web 操作(基于 Netty)、文件系统操作、数据访问(例如 [JDBC](http://www.java67.com/2018/03/top-5-free-jdbc-courses-for-java.html) 、 [MongoDB](https://javarevisited.blogspot.com/2019/01/top-5-mongodb-online-training-courses.html) 和 PostgreSQL)等等……
还有一个标准的方法，我们将在下面描述，直接从事件循环中管理阻塞操作。

# 垂直模型

正如概述中提到的，Vert.x 提出了一个基于自治和弱耦合组件的模型。这种类型的组件被称为 vert . x a**vertical**，可以用我们见过的不同语言编写。每个 verticle 都有自己的类加载器。

基于垂直市场开发 Vert.x 应用程序并不是强制性的，但是强烈建议采用这种模式。

verticle 基本上是一个实现两种方法的对象: *start()* 和 *stop()* 。
有三种不同类型的垂直市场:

*   **标准**:基于事件循环模型的垂直。如上所述，Vert.x 允许配置事件循环实例的数量。
*   **工人**:基于经典模型的垂直。定义了一个线程池，每个传入的请求将消耗一个可用的线程。
*   **多线程工作者**:基本工作者模型的扩展。多线程工作线程由一个实例管理，但可以由几个线程同时执行。

在下面的例子中，我们将展示如何配置和实例化每个垂直类型的具体例子。

垂直方法与 **Actor 模型**有一些相似之处，但不是它的完全实现。
我不会深究这些概念(尽管详细介绍 Vert.x 和 Actor 兼容框架(如 Akka)之间的差异，但这将是未来帖子的一个好主意)。如果我们仅仅坚持在[反应式消息传递模式和参与者模型](https://github.com/VaughnVernon/ReactiveMessagingPatterns_ActorModel)一书中给出的参与者的三个主要原则:

*   向其他参与者发送有限数量的**消息**:我们将在下一部分中看到，垂直对象使用消息以异步方式一起通信。
*   创造有限数量的**新演员**:在我的理解/感知中，这不是 Vert.x 的哲学，每个垂直都是由一个中心过程部署的。Vert.x 提出的模型并不是在垂直市场之间建立具体的依赖关系，从而形成一种家长必须监督其孩子的监督系统。
*   指定它接收到的下一条消息的行为:根据我的理解，verticles 是为无状态组件而设计的。它们不保持它们的状态来修改例如它们的行为，为将来的消息做准备。仍然可以手动实现它，但是没有 Vert.x 标准来实现它。

# 事件总线

最后一个核心概念是**事件总线**。可以想象，事件总线是以弱耦合的方式在不同的垂直领域之间交换**消息**的支持。

每条消息都被发送到一个逻辑地址。例如: *customer.create*

事件总线支持两种标准模式类型:**点对点**和**发布/订阅**。使用点对点模式，如果接收者必须回复初始消息，您还可以设法实现请求-回复。

需要理解的重要一点是，事件总线只实现了**尽力交付。**
这意味着无法通过在磁盘上存储消息来管理**保证交付**。所以确实有可能丢失消息。
同样，也没有能力实现**持久订阅者**。这意味着，如果在发布消息时消费者没有连接到事件总线，那么它将永远无法检索到消息。

在您的 Vert.x 应用程序中，事件总线可能是同步和异步交换的标准，而不必保证交付。
否则，应该使用第三方消息中间件。

如果您的 Vert.x 应用程序运行在一个 JVM 节点上，那么就没有物理通道。它仍然只是一个消息传递端点**堆上**。
如果您的应用程序是集群化的，那么可以通过 **TCP 通道**获得一个事件总线端点。但是这种逻辑对于开发人员来说是不可见的。使用发送/发布 API 的方式没有什么不同。Vert.x 可以帮你做到这一点。

最后一个精度，到目前为止现有版本(3.2.1 之前)，事件总线 TCP 通信是**不加密**。基于路线图，将从 3.3 版本开始实施(约 2016 年 6 月)。

# 我们来编码吧！

这篇文章的理论已经足够了。现在让我们看一些具体的例子:

*   在第一个和第二个例子中，我们将介绍实现 REST API 的基本概念。
*   在第三篇文章中，我们将描述如何用**组合**异步方法。
*   在第四个例子中，我们将看到如何在**事件总线**上发布、发送和接收消息。
*   最后一个例子将展示如何创建和**部署**垂直。

# 创建一个基本的休息服务

[Github 示例](https://github.com/teivah/vertx-tutorial/tree/master/src/main/java/org/enterpriseintegration/vertx/tutorial/examples/example01)

在这个例子中，我们将在 REST 资源上创建一个简单的方法。

我们将在最后的例子中看到如何部署垂直市场。到目前为止，您只需注意，如果您需要创建自己的 verticle，您必须扩展 *AbstractVerticle* 类。

在这个类中，为了管理部署和取消部署部分，您可以重写这四个方法:

*   *开始()*
*   *开始(未来)*
*   *停止()*
*   *停止(未来<作废)*

当部署实例时，Vert.x 调用 *start* 方法，当取消部署实例时，调用 *stop* 方法。
通过使用带有[*Future*](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html)*<Void>*参数的方法，您可以异步指示实例已经成功部署。

从 *vertx* 对象中检索出*路由器*对象。这个对象是一个受保护的对象，继承自 *AbstractVerticle* 。

我们使用 *route* 在带有 *name* 参数的 */hello* 资源上公开一个新的 GET 方法。

如果这个 verticle 是一个标准的 verticle，那么实现部分将基于事件循环机制被调用。这意味着执行**绝不能被阻塞**。正如我们提到的，每次可以异步管理操作时，我们都需要将它分派给一个专用的处理程序。
*routing context*对象是一个包含请求上下文的 lambda。

从 *routingContext* 中，我们检索传入的请求和我们将要格式化的响应。

我们根据输入的*名称*参数创建一个新的字符串。

然后，响应被格式化(内容、内容类型等)。

最后，当响应被格式化后，我们调用 *end()* 方法来指示 *HttpServerResponse* 发送回一个回复。

配置完第一条路由后，我们现在想在 8080 上创建一个监听 HTTP 端口。
基于这种配置，我们可以向 *startFuture* future 指示垂直是否已经正确开始。
如果没有特别的问题(比如端口没有被使用)，我们调用 *complete()* 方法。否则我们调用 *fail(Throwable)* 。

就是这样！您刚刚创建了您的第一个 REST 服务。

# 操纵 Json 数据并执行阻塞代码

[Github 示例](https://github.com/teivah/vertx-tutorial/tree/master/src/main/java/org/enterpriseintegration/vertx/tutorial/examples/example02)

在这个例子中，我们将公开一个 POST 方法，但是我们也将看到如何使用 Vert.x API 操作 JSON，以及如何管理来自 verticle 的阻塞代码。

第一部分是相似的，除了这里我们在一个 */file* 资源上公开了一个 POST 方法。

Vert.x 核心 API 提供了操纵 JSON 数据的机制。
在这种情况下，我们简单地请求 *routingContext* 将正文格式化为 *JsonObject* 。
我们使用 *getString(String)* 方法创建一个*文件名*字符串。

与之前相同的警报，因为我们正在实现一个事件循环，我们必须小心不要阻塞它。
然而，现在让我们想象一下**你必须调用一个阻塞方法**，例如，读取一个文件(即使对于 I/O 操作，我们将在下一个例子中看到 Vert.x 为此提供了一个非阻塞 API)或其他任何东西，但仍然基于阻塞 API。

通过简单地将阻塞部分封装在异步处理程序中，Vert.x 允许从您的事件循环实现中做到这一点。

在我们的例子中，我们在 future 中封装了一个阻塞的 *parseLargeFile()* 方法。

在这种情况下， *res* 对象是包含前一未来的异步结果的 *AsyncResult* 。
这里我们设置了一个特定的处理程序，以防未来成功，否则就设置另一个处理程序。

# 编排异步执行

[Github 示例](https://github.com/teivah/vertx-tutorial/tree/master/src/main/java/org/enterpriseintegration/vertx/tutorial/examples/example03)

在这个例子中，我们将展示编排异步执行的不同方法。

假设我们已经实现了这三种方法:

现在，每种方法都基于 Vert.x 提供的非阻塞 API。

下一个挑战将是协调他们。

在同步模式下，你可以做类似这样的事情:*copy file(writeFile(readFile))*

当然，对于异步开发，这已经不可能了。有两种方法可以做到。让我们看看第一个:

很啰嗦吧？让我们深入研究代码。

第一个动作是检索由 *readFile()* 方法发回的 *Future* 对象。

剩下的代码基本上就是这些模式的组合。如果未来成功，我们建立一个处理程序，如果失败，我们建立另一个。使用 *result()* 方法检索先前执行的结果

从 3.2.1 开始，有了另一种更简单的方法来编排异步函数:

现在重复出现的模式如下:

compose 方法接受一个参数，在执行中调用的一个*处理程序*成功，另一个失败(如果有)将被传播。
乍一看，这种实现似乎不太自然，但就代码行数而言，它比使用 *setHandler()* 的第一种实现要便宜 30%左右。

最后但同样重要的是，值得注意的是， *CompositeFuture* 对象提供了静态方法来实现处理程序，例如完成未来列表。如果你所有的未来都是独立的(一个未来不依赖于另一个未来的执行)，这可能是有用的。

# 使用事件总线发送和接收消息

[Github 示例](https://github.com/teivah/vertx-tutorial/tree/master/src/main/java/org/enterpriseintegration/vertx/tutorial/examples/example04)

让我们登上事件总线，看看如何接收和发送/发布消息。

让我们详细介绍一下关键步骤。

从 *vertx* 对象中检索事件总线实例。

我们创建一个*JSON object*-typed*message consumer*，并简单调用 *consumer()* 方法来定义通道的逻辑名称。

从 *MessageConsumer* 对象中，我们定义了一个显式的处理程序，每当我们在 *customer.create* 通道中接收到一条新消息时，就会执行这个处理程序。
将 *json* lambda 直接格式化为 [*JsonObject*](https://javarevisited.blogspot.com/2013/02/how-to-convert-json-string-to-java-object-jackson-example-tutorial.html) 。

现在让我们想象一下，在丰富了 *JsonObject* 之后，我们现在想要将它重新发布到另一个通道上。这可以通过以下方式简单实现:

在这个例子中，我们调用了 *publish()* 方法(一对多)，但是如果我们想要一个点对点的交互，我们应该调用 *send()* 方法(一对一)。

如果事件总线消费者是在一个标准的垂直领域中定义的，那么将应用相同的事件循环模型。所以同样的警告，小心不要阻塞你的实现。

# 部署垂直市场

[Github 示例](https://github.com/teivah/vertx-tutorial/tree/master/src/main/java/org/enterpriseintegration/vertx/tutorial/examples/example05)

正如我们所见，有三种不同类型的垂直市场。让我们先来看看如何部署一个**标准的** one:

*deploy article()*方法将引用实现类的字符串或 *Verticle* 对象作为第一个参数。
第二个参数是一个 *DeploymentOptions* 对象，用于在垂直部署期间设置选项。

在这里，我们可以通过调用 *setInstances()* 方法为一个顶点设置实例(或事件循环)的数量。
*setConfig()*调用展示了如何将信息从父实例传递到垂直实例。*配置*对象是一个 *JsonObject* 。

一件重要的事情是，如果我们在事件通道上定义一个具有两个垂直实例的事件总线消费者，发布的消息 *event01* 将被两个实例接收。这可能不是预期的行为，因此您必须将**的实例数量限制为一个，或者使用**另一个**垂直类型。**

以下示例显示了如何部署一个**worker**vertical:

唯一的变化(除了这里我们没有展示如何传递一个配置对象)是对 *setWorker()* 方法的调用。这个方法将一个*布尔值*作为参数，简单地表示一个顶点是否是一个工人。

通常，因为 worker 类型遵循更经典的模型一个事件=一个线程，所以我们需要设置比标准 verticles 更多的实例。

使用相同的部署配置(两个实例)，worker verticle 仍然会收到两次 *event01* 。然后管理*事件 02* 和*事件 03* 。所以与之前相同的警报，因为它可能会导致创建**副本**。

这个场景的解决方案是使用一个**多线程工作器**:

在那个例子中，我们用 true 调用了 *setMultiThreaded* 方法。
正如您在这里看到的，我们没有配置**实例的数量**。对于多线程的垂直工作者，不使用这个值。

一个多线程工作器只由**一个单实例**管理，但是可以由几个线程并发执行**。相比之下，一个默认的 worker 由**的一个或几个实例**管理，但是每个实例只由**的一个线程执行。****

在本例中， *event01* 将只被接收一次(当然 *event02* 和 *event03* 也是如此)。

# 还有什么？

在第一篇文章中，我们介绍了 Vert.x 的核心原则:**事件循环模型**、**垂直模型**和**事件总线**。

正如我们所见，Vert.x 是实现反应式应用的专用框架。同样值得补充的是，垂直模型非常适合**微服务**概念。

除了我们已经看到的，Vert.x 还提供了许多其他功能:Rx API，反应流和纤程的标准，一些用于生成代码或文档的工具，与云提供商如 OpenShift 的集成，一个 [Docker](https://hackernoon.com/10-free-courses-to-learn-docker-for-programmers-and-devops-engineers-7ff2781fd6e0) 集成，一些与服务发现工具如 Consul 等的桥梁…

Vert.x 绝对是**一个非常丰富的框架**，这篇文章只是**系列文章**的第一篇。

如果您对该解决方案感兴趣，您应该查看以下链接:

*   官方 Vert.x [文档](http://vertx.io/docs/)。
*   官方 [Vert.x v3 Github](https://github.com/vert-x3) 有非常大量的[不同语言的](https://github.com/vert-x3/vertx-examples)示例。
*   到目前为止使用 Vert.x 的公司列表。
*   官方 [Vert.x 路线图](https://github.com/vert-x3/wiki/wiki/Vert.x-Roadmap)。