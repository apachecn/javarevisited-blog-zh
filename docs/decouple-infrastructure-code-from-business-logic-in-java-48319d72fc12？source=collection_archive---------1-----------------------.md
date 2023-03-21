# 用 Java 将基础设施代码从业务逻辑中分离出来

> 原文：<https://medium.com/javarevisited/decouple-infrastructure-code-from-business-logic-in-java-48319d72fc12?source=collection_archive---------1----------------------->

## 通过分离基础设施代码使业务逻辑更加清晰

## 概观

*   作为开发人员，当我们编写代码来满足业务需求时，我们的代码通常可以分为基础结构代码和主要业务逻辑。在基础设施代码中，我们通常编写关于异常处理、跟踪代码、日志记录、引导代码、配置初始化代码等等的代码。
*   这些代码获取我们的初始代码行，并使我们的函数…