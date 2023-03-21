# 用 TLS 配置 RabbitMQ MQTT

> 原文：<https://medium.com/javarevisited/configuring-rabbitmq-mqtt-with-tls-ca1dcbc397d3?source=collection_archive---------0----------------------->

# 语境

当我们使用 MQTT 进行物联网通信时，建议在连接上使用 TLS。TLS 有两个主要目的:加密连接流量，并提供一种方法来验证对等体是否可信

在我的例子中，我使用带有 MQTT 插件的 RabbitMQ 来支持 MQTT 协议。

在本文中，我将展示如何使用 RabbitMQ 创建 docker 映像，从而启用 MQTT 插件和 TLS 支持。