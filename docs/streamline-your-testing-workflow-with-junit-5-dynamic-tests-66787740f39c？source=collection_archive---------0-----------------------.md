# 使用 JUnit 5 动态测试简化您的测试工作流程

> 原文：<https://medium.com/javarevisited/streamline-your-testing-workflow-with-junit-5-dynamic-tests-66787740f39c?source=collection_archive---------0----------------------->

让我们讨论 JUnit5 的 *@TestFactory* 并使用新特性动态生成和运行许多单元测试。

![](img/8b0e74053314dac986377b59ff6c8fbe.png)

在 imgflip.com[上生成的图像](https://imgflip.com/)

## 概观

让我们假设我们有一个文件，其中有关于我们的应用程序应该如何运行的规范:许多输入和预期的输出值。例如，这可以是从另一个系统或旧应用程序导出的数据…