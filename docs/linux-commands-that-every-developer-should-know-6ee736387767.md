# 每个开发人员都应该知道的 Linux 命令

> 原文：<https://medium.com/javarevisited/linux-commands-that-every-developer-should-know-6ee736387767?source=collection_archive---------2----------------------->

![](img/0c22e9ebd289ce81eaa052adf2f9d275.png)

Emile Perron 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

本人工作多年，以下是我为工作整理的 [LINUX 常用命令](/javarevisited/10-essential-linux-commands-every-developer-and-devops-should-know-9df39391aac7)，完全足够日常使用。

# 1.系统服务管理

## 1.1 系统 ctl

*   输出系统中每个服务的状态:

```
systemctl list-units --type=service
```