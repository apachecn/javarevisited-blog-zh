# Java 中的微优化。String.replaceAll

> 原文：<https://medium.com/javarevisited/micro-optimizations-in-java-string-replaceall-c6d0edf2ef6?source=collection_archive---------1----------------------->

![](img/fc9a5ed21e65149e5c0314317994b223.png)

String.replaceAll

在这篇文章中，我们将讨论另一种流行的代码构造的用法，即 *String.replaceAll* 和 *String.replace* 方法，我们将研究它如何影响 Java 11 中代码的性能，以及对此你能做些什么。

**(请从性能角度考虑以下所有代码)**