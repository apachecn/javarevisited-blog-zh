# 使用 Azure Kubernetes 服务创建 Kafka 集群

> 原文：<https://medium.com/javarevisited/create-a-kafka-cluster-with-azure-kubernetes-service-6ef811d3ae4b?source=collection_archive---------0----------------------->

## 使用 Azure Kubernetes 服务(AKS)创建具有外部访问的 Kafka 集群

![](img/ef8264d6e4e5222a18e5c1a5b18974b0.png)

图片由 Unsplash 提供

在本文中，我们将演示如何使用 Azure Kubernetes 服务创建一个 Apache Kafka 集群。我们还将确保不在 AKS 集群中运行的外部应用程序也可以访问 Kafka 主题。