# 使用 Github 动作将 Java Spring 服务部署到虚拟机

> 原文：<https://medium.com/javarevisited/deploying-a-java-spring-service-to-a-virtual-machine-using-github-actions-d7d4c2398cbb?source=collection_archive---------1----------------------->

![](img/7f43b27a43ec92641b51a6320f497bdf.png)

弗洛里安·克拉姆在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

虽然使用容器而不是虚拟机来部署应用程序确实有一些明显的好处，但在某些情况下，由于成本或其他需求，您可能更喜欢部署到虚拟机。

在这篇文章中，我们将详细介绍将 Java Spring 服务部署到虚拟环境所需的步骤