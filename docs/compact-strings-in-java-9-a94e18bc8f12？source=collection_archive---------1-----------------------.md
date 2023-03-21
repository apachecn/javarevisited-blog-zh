# Java 9 中的紧凑字符串

> 原文：<https://medium.com/javarevisited/compact-strings-in-java-9-a94e18bc8f12?source=collection_archive---------1----------------------->

## 字符串的性能改进

![](img/902f1e1f8a4be62492db209836ee8867.png)

纳撒尼尔·舒曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在 Java 9 之前，字符串在内部由包含字符串字符的 char 数组表示。由于 Java 内部使用 **UTF-16** ，每个字符占用两个字节，同样，如果单个字符可以用单个字节表示( **LATIN-1** 表示)，那么就有可能提高性能和内存消耗。