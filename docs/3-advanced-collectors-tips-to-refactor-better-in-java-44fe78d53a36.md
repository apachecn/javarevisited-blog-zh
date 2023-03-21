# 在 Java 中更好地重构的 3 个高级收集器技巧

> 原文：<https://medium.com/javarevisited/3-advanced-collectors-tips-to-refactor-better-in-java-44fe78d53a36?source=collection_archive---------3----------------------->

## 如何使用收集器来改进重构

![](img/15ed04e054e1082b72e4a16cf4f71224.png)

照片由[米哈伊尔·尼洛夫](https://www.pexels.com/@mikhail-nilov?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/offer-businessman-man-person-6613710/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

我们经常使用`[Collectors](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)`。我们压缩巨大的代码块。我们压缩逻辑。我们用它们来提高代码质量。

我们对`Collectors`了解不多。应用它们，测试通过，我们继续前进。这就是为什么我们现在应该探讨这个话题。