# 在普罗米修斯中创建记录规则

> 原文：<https://medium.com/javarevisited/create-recording-rules-in-prometheus-8a6a1c0b9e11?source=collection_archive---------3----------------------->

## 为什么我们需要录制规则，以及如何创建它们？

![](img/5d30c6a16647344ee9b5bee034d2b9ca.png)

各位开发者好，
我们已经在[上一篇](/javarevisited/prometheus-grafana-setup-to-visualize-your-servers-924773b83f3f)中看到了如何使用**普罗米修斯**和**格拉法纳进行**监控节点。现在，每当我们使用 prometheus 时，我们不只是直接使用它的指标，而是根据我们的需要和要求创建复杂的查询。这些表达式可能非常大，很难使用…