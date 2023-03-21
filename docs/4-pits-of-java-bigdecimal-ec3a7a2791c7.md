# Java BigDecimal 的 4 个坑

> 原文：<https://medium.com/javarevisited/4-pits-of-java-bigdecimal-ec3a7a2791c7?source=collection_archive---------1----------------------->

![](img/eb2ab59a51c6bc4351a02d37510af5f3.png)

照片由[嚎叫的红色](https://unsplash.com/@howlingred70?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我已经工作多年，在使用 BigDecimal 时必须非常小心，因为有太多的陷阱。

Java 在`java.math`包中提供的 API 类 BigDecimal 用于对超过 16 位有效数字的数字进行精确运算。双精度浮点变量 double 可以处理 16 位有效数字，但在实际应用中，可能需要…