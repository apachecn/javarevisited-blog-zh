# 如何使用原始类型的 Java 流

> 原文：<https://medium.com/javarevisited/how-to-use-java-streams-with-primitive-type-e339e5adb306?source=collection_archive---------1----------------------->

## Java 流中 IntStream 原语类型的详细解释

> 最初发表于[](https://asyncq.com/how-to-use-java-streams-with-primitive-type)

## **介绍**

*   **Java 支持诸如[字节](https://www.java67.com/2015/06/how-to-convert-bytebuffer-to-string-in-java-example.html)、[短](https://www.java67.com/2018/05/3-ways-to-convert-string-to-short-in-Java.html)、 [int](https://www.java67.com/2016/01/3-ways-to-convert-int-value-to-string-in-java.html) 、 [long](https://www.java67.com/2015/07/how-to-convert-long-value-to-string-in-java-example.html) 、 [double](https://javarevisited.blogspot.com/2011/10/convert-double-to-string-example.html) 、 [char](https://javarevisited.blogspot.com/2012/02/how-to-convert-char-to-string-in-java.html) 、 [boolean](https://www.java67.com/2018/03/java-convert-string-to-boolean.html) 等原语数据类型。因此，这是 Java 不是纯粹面向对象语言的原因之一。**
*   **自从 java 8 发布以来，开发人员对 [Streams API](/javarevisited/8-best-lambdas-stream-and-functional-programming-courses-for-java-developers-3d1836a97a1d) 和利用其方法编写更多声明性代码感到兴奋…**