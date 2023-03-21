# 为什么 Java 专家避免模仿

> 原文：<https://medium.com/javarevisited/why-java-experts-avoid-mocks-ed71f54ac81c?source=collection_archive---------0----------------------->

## 正确的嘲笑方式是什么——假的，嘲弄的，还是存根的？

![](img/e684f4b0cd20cbbaeb8dc1d4577a8501.png)

图片由 [RF 提供。_.来自](https://www.pexels.com/@rethaferguson?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)[像素](https://www.pexels.com/photo/crop-focused-repairman-fixing-graphics-card-on-computer-3825582/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的工作室

*“不要使用嘲笑。”* [](https://www.yegor256.com/2014/09/23/built-in-fake-objects.html)*“莫须有皆蠢。”*[](https://medium.com/r?url=https%3A%2F%2Fwww.endoflineblog.com%2Ftesting-with-doubles-or-why-mocks-are-stupid-part-4)

说起来容易做起来难。嘲笑无处不在。没有模拟就不能测试。

*有哪些替代方案？为什么模仿不是“一刀切”的？其他测试替身怎么用？*

# 模拟替代品做什么…