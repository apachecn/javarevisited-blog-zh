# 生产率最高的 5 大 Java 库

> 原文：<https://medium.com/javarevisited/the-top-5-java-libraries-for-maximum-productivity-24f75b1f4de8?source=collection_archive---------1----------------------->

![](img/ae990b844e39730c9e3bda2e6983a188.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Alireza Attari](https://unsplash.com/@alireza_attari?utm_source=medium&utm_medium=referral) 拍摄

你有没有发现自己写了一段 Java，然后想，“一定有更好的方法？” [Java](/javarevisited/top-5-java-online-courses-for-beginners-best-of-lot-1e1e240a758) 这么受欢迎的语言，大概有吧！这里是我遇到的一些最有用的 Java 库的综合列表。使用这些库将提高您的生产力，并利用他人已经完成的工作！

# ①龙目岛

[Project Lombok](https://projectlombok.org/) 是一个 Java 库，它使用注释来减少[样板代码](https://en.wikipedia.org/wiki/Boilerplate_code)。可以使用 **@** Getter 等注释来自动生成“getField()”方法。以下是一些受支持的注释:

1.  @Getter 和@Setter，它们生成 Getter 和 Setter。
2.  @EqualsAndHashCode 自动生成符合 [Equals 和 HashCode 契约](https://www.baeldung.com/java-equals-hashcode-contracts)的 Equals 和 HashCode 方法。
3.  @ToString 将生成一个 ToString()方法，该方法遵循格式 *ClassName(fieldName=value，fieldName2=value…)。*
4.  @Builder 自动实现了 [builder 模式](https://howtodoinjava.com/design-patterns/creational/builder-pattern-in-java/)，以便于构建 POJO。
5.  @Data 是@Getter、@Setter、@EqualsAndHashCode、@ToString 和@RequiredArgsConstructor 的简写！

还有更多受支持的注释，它们都是高度可定制的。再也不写样板了！

# 2)番石榴

Guava 是一个由 Google 创建和维护的 Java 库，它包含许多广泛适用的实用程序，可以解决 Java 中的常见问题。一些功能包括:

1.  对集合的扩展，比如`Multimap<K, V>`，它是一个支持给定键的多个值的映射，相当于带有更干净 API 的映射< K，集合< V > >。
2.  Graphs 包，其中包括许多用于对图形类型数据建模的实用程序
3.  并发实用程序，如 MoreExecutors、Atomics 和 ListenableFuture

番石榴图书馆里有太多值得挖掘的东西。因为它是由 Google 维护并被广泛使用的，所以你可以相信他们的 API 已经过彻底的测试并得到了精心的维护。如果你有一个常见的 Java 问题需要解决，那么 Guava 大概有解决方案！

# 3)冬眠

[Hibernate](http://hibernate.org/orm/) 是一个对象关系映射库，它允许你与数据库进行交互，而不必考虑如何在 SQL 表和 POJOs 之间进行转换。来自 Hibernate 的网站:

> Hibernate 使您能够按照自然的面向对象的习惯用法开发持久类，包括继承、多态、关联、组合和 Java 集合框架。Hibernate 不需要持久类的接口或基类，并且支持任何类或数据结构的持久化。

使用 [Hibernate](/javarevisited/top-5-hibernate-online-training-courses-for-beginners-and-advance-java-programmers-469460596b2b) 为你的持久层增压，消除数千行数据库代码。

# 4)假装

OpenFeign 是网飞创建的一个库，可以让你轻松地用 Java 创建 RESTful HTTP 客户端。要创建一个虚拟客户机，只需声明一个概述请求和响应细节的接口。这可以通过一个例子得到最好的说明:

使用时，这个接口 GitHubClient 将执行在方法上声明的 GET 和 POST 请求。这个客户端将默认对所有请求使用 JSON 格式。对于 Feign 客户端，有无数的定制:

1.  编码器和解码器来选择如何在网络上序列化和反序列化 POJOs
2.  指定重试策略和逻辑的重试程序
3.  用于其他请求前任务(如 cookie 检索或授权)的请求拦截器

使用 Feign，再也不用手动编写 HTTP 客户端了！注意:如果你使用 [Spring](/javarevisited/10-best-online-courses-to-learn-spring-framework-in-2020-f7f73599c2fd) ，你应该使用 [Spring Cloud OpenFeign](https://docs.spring.io/spring-cloud-openfeign/docs/2.2.4.RELEASE/reference/html/) ，它拥有比 OpenFeign 本身更好的 Spring 集成。

# 5) Spring Boot

最后但同样重要的是 Spring Boot。Spring Boot 通过以下方式简化了生产就绪型 Java 应用程序的创建流程:

> 创建独立的 Spring 应用程序
> 
> 直接嵌入 Tomcat、Jetty 或 Undertow(无需部署 WAR 文件)
> 
> 提供自以为是的“初学者”依赖项，以简化您的构建配置
> 
> 尽可能自动配置 Spring 和第三方库
> 
> 提供生产就绪特性，如指标、健康检查和外部化配置

使用 [Spring Boot](/javarevisited/10-free-spring-boot-tutorials-and-courses-for-java-developers-53dfe084587e?source=collection_home---4------7-----------------------) 可能会有一个困难的学习曲线，但我可以向你保证，投入的时间会有巨大的回报。Spring Boot 全面减少了我的项目的开发时间，并继续通过其稳定性、可扩展性和可读性获得回报。

# 结论

[Java](/javarevisited/top-10-frameworks-full-stack-java-developers-can-learn-in-2020-5995021401e5) 可能是一种较老的语言，缺乏较新语言的一些特性，但是它拥有来自社区的无与伦比的库支持。您可以在项目中利用现成的生产库。掌握了这些库的知识，您可以成为一名更有生产力的开发人员。不要重新发明轮子——专注于你的核心能力，解决问题。

如果你喜欢这篇文章，请考虑鼓掌并在下面留下你的邮箱来订阅我的邮箱列表。