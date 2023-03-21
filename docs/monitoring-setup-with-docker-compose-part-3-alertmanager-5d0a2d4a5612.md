# 使用 docker-compose 设置 Alertmanager。

> 原文：<https://medium.com/javarevisited/monitoring-setup-with-docker-compose-part-3-alertmanager-5d0a2d4a5612?source=collection_archive---------0----------------------->

## 完整的监控堆栈—第 3 部分。

本教程是 [**监控栈系列**](https://verbosemode.dev/list/monitoring-stack-with-prometheus-grafana-and-docker-3e6e4b94523c) 的第 3 部分。

## **简而言之:什么是 Alertmanager？**

[警报管理器](https://prometheus.io/docs/alerting/latest/alertmanager/)向各种渠道发送警报，如 Slack 或电子邮件。

回想一下第一部分[中的内容，如果有东西违反了规则，普罗米修斯就会发出警报。您也可以使用 Alertmanager 来静默和分组警报。](https://dashdashverbose.medium.com/monitoring-setup-with-docker-compose-part-1-prometheus-3d2c9089ee82)

## 配置