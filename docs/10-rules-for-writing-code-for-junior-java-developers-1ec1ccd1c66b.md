# 初级 Java 开发人员编写代码的 10 条规则

> 原文：<https://medium.com/javarevisited/10-rules-for-writing-code-for-junior-java-developers-1ec1ccd1c66b?source=collection_archive---------2----------------------->

**规则 1:功能不能超出你的屏幕**

这当然来自 Linux 编码指南。但是我担心我们的大多数 Java 出身的初级开发人员并没有真正接触过这些标准。如今，随着高分辨率显示器和多显示器的出现，编写具有复杂行为的函数变得非常容易。这使得新加入团队的人很难阅读和理解代码。

**规则 2:验证函数的输入参数**

这又是一个令人担忧的特性，可以在一些代码中看到，开发人员只是依赖于抛出的 [NullPointerException](https://www.java67.com/2021/05/how-to-solve-nullpointerexception-in-java.html) 而不是验证输入参数。好像在推广不好的代码(太苛刻了？)Jdk 14 以后，你还有`XX:+ShowCodeDetailsInExceptionMessages`选项，它会告诉你到底是什么变量导致了 NPE。这应该只在开发环境中使用，因为它存在安全风险和性能开销。

规则 3:注意资源会发生什么

java 程序员有时很容易掉入陷阱，认为因为 Java 有自动垃圾收集功能，所以这与他们无关。这导致了许多内存泄漏，尤其是在占用资源的时候，比如套接字、文件、数据库连接等等。永远要知道 JVM 负责什么，哪些资源需要自己管理

**规则 4:时间复杂性很重要**

尽管许多程序员学习如何计算时间复杂度，作为他们学位的一部分，但他们忘记了当他们开始编码时，类似的东西甚至已经存在。Java 程序员通常依靠 JDK 例程来为排序之类的事情提供最佳算法。

通常花费更多的时间来分析和试图找出为什么一个程序有不必要的性能问题。如果您了解每个 JDK 函数的时间复杂性，并且知道这些函数会对您的代码产生什么影响，那么在编写代码时就可以很容易地纠正这些问题。

**规则 5:测试驱动开发(TDD)**

尽管这像口头禅一样被宣扬，但似乎还没有成为所有新程序员的信仰。通常借口是项目截止日期意味着减少编写单元测试。强烈抵制这样做的诱惑，因为这是一种邪恶，它会吞噬你，让你永远在程序员的地狱里。

不实践 [TDD](/javarevisited/5-best-junit-and-test-driven-development-books-for-java-developers-2d3fecb5c9ac) 的项目需要多次 QA 工程师来发现代码中的问题。不进行 TDD 的另一个借口是，初级开发人员被指派对没有任何测试用例的遗留代码进行增强。这通常是因为我们低估了[嘲讽框架](/javarevisited/top-10-courses-to-learn-eclipse-junit-and-mockito-for-java-developers-4de1e8d62b96)的力量。

**规则 6:记忆很重要**

记忆曾经是一种稀缺资源。另一代人可能还记得您必须将代码放入 32KB RAM 的时代。如今，我们经常看到当人们的电脑有 32GB 内存时，他们看起来很不满。

请注意，耗费大量内存的微软操作系统并没有帮助，事实上，即使用 Java 编写一个简单的“Hello World”也是令人痛苦的内存密集型，直到[模块化 JDK](https://openjdk.java.net/jeps/200) 项目。

然而，在遗留代码中采用 Java 平台中的这些变化仍然滞后。尽量减少应用程序的内存占用，即使你没有被强迫这样做。

**规则 7:内存分配**

何时创建对象以及创建多少对象对应用程序的性能也有很大影响。经常有人可能不必要地克隆类。在 Java 中，一切都是通过引用传递的，这是有原因的。然而，如果你盲目地编写[“克隆”方法](https://javarevisited.blogspot.com/2013/09/how-clone-method-works-in-java.html#axzz5Y4Ks1BbR)并复制大量的内存，你就失去了这个优势。

您甚至可能没有注意到对象分配中的问题，因为您有足够大的年轻一代(在旧的 CMS 术语中),并且[垃圾收集器](/javarevisited/7-best-courses-to-learn-jvm-garbage-collection-and-performance-tuning-for-experienced-java-331705180686?source=---------8------------------)正在有效地清理那些短命的对象。

但是，您需要记住的是，对象分配是一项成本高昂的操作，当涉及到企业级性能时，您最终会遇到瓶颈，这可能会表现为性能问题。

**规则 8:算术运算**

注意计算中使用的数据类型。使用 float 和 double 来表示货币非常容易，但是您真的需要那么多小数位来表示货币吗？

请记住，精度损失仍然是现代计算机中的一个问题。为了方便起见，阅读 Java 提供的关于 [BigDecimal](https://javarevisited.blogspot.com/2012/02/java-mistake-1-using-float-and-double.html) 的文档。BigDecimal 强制您使用舍入模式，这样您就不会得到不明确的结果。

**规则九:模块化设计**

即使您的应用程序不是使用微服务架构设计的，考虑如何使用 Java 中可用的功能将您的应用程序分解成更小的模块也是一个好主意。

然而，如果你在任何地方都使用所有的 jdk 库，那么这就不适合你了。例如，如果你的 [Rest API](/javarevisited/10-best-java-web-services-rest-soap-and-api-courses-for-beginners-724a8f51298d) 组件只是盲目地导入 spring 中的所有东西，引用加密、文件系统、数据库和每个 jdk 模块，那么你将无法解绑 JDK。

尝试使用来自微服务原则的关注点分离原则。您可能希望将一些功能卸载给生态系统中的其他服务，并通过 API 进行通信。

**规则 10:线程和中断**

我采访的超过 70%的开发人员不知道 Java 中线程是如何工作的。他们也不知道为什么除了 [ReantrantLock](https://javarevisited.blogspot.com/2013/03/reentrantlock-example-in-java-synchronized-difference-vs-lock.html) 还有其他类型的锁？

同步块和锁是有区别的。除非您在 jdk 6 之前编程，否则您可能不必从使用 [wait() - > notify()](https://www.java67.com/2019/05/when-and-how-to-use-wait-and-notify-in-Java.html) 切换到 [Condition - > await()](https://javarevisited.blogspot.com/2015/06/java-lock-and-condition-example-producer-consumer.html) 。

如果您正在处理同时运行多个线程的服务器端代码，请仔细阅读 java.util.concurrent 包。

您可能也喜欢其他 Java 文章

</javarevisited/the-java-programmer-roadmap-f9db163ef2c2>  </javarevisited/10-books-java-developers-should-read-in-2020-e6222f25cc72> 