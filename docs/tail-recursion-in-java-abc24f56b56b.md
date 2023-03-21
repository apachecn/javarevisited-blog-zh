# Java 中的尾部递归

> 原文：<https://medium.com/javarevisited/tail-recursion-in-java-abc24f56b56b?source=collection_archive---------2----------------------->

…或者如何从比构建器示例更酷的注释处理中获益。

![](img/dc8a47630ea41df71436f398ddbe65e4.png)

Ourobos:编写干净的代码有助于维护它；尾部递归服务于这个目标。

# 前言

尾递归是一种编译级优化，旨在避免调用递归方法时堆栈溢出。例如，以下斐波纳契数的实现是递归的，而不是尾递归的。