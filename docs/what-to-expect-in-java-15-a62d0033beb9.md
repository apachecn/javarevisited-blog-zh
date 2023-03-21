# Java 15 有什么值得期待的

> 原文：<https://medium.com/javarevisited/what-to-expect-in-java-15-a62d0033beb9?source=collection_archive---------1----------------------->

## 简要介绍即将发布的 Java 版本的新特性

![](img/a9cfcf1fc39e1a71e9c4a784a161d36b.png)

照片由[数学](https://unsplash.com/@builtbymath?utm_source=medium&utm_medium=referral)上的 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

3 月 17 日发布的 Java 14 有一些重要的改进。按照为 Java 设定的 6 个月发布周期，Java 14 将在 8 月被 Java 15 取代。Java 15 也将像 Java 14 一样成为 Java 的一个特性版本，而不是一个长期支持(LTS)版本。当前的 LTS 版本是 Java 11，有望在 2021 年被 Java 17 LTS 版本取代。

截至 7 月 16 日，Java 15 正处于第二阶段。这意味着整个特性集被冻结了，在这个版本中不会再有新的 jep 了。因此，根据 [OpenJDK 网站](https://openjdk.java.net/projects/jdk/15/)的消息，以下是 Java 15 将推出的预期特性。

*   [爱德华兹曲线数字签名算法(EdDSA)](https://openjdk.java.net/jeps/339)
*   [密封类(预览)](https://openjdk.java.net/jeps/360)
*   [隐藏类](https://openjdk.java.net/jeps/371)
*   [重新实现传统 DatagramSocket API](https://openjdk.java.net/jeps/373)
*   [文本块](https://openjdk.java.net/jeps/378)
*   [禁用和取消偏向锁定](https://openjdk.java.net/jeps/374)
*   [移除 Solaris 和 SPARC 端口](https://openjdk.java.net/jeps/381)
*   [移除 Nashorn JavaScript 引擎](https://openjdk.java.net/jeps/372)
*   [不赞成为移除而激活 RMI](https://openjdk.java.net/jeps/385)
*   一个低暂停时间的垃圾收集器
*   ZGC:一个可扩展的低延迟垃圾收集器
*   [实例的模式匹配(第二次预览)](https://openjdk.java.net/jeps/375)
*   [记录(第二次预览)](https://openjdk.java.net/jeps/384)
*   [外部存储器访问 API(第二孵化器)](https://openjdk.java.net/jeps/383)

下面我将讨论其中的一些重要特性。

## JEP 339:爱德华曲线数字签名算法(EdDSA)

EdDSA 是一个数字签名系统，它是 Schnorr 的带有 Edwards 曲线的签名系统的变体。你可以在这里了解更多关于[的信息。Java 15 提供了这种签名方案，它优于 JDK 中现有的签名方案。](https://tools.ietf.org/html/rfc8032)

Java 增强建议(JEP)主要旨在实现 RFC 8032 中标准化的该方案，并开发与平台无关的 EdDSA 实现，其性能优于现有的 ECDSA 实现，并确保定时独立于所使用的秘密。

以下是摘自 [JEP](https://openjdk.java.net/jeps/339) 的 API 用法示例。

```
// example: generate a key pair and sign
KeyPairGenerator kpg = KeyPairGenerator.getInstance("Ed25519");
KeyPair kp = kpg.generateKeyPair();
// algorithm is pure Ed25519
Signature sig = Signature.getInstance("Ed25519");
sig.initSign(kp.getPrivate());
sig.update(msg);
byte[] s = sig.sign();

// example: use KeyFactory to contruct a public key
KeyFactory kf = KeyFactory.getInstance("EdDSA");
boolean xOdd = ...
BigInteger y = ...
NamedParameterSpec paramSpec = new NamedParameterSpec("Ed25519");
EdECPublicKeySpec pubSpec = new EdECPublicKeySpec(paramSpec, new EdPoint(xOdd, y));
PublicKey pubKey = kf.generatePublic(pubSpec);
```

## JEP 360:密封类(预览)

这只是 Java 15 中的一个预览特性。这里，我们可以通过对声明应用*密封的*修饰符来密封一个类/接口。这样，我们可以限制哪些其他类或接口可以扩展或实现它们。这样，我们将能够提供一种比访问修饰符更具声明性的方式来限制超类的使用。

在以前的版本中，Java 在这方面提供的工具非常有限。开发人员必须要么使一个类成为 final，这样它就不能有任何子类，要么使一个类或它的构造函数成为 package-private，这样它只能在同一个包中有子类。这些变通办法有其缺点。但是有了这个新特性，实现类限制就非常容易了。

```
public sealed class Shape
    permits Circle, Rectangle, Square {...}
```

## JEP 371:隐藏的班级

Java 15 引入了不能被其他类的字节码直接使用的类。这些类旨在供在运行时生成类并通过反射间接使用它们的框架使用。

该 JEP 的主要目标包括允许框架将类定义为框架的不可发现的实现细节，以便它们不能被其他类链接，也不能通过反射被发现，支持主动卸载不可发现的类，并反对非标准 API，*misc . Unsafe::define anonymous class。*

## JEP 373:重新实现传统的 DatagramSocket API

*java.net.DatagramSocket* 和*Java . net . multicast socket*API 的代码库都非常陈旧脆弱。这些 API 可以追溯到 JDK 1.0，它们是遗留 Java 和 C 代码的混合，现在很难维护。并且多播套接字也是在 IPv6 开发时实现的。因此，它不太支持 IPv6。此外，他们有一些并发问题，由于这些原因，这些实现将被更简单、更现代、更易于维护和调试的实现所取代。

## JEP 378:文本块

文本块是一种多行字符串文字，可以避免使用转义序列。从 Java 12 延续到 Java 13 和 14，有很多关于多行字符串的实验。现在它正在成为 Java 15 中的一个永久特性。这样，表达跨越几行的字符串就非常容易使用，并且可读性也得到提高。现在，程序员可以轻松地将 HTML、XML、SQL 或 JSON 的片段嵌入到字符串文字中，这在以前需要使用连接和转义序列进行大量编辑。

文本块可以用三个”字符创建，也可以用同样的三个”字符结束。以下是摘自 [OpenJDK 网站的一个例子。](https://openjdk.java.net/jeps/378)

```
String query = """
               SELECT "EMP_ID", "LAST_NAME" FROM "EMPLOYEE_TB"
               WHERE "CITY" = 'INDIANAPOLIS'
               ORDER BY "EMP_ID", "LAST_NAME";
               """;
```

## JEP 374:禁用和反对有偏向的锁定

偏向锁定是 HotSpot 虚拟机中使用的一种优化技术，用于减少无竞争锁定的开销。但是随着现代先进计算机的发展，偏向锁的好处变得不那么重要了，因此 Java 15 中禁用了偏向锁。

但是由于一些 Java 应用程序可能会在禁用偏置锁定的情况下出现性能下降，程序员可以通过在命令行中设置 *XX:+UseBiasedLocking* 来重新启用这种情况。

## JEP 377 和 JEP 379:垃圾收集者

ZGC 是一个可伸缩的低延迟垃圾收集器，从 Java 11 开始就是一个实验性的特性，在 Java 15 中它变成了一个产品特性。Shenandoah GC 也是自 Java 12 以来的一个实验性特性，它将成为 Java 15 中的一个产品特性。

## JEP 375:实例的模式匹配(第二次预览)

这个特性的第一个预览版出现在 Java 14 中。这提供了一种简单有效的检查实例的方法。

```
if (obj instanceof String s) {
    // can use s here
} else {
    // can't use s here
}
```

你可以通过[这里](https://cr.openjdk.java.net/~briangoetz/amber/pattern-match.html)阅读更多关于模式匹配的内容。

## JEP 384:记录(第二次预览)

这也是 Java 14 中首次推出的预览版。记录是充当不可变数据的透明载体的类。

## JEP 383:外部内存访问 API(第二个孵化器)

这个 API 已经作为 Java 14 中的孵化模块交付了。孵化特性是实验性的 API，以独立模块的形式发布，名称以“jdk.incubator”为前缀。

这个 API 允许 Java 程序安全有效地访问 Java 堆外的内存，重点是提供通用性、安全性、确定性和可用性。

根据我的观点，上面描述的是一些重要的特征。希望我们能很快看到一个具有上述特性的改进的、高效的、无 bug 的版本。此外，如果你喜欢查看 Java 15 的早期开源版本，你可以下载并亲自测试。

我的文章已经结束了，我希望你能从这次阅读中有所收获。感谢您的阅读。干杯。

## 资源

*   [*OpenJDK 网站*](https://openjdk.java.net/projects/jdk/15/)
*   [*InfoWorld 科技博客*](https://www.infoworld.com/article/3534133/jdk-15-the-new-features-in-java-15.html#:~:text=The%20next%20version%20of%20standard,of%20pattern%20matching%20and%20records&text=Java%20Development%20Kit%2015%2C%20Oracle's,the%20feature%20set%20now%20frozen)
*   [*Metebalci 技术博客*](https://metebalci.com/blog/what-is-new-in-java-15/)