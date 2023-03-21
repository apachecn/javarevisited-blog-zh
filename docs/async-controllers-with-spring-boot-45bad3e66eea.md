# 带 Spring Boot 的异步控制器

> 原文：<https://medium.com/javarevisited/async-controllers-with-spring-boot-45bad3e66eea?source=collection_archive---------0----------------------->

## 当您在 Spring 中搜索异步方法时，您会了解到@EnableAsync 注释，并将`@Async`添加到您的方法中。一切正常，您的方法在另一个线程上执行，但是当异常发生时会发生什么呢？如何处理不愉快的流程而不丢失最初请求的上下文？