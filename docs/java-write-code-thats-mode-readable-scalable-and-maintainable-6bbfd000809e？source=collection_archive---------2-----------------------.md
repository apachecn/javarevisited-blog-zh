# Java:编写可读性、可伸缩性和可维护性更强的代码

> 原文：<https://medium.com/javarevisited/java-write-code-thats-mode-readable-scalable-and-maintainable-6bbfd000809e?source=collection_archive---------2----------------------->

## 使用访问者设计模式避免不必要的 if-else

![](img/801a1523ef0fd737be056aee5f754e7e.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[尼克·费因斯](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral)拍摄

**冗余和可避免的**条件语句是一个**码闻**。
这些允许我们基于条件逻辑选择性地执行代码。过度使用条件会导致代码难以理解和修改。