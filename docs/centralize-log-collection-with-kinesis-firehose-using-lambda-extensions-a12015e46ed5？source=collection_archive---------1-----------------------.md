# 使用 Lambda 扩展集中日志收集

> 原文：<https://medium.com/javarevisited/centralize-log-collection-with-kinesis-firehose-using-lambda-extensions-a12015e46ed5?source=collection_archive---------1----------------------->

![](img/d86816ffd77eaeba32ce4fa26f2c057e.png)

# 介绍

本文介绍了一种使用外部扩展通过 Kinesis firehose 为 lambda 函数集中日志收集的方法。提供的代码示例显示了如何将日志直接发送到 kinesis firehose，而不发送到 AWS CloudWatch 服务。

> *注意:这是一个简单的例子* …