# 对庞大的代码说不

> 原文：<https://medium.com/javarevisited/say-no-to-bulky-codes-83a792474597?source=collection_archive---------0----------------------->

## Java 库和框架的迷人之旅

![](img/dd128030fcc10a3535438b2015ff1420.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Caleb Russell](https://unsplash.com/@calebrussell?utm_source=medium&utm_medium=referral) 拍摄的照片

要成为一名优秀的 Java 开发人员，仅仅知道 Java 语法是不够的。要让[成为一名更好的 Java 开发人员](/javarevisited/9-tips-to-become-a-better-java-programmer-cad4c9334cc1)，还有很多工作要做。其中之一就是对 Java 库和框架有很好的了解，并学习如何以及何时使用它们。

Java 库是包含属于某一类别的类或接口的包。这些类和接口包含可以在程序中用来解决编程相关问题的方法。

而**框架**可以是我们在开发应用程序中使用的任何东西——一个库，一个库集合，或者一个脚本集合，等等。

有大量的[库](https://javarevisited.blogspot.com/2018/01/top-20-libraries-and-apis-for-java-programmers.html)和[框架](/javarevisited/top-10-frameworks-full-stack-java-developers-can-learn-in-2020-5995021401e5)可用，所以熟悉至少几个最常用的非常重要。所以我将与你分享一些我认为 Java 开发人员应该知道的库类别，不管你是专家还是新手。

## 1.日志库

使用 print 语句将消息打印到控制台以测试和调试 Java 程序是一种常见的做法。但是这种[调试](https://www.java67.com/2018/01/how-to-remote-debug-java-application-in-Eclipse.html)的方法并不那么有效，因为扫描数百行可能会令人疲惫不堪。此外，根据经验，您都知道您添加的这些调试行会使您的代码混乱不堪。

在这种情况下，日志库就派上了用场。这些库是至关重要的，尤其是在服务器端应用程序中，因为你看不到服务器内部发生了什么，日志文件必须用于这个目的。

[**Java . util . logging**](https://docs.oracle.com/javase/8/docs/api/java/util/logging/package-summary.html)包与**【JDK】**一起提供，但还有更好的日志框架可用，如 [**Flogger**](https://google.github.io/flogger/) **，**[**SLF4j**](http://www.slf4j.org/)**，** [**LogBack**](http://logback.qos.ch/) ，还有[**Log4j(Log4j)**](https://logging.apache.org/log4j/2.x/)。

你可以在这里了解更多这些[。](https://stackify.com/logging-java/)

## 2.单元测试库

一个 Java 开发人员应该确保他开发的软件的质量，并且应该尽快发现错误。对于那些人来说，[单元测试](/javarevisited/5-courses-to-learn-junit-and-mockito-in-2019-best-of-lot-f217d8b93688?source=---------20------------------)是一个好方法。

> **单元测试**意味着将你的代码分解成最小的可测试代码片段，并对每一个片段进行一系列测试，以验证代码的目的是否得到满足。

简单地说，它意味着——编写一段代码(单元测试)来验证为实现需求而编写的代码(单元)。但是大多数开发人员跳过这一步，因为他们认为这是额外的工作，或者是由于时间紧迫。

但是编写好的单元测试是很重要的，因为在测试过程中发现并修复错误比在软件开发的后期阶段发现错误更好，这样可以节省金钱和时间。

为单元测试编写自己的代码需要做大量的工作，但这并不是必需的。[这个堆栈溢出回答](https://stackoverflow.com/questions/4325014/how-to-do-unit-testing-without-the-use-of-a-library)会给你看一个手动编写单元测试的例子。

像 [**JUnit**](https://junit.org/junit5/) **，** [**AssertJ**](https://joel-costigliola.github.io/assertj/) **，**[**TestNG**](https://testng.org/doc/)**，**[**mock ITO**](https://site.mockito.org/)**，**[**wire mock**](http://wiremock.org/docs/getting-started/)**，**和 [**PowerMock**](https://powermock.github.io/)

所以要熟悉使用[单元测试库](/javarevisited/top-10-courses-to-learn-eclipse-junit-and-mockito-for-java-developers-4de1e8d62b96?source=collection_home---4------1-----------------------)。它将帮助你尽早发现错误，提高代码质量，节省金钱和时间，以及更多的好处。

> 对于单元测试调试、信息或警告日志消息，上述框架不起作用。在这些情况下， [**LogCaptor 库**](https://dzone.com/articles/unit-testing-log-messages-made-easy) 会帮到你。

## 3.数据库连接池库

**连接池**是软件应用程序用来连接到[数据库](/hackernoon/top-5-sql-and-database-courses-to-learn-online-48424533ac61)的数据访问模式。

这是通过一组现有的、可重用的连接对象来完成的。一旦需要新的连接，就检索池中的连接对象，然后在使用的线程完成后，将它放回池中。

如果不是这种数据模式，数据库连接是非常昂贵的。我们可以使用像 [**公地池**](https://commons.apache.org/proper/commons-pool/) 和 [**DBCP**](https://commons.apache.org/proper/commons-dbcp/) 这样的库，因为在运行时创建数据库连接需要时间，会使请求处理速度变慢。

## 4.HTTP 库

有一个内置的 Java 类[***HttpUrlConnection***](https://docs.oracle.com/javase/8/docs/api/java/net/HttpURLConnection.html)*允许us 在没有任何第三方库支持的情况下执行基本的 HTTP 请求。所有需要的类都包含在***java.net****包中。**

**但是在***HttpUrlConnection***的帮助下编写的代码可能会很庞大，并且它不提供高级功能、完全的灵活性和所需的用户友好性。**

**因此，我们必须使用第三方库来填补这一空白。我们可以使用的一些库有[**Apache http components Client**](https://hc.apache.org/)， [**Jetty**](https://www.eclipse.org/jetty/documentation/current/http-client-api.html) 和[**Spring 5 WebClient**](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/reactive/function/client/WebClient.html)**。****

> **但是注意，从 **JDK 11** 开始，Java 提供了一个新的 API，[http client API](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpClient.html)**，**作为 **HttpUrlConnection** 的替代。这支持现代 web 特性，而不需要添加第三方库。**

## **5.消息库**

**一条**消息，**如你所知是一条信息——文本、XML 文档或 JSON 数据等。那么**消息传递**意味着软件应用程序或软件组件之间的信息交换。**

**[**【JMS(Java 消息服务)**](https://www.oracle.com/java/technologies/java-message-service.html)**由 Java 提供，用于创建、发送、接收和读取不同系统之间的消息。你也可以使用 [**Tibco RV**](https://docs.tibco.com/pub/ems/8.5.1/doc/html/GUID-13A55DB1-55CA-4372-BA44-3EE2C95E30CB.html) 消息协议来完成这项工作。****

## ****6.JSON 解析库****

****基于网络的应用程序的使用正在迅速增长。这导致了 **JSON** 的使用越来越多。****

******JSON** 用作服务器和客户端之间的通信协议。以前这项任务是用 XML 完成的，现在几乎完全被 JSON 取代了。这是因为 [JSON](https://javarevisited.blogspot.com/2017/02/how-to-consume-json-from-restful-web-services-Spring-RESTTemplate-Example.html) 需要的编码更少，尺寸更小，处理和传输数据更快。****

****在 **J** [**SON 解析**](https://javarevisited.blogspot.com/2018/02/how-to-parse-json-with-date-field-in-java-jackson-example.html#axzz5Xl8EXHhY) **，**接收到的 JSON 数据被读取，然后被解释为 list 对象，以用户友好的方式呈现给最终用户。****

****虽然 JDK 没有内置的 JSON 库，但是你可以使用第三方库，比如<https://github.com/FasterXML/jackson>****，**[**Gson**](https://github.com/google/gson)**，**[**JSON . simple**](https://code.google.com/archive/p/json-simple/)**，**[**JSON parser**](https://javaee.github.io/javaee-spec/javadocs/javax/json/stream/JsonParser.html)**，**[**Genson**最适合你的项目必须由你来决定。****](http://genson.io/)******

****[这里的](https://blog.overops.com/the-ultimate-json-library-json-simple-vs-gson-vs-jackson-vs-json/)可以帮助你选择最适合你的项目的库。****

## ****7.通用 Java 库****

****有一些真正令人兴奋的第三方库可用，您可以使用它们来使编码变得更加简单和干净。以下是其中的一些:****

*   ****[**阿帕奇公地**](https://commons.apache.org/)****

****这是一个关于创建 Java 库的项目- **Commons Math，Commons CLI，Commons CSV，**和 **Commons IO** 是一些常用的 Apache Commons 库。****

*   ****[**谷歌番石榴**](https://github.com/google/guava/wiki)****

****为项目中遇到的许多问题提供解决方案，如集合、数学、字符串、输入和输出。****

*   ****[**龙目岛**](https://projectlombok.org/)****

******Lombok** 是一个非常有用的库，尤其是这有助于避免重复代码。举个例子，你知道一个程序的***getter***和***setter***占用了相当多的代码行，弄得乱糟糟的。但是使用 Lombok，你可以有一个更干净的代码，因为它生成重复的代码，但不使它在代码中可见。****

****正如我在开始时提到的，有大量的库和框架可用。深入详细地解释它们是不可能的，而且会使这篇文章太冗长。****

****这篇文章只是为了启发你 Java [库](/javarevisited/top-10-tools-for-automation-testing-in-java-b615c2d57f54)和[框架](/javarevisited/10-best-online-courses-to-learn-spring-framework-in-2020-f7f73599c2fd)如何帮助你摆脱样板代码。因此，除了我没有涉及到的内容之外，还有更多关于上述图书馆类别的内容需要你自己去调查。更多地探索它们的优缺点，这将让你知道什么最适合你的计划。****

****我希望这有所帮助。希望不久能看到你更多的文章。感谢您的阅读。****

## ****资源****

*   ****[*Java 程序员应该知道的 20 个有用的库来自 DZone*](https://dzone.com/articles/20-useful-open-source-libraries-for-java-programme)****
*   ****[*弗洛格流畅的从巴尔东伐木*](https://www.baeldung.com/flogger-logging)****
*   ****[*从 Baeldung*](https://www.baeldung.com/java-http-request) 用 Java 做一个简单的 HTTP 请求****
*   ****[*从 Baeldung*](https://www.baeldung.com/java-9-http-client) 探索 Java 中的新 HttpClient****
*   ****[Java 开发人员的 10 个单元测试和集成库](https://javarevisited.blogspot.sg/2018/01/10-unit-testing-and-integration-tools-for-java-programmers.html)****