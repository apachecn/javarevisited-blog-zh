# 优化 AWS 基础设施

> 原文：<https://medium.com/javarevisited/optimizing-aws-infrastructure-7a8744801284?source=collection_archive---------3----------------------->

![](img/3de6fb8df2b95a89c3480dfd90577bb5.png)

亚历克斯·库利科夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

通过维护 AWS 生产基础设施超过 6 个月获得的优化经验:

1.  在较低环境中关闭 EC2 和 RDS:

随用随付的定价模式是云相对于本地服务器的主要优势。为了利用这一点，我们编写了一个 lambda，每天晚上由 eventbridge cron 触发。这个 lambda 将关闭我们的非 prod 帐户使用的所有 EC2s 和 RDS 实例。

2.参考 [EC2](/javarevisited/7-best-aws-ec2-amazon-elastic-compute-cloud-online-courses-for-beginners-in-2021-f7a1a55ea719) 使用私有 IP 使得流量被限制在 VPC 境内:

用 EC2 的公共 IP 而不是私有 IP 来指代 EC2 的天真错误导致流量通过互联网路由，从而增加了出站流量和 NAT 网关的使用成本。

3.为 [S3](/javarevisited/7-best-aws-s3-and-dynamodb-courses-for-beginners-in-2021-a8a44b6066da) 、Cloudwatch 和其他位于 VPC 之外的托管服务定义 VPC 端点

这有助于在 VPC 内保持流量私有，并降低 NAT 网关成本。

4.在 ALB 终止 SSL

在典型的 3 层架构 ALB->EC2 -> RDS 中，在 ALB 级别终止加密有助于节省 EC2 解密流量的计算成本。从 ALB 到 EC2，SSL 是冗余的，因为这是我们 VPC 内的私有流量。