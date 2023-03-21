# Java 虚拟机(JVM)内部机制，第 1 部分—类加载器

> 原文：<https://medium.com/javarevisited/java-virtual-machine-internals-class-loader-eea706eb37d9?source=collection_archive---------0----------------------->

在这一系列文章中，我将讨论 Java 虚拟机是如何工作的。在这篇文章中，我将讨论 JVM 中的类加载机制。

Java 虚拟机是 Java 技术生态系统的核心。是 JVM 让 Java 程序成为“写一次就能到处运行”的东西。像其他虚拟机一样，JVM 也是一台抽象计算机。Java 虚拟机的主要工作是加载[类文件](https://en.wikipedia.org/wiki/Java_class_file)并执行它们包含的[字节码](https://en.wikipedia.org/wiki/Java_bytecode)。