# 如何为空间索引创建高效的数据结构？

> 原文：<https://medium.com/javarevisited/how-do-you-create-an-efficient-data-structure-for-spatial-indexing-3555455f6510?source=collection_archive---------1----------------------->

![](img/57a2cbf7bddcb72e3a54a866982cb369.png)

*最初发表于*[*【https://edward-huang.com】*](https://edward-huang.com/algorithm/programming/tech/java/2021/01/11/data-structure-for-spatial-indexing/)*。*

数据结构帮助我们将值存储在数据中，并在需要时帮助我们有效地对这些数据进行操作。例如，如果我们想要存储一维数据点，即你将绘制成单行或字符串的自然数，我们可以使用 [1D 数组](/javarevisited/20-array-coding-problems-and-questions-from-programming-interviews-869b475b9121)来存储这些数据。为了创建快速检索(搜索)，我们将使用自然顺序索引(1 < 2…