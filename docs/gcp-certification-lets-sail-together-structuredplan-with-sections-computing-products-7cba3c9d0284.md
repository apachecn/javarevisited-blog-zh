# GCP 认证-让我们一起航行。结构化计划，包含多个部分:计算产品。

> 原文：<https://medium.com/javarevisited/gcp-certification-lets-sail-together-structuredplan-with-sections-computing-products-7cba3c9d0284?source=collection_archive---------3----------------------->

![](img/cb3b86ffc474598c39b5b9da333a28b9.png)

大家好，

[1 分钟了解谷歌云认证|谷歌云博客](https://cloud.google.com/blog/topics/developers-practitioners/get-know-google-cloud-certifications-1-minute)

在此，我向您展示一份包含多个部分的结构化计划，作为计划获得解决方案架构师认证的云有志者的参考。

**了解基本知识:**

1.  [云计算简介](https://javarevisited.blogspot.com/2019/07/top-5-online-courses-to-learn-cloud-computing-aws.html)
2.  不同云平台的概述([AWS](/javarevisited/5-best-aws-courses-for-beginners-and-experienced-developers-to-learn-in-2021-563212409fbd)/[Azure](/javarevisited/5-best-azure-fundamentals-courses-to-pass-az-900-certification-exam-in-2020-9e602aea035d)/[GCP](/javarevisited/5-best-courses-to-learn-google-cloud-platform-gcp-in-2021-169093a3771a))—一个很好的对比[将 AWS 和 Azure 服务与谷歌云进行对比](https://cloud.google.com/free/docs/aws-azure-gcp-service-comparison)
3.  谷歌云平台及其可用产品概述

**感受谷歌云产品:** [*产品说明书*](https://raw.githubusercontent.com/gregsramblings/google-cloud-4-words/master/DarkPoster-lowres.png)

1.  [网络产品](https://cloud.google.com/products/networking)
2.  [计算产品](https://cloud.google.com/products/compute)
3.  [数据库产品](https://cloud.google.com/products/databases)
4.  [存储产品](https://cloud.google.com/products/storage)
5.  [集装箱](https://cloud.google.com/products/#section-6)
6.  [开发者工具](https://cloud.google.com/products/tools)
7.  [安全和身份产品](https://cloud.google.com/products/security-and-identity)

…..更多，但这应该有助于您开始并在您的旅程中不断增加。

在动手之前，先熟悉

1.  [谷歌云控制台](https://cloud.google.com/cloud-console)
2.  [Google Cloud shell](https://cloud.google.com/shell) — [命令行](https://cloud.google.com/sdk/gcloud)工具是创建和管理 Google Cloud 资源的主要 CLI 工具
3.  [谷歌云存储](https://cloud.google.com/storage) — [gsutil 访问它](https://cloud.google.com/storage/docs/gsutil)
4.  [bq 命令行工具](https://cloud.google.com/bigquery/docs/bq-command-line-tool) —基于 Python 的 BigQuery 命令行工具。
5.  [地区和区域](https://cloud.google.com/compute/docs/regions-zones)和[全球位置](https://cloud.google.com/about/locations)

*————**计算**开始— — — — *

计算选项—

1.  计算引擎—用于部署虚拟机的托管环境
2.  [Kubernetes 引擎](/javarevisited/10-best-kubernetes-courses-for-developers-and-devops-engineers-94c35cd3a2fd) —用于部署容器化应用的托管环境
3.  App Engine —用于部署应用程序的托管无服务器平台
4.  云功能—用于部署事件驱动功能的托管无服务器平台
5.  云运行—在完全托管的无服务器平台上开发和部署高度可扩展的容器化应用程序。

参考:[应用托管选项](https://cloud.google.com/hosting-options)

*—————**计算**继续— — — — *

[**计算引擎:虚拟机**](https://cloud.google.com/compute)

[自动缩放实例组](https://cloud.google.com/compute/docs/autoscaler)

1.  基于 CPU 利用率
2.  基于负载均衡服务能力
3.  基于云监控指标
4.  基于时间表
5.  基于预测性自动扩展—基于 CPU 利用率

*————**计算**继续— — — — *

**Google Kubernetes 引擎—必去浏览—** [十二要素 App](https://12factor.net/)

深潜前的基本知识

1.  虚拟化与容器化
2.  [码头工人](/javarevisited/5-best-docker-courses-for-java-and-spring-boot-developers-bbf01c5e6542)
3.  码头枢纽
4.  Docker 桌面
5.  [集装箱](https://javarevisited.blogspot.com/2018/02/10-free-docker-container-courses-for-Java-Developers.html#axzz5XlFjtjA0)
6.  库贝特尔
7.  GKE
8.  头盔包管理器
9.  舵图
10.  单元级缩放—参考[使用 GKE 自动缩放:集群和节点](https://youtu.be/VNAWA6NkoBs)
    水平单元自动缩放
    垂直单元自动缩放
11.  节点级扩展—参见[GKE 自动扩展:集群和节点](https://youtu.be/VNAWA6NkoBs)
    集群自动扩展(水平基础架构解决方案)
    节点自动配置(垂直基础架构解决方案)
12.  Kubernetes 控制器对象
13.  GKE 对象-服务

很好的参考: [Kubernetes 节点端口 vs 负载均衡器 vs 入口？什么时候该用什么？](/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0)

*—————**计算**继续— — — — *

服务提供对指定 pod 的负载平衡访问。有三种主要类型的服务:

● ClusterIP:在只能从该集群内部访问的 IP 地址上公开服务。这是默认类型。

● NodePort:在集群中的每个节点的 IP 地址上，在特定的端口号上公开服务。可以从群集外部访问。

●负载平衡器:使用云提供商提供的负载平衡服务对外公开服务。可以从群集外部访问。

GKE 提供了三种不同类型的负载平衡器来控制访问，并将传入流量尽可能均匀地分布在集群中。您可以将一个服务配置为同时使用多种类型的负载平衡器。

*   [**外部负载平衡器**](https://cloud.google.com/kubernetes-engine/docs/concepts/network-overview#ext-lb) 管理来自集群外部和您的谷歌云 VPC 网络外部的流量。他们使用与谷歌云网络相关的转发规则将流量路由到 Kubernetes 节点。
*   [**内部负载平衡器**](https://cloud.google.com/kubernetes-engine/docs/concepts/network-overview#int-lb) 管理来自同一个 VPC 网络的流量。像外部负载平衡器一样，它们使用与谷歌云网络相关的转发规则将流量路由到 Kubernetes 节点。
*   [**HTTP(S)负载平衡器**](https://cloud.google.com/kubernetes-engine/docs/concepts/network-overview#https-lb) 是专门用于 HTTP(S)流量的外部负载平衡器。它们使用入口资源而不是转发规则将流量路由到 Kubernetes 节点。

*—————**计算**继续— — — — *

GKE 的好处:-

*   谷歌云针对计算引擎实例的[负载平衡](https://cloud.google.com/compute/docs/load-balancing-and-autoscaling)
*   [节点池](https://cloud.google.com/kubernetes-engine/docs/concepts/node-pools)用于指定集群中的节点子集，以提供额外的灵活性
*   [集群节点实例计数的自动扩展](https://cloud.google.com/kubernetes-engine/docs/cluster-autoscaler)
*   [集群节点软件的自动升级](https://cloud.google.com/kubernetes-engine/docs/concepts/node-auto-upgrades)
*   [节点自动修复](https://cloud.google.com/kubernetes-engine/docs/concepts/node-auto-repair)维护节点健康和可用性
*   [使用 Google Cloud 的操作套件进行日志记录和监控](https://cloud.google.com/monitoring/kubernetes-engine),了解您的集群

[GKE 草图注释](https://thecloudgirl.dev/GKE.html)

* — — — — — — —自动缩放— — — *

GKE 通过使用如下特性处理自动缩放场景:参考[使用 GKE 自动缩放:集群和节点](https://youtu.be/VNAWA6NkoBs)

*   [水平机架自动缩放器(HPA)](https://cloud.google.com/architecture/best-practices-for-running-cost-effective-kubernetes-applications-on-gke#horizontal_pod_autoscaler) 用于根据利用率指标添加和移除机架。
*   [立式豆荚自动秤(VPA)](https://cloud.google.com/architecture/best-practices-for-running-cost-effective-kubernetes-applications-on-gke#vertical_pod_autoscaler) 用于调整豆荚大小。
*   [集群自动缩放器](https://cloud.google.com/architecture/best-practices-for-running-cost-effective-kubernetes-applications-on-gke#cluster_autoscaler)，用于根据计划的工作负载添加和删除节点。
*   [节点自动配置](https://cloud.google.com/kubernetes-engine/docs/how-to/node-auto-provisioning)用于动态创建新的节点池，其节点可满足用户的需求。

* — — — — — —经营方式— — *

GKE 操作模式

1.  Auto Pilot : Google 管理集群和节点基础架构(区域范围)。
2.  标准:客户管理群集和节点基础架构(区域和分区范围)。

*—————**计算**继续— — — — *

GKE 集群:

1.  [带状星团](https://cloud.google.com/kubernetes-engine/docs/concepts/types-of-clusters#zonal_clusters)

a.[单区域集群](https://cloud.google.com/kubernetes-engine/docs/concepts/types-of-clusters#single-zone_clusters)

b.[多区域集群](https://cloud.google.com/kubernetes-engine/docs/concepts/types-of-clusters#multi-zonal_clusters)

2. [regional_clusters](https://cloud.google.com/kubernetes-engine/docs/concepts/types-of-clusters#regional_clusters) —更适合生产工作负载，因为它提供了高可用性

3.[私有集群](https://cloud.google.com/kubernetes-engine/docs/concepts/private-cluster-concept)—*私有集群*是 [VPC 本地集群](https://cloud.google.com/kubernetes-engine/docs/concepts/alias-ips)的一种，仅依赖于[内部 IP 地址](https://cloud.google.com/vpc/docs/ip-addresses)。私有集群中的节点、单元和服务需要[唯一的子网 IP 地址范围](https://cloud.google.com/kubernetes-engine/docs/concepts/alias-ips#cluster_sizing)。

4.[多集群入口](https://cloud.google.com/kubernetes-engine/docs/concepts/multi-cluster-ingress)-是一个面向 GKE 集群的云托管多集群入口控制器。这是谷歌托管的服务，支持跨集群部署共享负载平衡资源**和跨地区部署共享负载平衡资源**和**。**

**好参考** — [使用 Google Kubernetes 引擎的多集群入口](https://youtu.be/6Aof15getu0)

5.[容器本地负载均衡](https://cloud.google.com/kubernetes-engine/docs/concepts/container-native-load-balancing) —使[多种负载均衡器](https://cloud.google.com/load-balancing/docs/negs/zonal-neg-concepts)能够直接以 pod 为目标，并将流量平均分配给 pod。

6.[网络端点组概述](https://cloud.google.com/load-balancing/docs/negs) — NEG:是指定一组后端端点或服务的配置对象。这种配置的一个常见用例是在容器中部署服务。

*—————**计算**继续— — — — *

[Google App Engine](https://cloud.google.com/appengine/docs/) —是一个完全托管的无服务器平台，用于大规模开发和托管网络应用。您可以从几种流行的语言、库和框架中进行选择来开发您的应用程序，然后让 App Engine 负责根据需求配置服务器和扩展您的应用程序实例。

1.  [谷歌应用引擎灵活环境](https://cloud.google.com/appengine/docs/flexible/)
2.  [谷歌应用引擎标准环境](https://cloud.google.com/appengine/docs/standard/)

*————**计算**继续— — — — *

[云运行](https://cloud.google.com/run) —在完全托管的无服务器平台上开发和部署高度可扩展的容器化应用。

重要特性— [容器实例自动缩放](https://cloud.google.com/run/docs/about-instance-autoscaling):每个[修订版](https://cloud.google.com/run/docs/resource-model#revisions)都会自动缩放到处理所有传入请求所需的容器实例数量。当一个修订没有接收到任何流量时，默认情况下，它被缩放到零个容器实例中。

除了传入请求的速率之外，计划的实例数量还受到以下因素的影响:

*   现有实例的 CPU 利用率(目标是将调度的实例保持在 60%的 CPU 利用率)
*   [最大并发设置](https://cloud.google.com/run/docs/about-concurrency)
*   [最大容器实例数设置](https://cloud.google.com/run/docs/configuring/max-instances)
*   [容器实例的最小数量设置](https://cloud.google.com/run/docs/configuring/min-instances)

*—————**计算**继续— — — — *

[云功能](https://cloud.google.com/functions/docs/concepts/overview) —是一个用于构建和连接云服务的无服务器执行环境。使用云函数，您可以编写简单、单一用途的函数，这些函数附加到从您的云基础架构和服务发出的事件上。

当被监视的事件被触发时，您的函数被触发。您的代码在完全托管的环境中执行。不需要配置任何基础架构，也不需要担心管理任何服务器。

了解 IOT 核心和通过云函数的调用非常方便。

[**物联网核心**](https://cloud.google.com/iot/docs/concepts/overview#key_concepts) **—** 一项完全托管的服务，可轻松安全地连接、管理和接收来自全球分散设备的数据。

*————**计算**结束— — — — *

参考消息— [谷歌云平台服务水平协议](https://cloud.google.com/terms/sla/)

好样的…..让我们一起航行吧。

*——————**问题** — — — — —

为了让它更令人兴奋，我会在这里列出一些问题，鼓励你在评论区回答:-

**回答:**查看评论

```
Q1\. Your company just finished a rapid lift and shift to Google Compute Engine for your computing needs. You have another 9 months to design and deploy a more cloud-native solution. The business team is looking for services with lesser responsibility and easy manageability. Please select the order of services with lesser responsibility to more responsibility.a. GKE > Google App Engine Standard Environment > Cloud Functions > Compute Engine with containers > Compute Engine
b. Cloud Functions > Google App Engine Standard Environment > GKE > Compute Engine with containers > Compute Engine
c. Google App Engine Standard Environment > Cloud Functions>Compute Engine with containers > GKE > Compute Engine
d. Cloud Functions > GKE > Google App Engine Standard Environment > Compute Engine > Compute Engine with containersQ2.You work for an organization called QwikCloud. Your organization has decided to implement continuous integration and delivery (CI/CD) pipeline on Google Cloud Platform using only managed services and the popular GitOps methodology. The architecture includes many containerized microservices that are updated frequently and rolled back. Please select the GCP services that should be used in order build the CICD pipeline and to host the containerized workloads.a. Cloud Storage, Cloud Dataflow, Compute Engine
b. Cloud Source repositories, Jenkins on Compute Engine, Container Registry, Google Kubernetes Engine
c. BitBucket, Cloud Build, Container Registry, Google Kubernetes Engine
d. Cloud Source repositories, Cloud Build, Container Registry, Google Kubernetes EngineQ3\. You have an application running on a managed instance group. To ensure that your application will handle the load even if an entire zone fails, what should you do? Select all correct options.a. Don't create the "Multizone" managed instance groups.
b. Spread your managed instance group over two zones and overprovision by 100%. (for Two Zone)
c. Overprovision your regional managed instance group by at least 50%. (for Three Zones)
d. Create a regional unmanaged instance group and spread your instances across multiple zoneQ4\. Your company’s test suite is a custom C++ application that runs tests throughout each day on Linux virtual machines. The full test suite takes several hours to complete, running on a limited number of on-premises servers reserved for testing. Your company wants to move the testing infrastructure to the cloud, to reduce the amount of time it takes to fully test a change to the system while changing the tests as little as possible. Which cloud infrastructure should you recommend?a. Google Cloud Dataproc to run Apache Hadoop jobs to process each test
b. Google App Engine Standard with Google Stackdriver for logging
c. Google Compute Engine managed instance groups with auto-scaling
d. Google Compute Engine unmanaged instance groups and Network Load BalancerQ5\. Your customer is receiving reports that their recently updated Google App Engine application is taking approximately 30 seconds to load for some of their users. This behaviour was not reported before the update. What strategy should you take?a. Rollback to an earlier known good release, then push the release again at a quieter period to investigate. Then use Cloud Trace and Logging to diagnose the problem.
b. Rollback to an earlier known good release initially, then use Cloud Trace and Logging to diagnose the problem in a development/test/staging environment.
c. Work with your ISP to diagnose the problem.
d. Open a support ticket to ask for network capture and flow data to diagnose the problem, then roll back your application.Q6\. You are creating a single preemptible VM instance named “preempt-vm” to be used as scratch space for a single workload. If your VM is preempted, you need to ensure that disk contents can be re-used. Which gcloud command would you use to create this instance?a. gcloud compute instances create "preempt-vm" –preemptible –boot-disk-auto-delete=no
b. gcloud compute instances create "preempt-vm" –no-auto-delete
c. gcloud compute instances create "preempt-vm" –preemptible –no-boot-disk-auto-delete
d. gcloud compute instances create "preempt-vm" –preemptible
```

*————**问题**结束— — — — *