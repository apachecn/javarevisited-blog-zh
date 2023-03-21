# Java 中的微优化。String.equalsIgnoreCase()

> 原文：<https://medium.com/javarevisited/micro-optimizations-in-java-string-equalsignorecase-ea25dfb03f95?source=collection_archive---------0----------------------->

![](img/3305a6534b3e6c6bf49e8527d714e122.png)

因此，在[之前的文章](/javarevisited/micro-optimizations-in-java-string-equals-22be19fd8416?source=friends_link&sk=61649c9c9fccfb59c0515fcb9f7447ef)中，我们考虑了两种方法来提高众所周知的 *String.equals()* 方法的性能。现在，我们来看看你每天都在使用的另外两个 String 类方法。

**(请从性能角度考虑以下所有代码)**