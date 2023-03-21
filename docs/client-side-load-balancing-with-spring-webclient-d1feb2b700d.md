# 使用 Spring WebClient 实现客户端负载平衡

> 原文：<https://medium.com/javarevisited/client-side-load-balancing-with-spring-webclient-d1feb2b700d?source=collection_archive---------1----------------------->

## Spring cloud 支持几个用于服务发现的注册中心 *(Apache Zookeeper，网飞的 Eureka，hashi corp consult)*，我们将看看它如何与定制实现一起工作。通常我们不会调用同一个([硬编码](https://en.wikipedia.org/wiki/Hard_coding#:~:text=Hard%20coding%20(also%20hard%2Dcoding,or%20generating%20it%20at%20runtime.))实例来获得响应，除非用[负载均衡器](https://www.nginx.com/resources/glossary/load-balancing/)隐藏它。如果另一个 API 托管在多台机器上，我们如何…