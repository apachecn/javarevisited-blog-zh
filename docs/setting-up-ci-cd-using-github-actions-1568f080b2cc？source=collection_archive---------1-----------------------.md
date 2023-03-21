# 使用 GitHub 操作设置 CI/CD

> 原文：<https://medium.com/javarevisited/setting-up-ci-cd-using-github-actions-1568f080b2cc?source=collection_archive---------1----------------------->

![](img/97e33be93e67ae795b71bc9bc802f59f.png)

照片由 [Richy Great](https://unsplash.com/@richygreat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/github?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

GitHub Action 是你想在某个事件中完成的任务。

例如，在 Push to a master 事件中，您希望构建、运行测试、创建 docker 映像、push to docker hub 并部署到 Kubernetes。

构建、运行、创建、推送和部署都是 GitHub 的动作。

GitHub 有一个资源库，你可以在里面找到几乎任何任务的动作…