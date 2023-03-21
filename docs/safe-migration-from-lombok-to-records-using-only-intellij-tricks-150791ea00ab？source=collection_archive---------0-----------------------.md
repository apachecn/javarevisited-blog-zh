# 仅使用 IntelliJ 技巧从 Lombok 安全迁移到记录

> 原文：<https://medium.com/javarevisited/safe-migration-from-lombok-to-records-using-only-intellij-tricks-150791ea00ab?source=collection_archive---------0----------------------->

将@Data 类转换为记录并更新所有使用它的地方的安全简单的 Intelij 快捷方式。

[![](img/3f8c60338252410235f6a6a47023c6b1.png)](https://www.java67.com/2022/12/10-examples-of-lombok-in-java.html)

IntelliJ Teicks-图片由 [Edson Junior](https://unsplash.com/@roinuj16?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 概观

在本文中，我们将尝试将*学生*类从带有 [Lombok 注释](https://javarevisited.blogspot.com/2021/08/how-to-use-lombok-library-in-java.html)的 java 对象转换为 Java17 记录。我们将假设这个类在我们的项目中的许多地方使用: