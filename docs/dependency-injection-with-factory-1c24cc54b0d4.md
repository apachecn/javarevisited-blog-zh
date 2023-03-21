# 工厂依赖注入

> 原文：<https://medium.com/javarevisited/dependency-injection-with-factory-1c24cc54b0d4?source=collection_archive---------1----------------------->

另一个可供考虑的选择

![](img/32cef23104458d545ea11ceb3d4d2569.png)

照片由 [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**简介**

在 Java 中，我们有不同的初始化对象的方法。它可以是普通初始化、依赖注入或工厂模式。

在 [Spring 框架](/javarevisited/10-best-online-courses-to-learn-spring-framework-in-2020-f7f73599c2fd)中，依赖注入是基本的方面，通常用于允许组件的松散耦合，而不需要知道每个组件的依赖关系。

同样地，[工厂模式](https://javarevisited.blogspot.com/2011/12/factory-design-pattern-java-example.html)也促进了组件的松散耦合，因为它将了解的需求委托给了工厂。

**依赖注入 vs 工厂模式**

依赖注入和工厂模式的区别在于实例的生命周期管理。

对于依赖注入，生命周期管理由框架在代码之外处理。

对于工厂模式，生命周期管理由应用程序中的程序员处理。

**什么是工厂依赖注入？**

基于实现仍然是[依赖注入](https://javarevisited.blogspot.com/2012/12/inversion-of-control-dependency-injection-design-pattern-spring-example-tutorial.html)和工厂模式的对象创建决策逻辑。

让我们看一个例子。假设我们有两个类，InstanceDelivery 和 ScheduledDelivery，它们在创建订单时执行不同的逻辑。

**只是依赖注入**

有了刚刚的[依赖注入](https://www.java67.com/2012/08/spring-interview-questions-answers.html)，它的好处是显而易见的。

*   首先，使用构造函数注入创建对象很简单。
*   其次，对于具有多个实例的类，对象的创建更加透明。例如，ScheduledDelivery 类依赖于 Scheduler 类。

然而，构造函数注入也有缺点。

*   首先，它会导致一种叫做长参数表的代码味道。在这个例子中，只有两种交付模式(即时和预定)，所以看起来可以接受。但是如果有 10 种交付方式呢？
*   第二，无论何时引入了额外的交付模式，构造函数签名都容易发生变化。看起来构造函数签名的改变不会影响任何依赖注入的代码。然而，如果你在单元测试中注入一个模仿对象，它们将影响 DeliveryService 类的单元测试。

**只是工厂模式**

对于工厂模式，对象的创建不像依赖注入那样简单。

*   首先，您需要知道每个类的依赖关系，在其中创建一个对象可能会很乏味。
*   其次，DeliveryService 类的单元测试更难构建，因为 DeliveryFactory 不可模仿。

然而，工厂模式的好处是减少了依赖构造函数注入导致的长参数列表。

**依赖注入与工厂**

通过工厂的依赖注入，它结合了两者的优点，同时消除了缺点。

*   首先，对象的创建是透明的。
*   第二，长参数表从很多减少到 1 个。
*   第三，不太容易改变构造函数签名。
*   第四，[单元测试](/javarevisited/top-10-courses-to-learn-eclipse-junit-and-mockito-for-java-developers-4de1e8d62b96)更容易，因为它是可模仿的。

基础实现仍然是依赖注入，其中 DeliveryService 和 DeliveryFactory 使用构造函数注入。DeliveryFactory 中的 getInstance 实现了对象创建决策逻辑来返回所需的对象。

**结论**

仅仅依赖注入或者仅仅工厂模式实现都没有错。工厂的依赖注入增加了另一种选择，使你的代码更加整洁和有组织。当您有两个以上的服务类时，这种技术更合适，并且随着它的发展，这种技术的好处会更大。最终，使用任何适合用例的设计模式都是很重要的。

短读到此为止。谢谢你的时间