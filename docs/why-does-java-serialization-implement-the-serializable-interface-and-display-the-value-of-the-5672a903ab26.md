# 为什么 Java 序列化实现 Serializable 接口并显示指定 SerialVersionUID 的值

> 原文：<https://medium.com/javarevisited/why-does-java-serialization-implement-the-serializable-interface-and-display-the-value-of-the-5672a903ab26?source=collection_archive---------1----------------------->

![](img/cee4dd875f4e18a6ffad90959eae319c.png)

一只[胖柯基](https://unsplash.com/@fattycorgi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

最近公司在做服务，模型包里所有的类都需要实现`[Serializable](https://www.java67.com/2021/04/java-serialization-tutorial-example-.html)` [](https://www.java67.com/2021/04/java-serialization-tutorial-example-.html)接口，还要显示指定的 [serialVersionUID](https://javarevisited.blogspot.com/2014/05/why-use-serialversionuid-inside-serializable-class-in-java.html) 的值。