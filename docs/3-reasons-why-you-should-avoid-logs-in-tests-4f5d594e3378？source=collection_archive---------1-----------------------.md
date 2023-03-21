# 测试中应该避免日志的 3 个原因

> 原文：<https://medium.com/javarevisited/3-reasons-why-you-should-avoid-logs-in-tests-4f5d594e3378?source=collection_archive---------1----------------------->

## 这就是单元测试不需要日志记录的原因

![](img/2cf484b53c8dcd4fb083145dfae9da23.png)

照片由来自[佩克斯](https://www.pexels.com/photo/man-in-white-crew-neck-t-shirt-and-blue-denim-jeans-sitting-on-brown-sofa-5702465/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[马体·米罗什尼琴科](https://www.pexels.com/@tima-miroshnichenko?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

*单元测试中的日志是不好的实践。*

无关紧要，毫无意义，并堵塞控制台输出。单元测试日志减缓了您的构建、交付和开发。

下面是为什么你不应该登录单元测试的原因。

# 日志添加了更多代码