# Kubernetes 的 KoolKits——OSS 调试工具包简介

> 原文：<https://medium.com/javarevisited/introducing-koolkits-oss-debugging-toolkits-for-kubernetes-296b000990ea?source=collection_archive---------1----------------------->

![](img/c11a51aa367f158c4ff640e257eb0491.png)

KoolKits(**K**ubernetes t**ool kits**)是高度固执己见的、特定于语言的、包含调试容器镜像的电池，用于 Kubernetes 。在实践中，如果您在一个不熟悉的 shell 中进行艰难的调试会话时遇到困难，那么您应该在生产 pod 上安装它们。

简单介绍一下背景，请注意这些容器图像旨在与新的`[kubectl debug](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-running-pod/#ephemeral-container)` [功能](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-running-pod/#ephemeral-container)一起使用，该功能会启动[临时容器](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/)进行交互式故障排除。KoolKit 将由`kubectl debug`拉出，作为 pod 中的一个容器，并且能够访问与原始容器相同的进程名称空间。

因为生产容器通常是 [**而不是**](https://cloud.google.com/architecture/best-practices-for-building-containers#remove_unnecessary_tools) 光秃秃的，所以使用 KoolKit 可以让您**使用电动工具**进行故障排除，而不是依赖由于最初构建生产映像的人的慷慨(或粗心)而留下的东西。

每个 KoolKit 中的工具都是精心挑选的，你可以在下面的中阅读更多关于整个项目背后的动机。

如果你只是想看看好东西，可以在 GitHub 上查看完整的项目[。](https://github.com/lightrun-platform/koolkits)

# 调试 Kubernetes 很难

了解 Kubernetes pod 内部的情况并不容易。

首先，您的应用程序不再是一个单一的实体——它由多个单元组成，为水平扩展而复制，有时甚至分散在多个集群中。

此外，要使用本地工具(如调试器)访问您的应用程序，您需要处理讨厌的网络问题，如发现和端口转发，这会降低此类工具的使用速度。

而且，分布式系统世界的皇冠上的宝石——改变正在运行的 pod 的状态或完全停止它(例如，当放置一个[断点](https://javarevisited.blogspot.com/2011/07/java-debugging-tutorial-example-tips.html#axzz6bYzaddcE)时)可能会导致系统的其他部分发生级联故障，这将加剧现有的问题。

# KoolKits 背后的动机

Lightrun 在构建时就考虑到了 Kubernetes 我们在多个 pod、多个集群甚至多个云中工作。我们很早就明白，通过使用正确的工具来打包一个 punch 对于故障排除开发人员来说是一个巨大的动力来源——我们认为我们会以某种方式找到一种方法来回报社区——这就是我们如何想到 KoolKits 的想法。

让我们深入一下，解释一下为什么 KoolKits 非常有用:

有一个[众所周知的 Kubernetes 最佳实践](https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-how-and-why-to-build-small-container-images)指出应该构建**小型**容器映像。出于几个不同的原因，这是有意义的:

1.  构建映像将消耗更少的资源(即 CI 工时)
2.  拉图片将花费更少的时间(谁愿意为这么多的入口付费呢？)
3.  更少的东西意味着暴露于安全漏洞的表面区域更少，在这个[世界中，甚至无操作日志记录都不再安全](https://en.wikipedia.org/wiki/Log4Shell)

也有很多工具可以帮助您不需要做太多繁重的工作就能达到目的:

1.  [Alpine Linux](https://hub.docker.com/_/alpine) 基础映像超级小
2.  [disloses Docker images](https://github.com/GoogleContainerTools/distroless)更进一步，删除除运行时之外的所有内容
3.  [Docker 多阶段构建](https://docs.docker.com/develop/develop-images/multistage-build/)帮助创建精简的最终产品映像

当您试图调试这些容器中发生的事情时，问题就出现了。通过使用一个小的产品映像，你就放弃了大量的工具，而这些工具在你思考应用程序中的问题时是非常有价值的。

通过使用 KoolKit，您可以在不牺牲工具质量的情况下享受小型产品映像的好处——每个 KoolKit 都包含针对它所代表的特定运行时精心挑选的工具，此外还有一组针对基于 Linux 的系统的更通用的工具。

*P.S. KoolKits 的灵感来源于* `[*kubespy*](https://github.com/huazhihao/kubespy)` *和* `[*netshoot*](https://github.com/nicolaka/netshoot)` *。*

# 考虑

在构建这些图像的过程中，我们做了很多决定——下面列出了我们考虑的一些事情。

# 图像尺寸

KoolKits Docker 图像倾向于运行，嗯，而不是**大**。

KoolKits 旨在下载一次，保存在集群的 Docker 注册表中，然后根据需要立即作为容器启动。因为它们不是用来持续拉动的，因为它们是用来装满好吃的东西的，这是我们愿意忍受的副作用。

# 使用 Ubuntu 基本图像

很难创建一个真正苗条的图像的部分原因是因为我们决定使用完整的 Ubuntu 20.04 系统作为每个 KoolKit 的基础。这主要是因为我们希望在集群内部复制与本地调试相同的环境。

例如，这意味着不要用 Alpine 替代你习惯使用的普通 Ubuntu 包。实际上，这意味着我们有办法在每个 KoolKit 中包含没有 Alpine 版本的工具。

# 使用语言版本管理器

每个 KoolKit 都(尽可能地)使用语言版本管理器，而不是依赖于特定语言的发行版。这样做是为了让您能够轻松地安装旧的运行时版本，并且允许您根据需要在运行时版本之间随意切换(例如，为了获得只针对特定运行时版本而存在的工具的特定版本)。

# 可用的 KoolKits

repo 中的每个文件夹都包含 KoolKit 后面的 docker 文件和调试映像的简短说明。所有的怪人都是基于`[ubuntu:20.04](https://hub.docker.com/layers/ubuntu/library/ubuntu/20.04/images/sha256-57df66b9fc9ce2947e434b4aa02dbe16f6685e20db0c170917d4a1962a5fe6a9?context=explore)`的基础形象，因为真实的人需要真实的贝壳。

可用的 KoolKits 列表:

1.  `[koolkit-jvm](https://github.com/lightrun-platform/koolkits/blob/main/jvm/README.md)`–采用 OpenJDK 17.0.2 &相关工具(包括`jabba`便于版本管理和 Maven 3.8.4)
2.  `[koolkit-node](https://github.com/lightrun-platform/koolkits/blob/main/node/README.MD)`–节点 16.13.1 &相关工具(包括`nvm`以方便版本管理)
3.  `[koolkit-python](https://github.com/lightrun-platform/koolkits/blob/main/python/README.md)`–Python 3 . 10 . 2&相关工具(包括`pyenv`，方便版本管理)

请注意，您实际上不必自己构建它们——所有的 KoolKits 都在 [Docker Hub](https://hub.docker.com/repository/docker/lightruncom/koolkits) 上公开托管，并且免费提供。

# KoolKits 来了

*   全新的，Go 1.17.7 KoolKit
*   JVM KoolKit — `[jvm-profiler](https://github.com/uber-common/jvm-profiler)`，`[jHiccup](https://github.com/giltene/jHiccup)`支持
*   Node.js KoolKit — `[llnode](https://github.com/nodejs/llnode)`，`[thetool](https://github.com/sfninja/thetool)`支持
*   Python KoolKit — `[vardbg](https://github.com/CCExtractor/vardbg)`，`[memprof](https://github.com/jmdana/memprof)`支持

# 贡献

我们非常乐意为任何图像添加我们遗漏的工具——只需[打开一个拉取请求](https://github.com/lightrun-platform/koolkits/pulls)或[一个问题](https://github.com/lightrun-platform/koolkits/issues)来建议一个。