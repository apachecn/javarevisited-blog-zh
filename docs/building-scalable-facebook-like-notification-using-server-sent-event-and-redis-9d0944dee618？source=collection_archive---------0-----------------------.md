# 使用服务器发送的事件和 Redis 构建可伸缩的类似脸书的通知

> 原文：<https://medium.com/javarevisited/building-scalable-facebook-like-notification-using-server-sent-event-and-redis-9d0944dee618?source=collection_archive---------0----------------------->

![](img/65c718668ae050a98969d0feea4662b6.png)

照片由 [**克里斯蒂安娜**](https://www.pexels.com/@cristian-dina-924373?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**佩克斯**](https://www.pexels.com/photo/white-smartphone-1851415/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

有时，我们希望服务器通知客户端一些变化。这在传统的 HTTP web 应用程序中是不可能的。在传统的 web 应用程序中，客户端必须与服务器建立连接，然后等待服务器的响应。这就是服务器端事件正在解决的问题。背后的想法…