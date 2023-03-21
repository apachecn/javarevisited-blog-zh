# Terraform 和 Azure 存储帐户后端——让我们一起航行

> 原文：<https://medium.com/javarevisited/terraform-and-azure-storage-account-backend-975b1a603aab?source=collection_archive---------1----------------------->

![](img/cbaac26185b8b017e4e97490615e70bb.png)

让我们使用 Terraform 创建 Azure 存储帐户，然后引用同一个帐户作为后端来存储 terraform 状态文件。

用例 1:在 Azure Cloud 上使用 [Terraform](/javarevisited/7-best-terraform-online-courses-for-devops-engineers-5e4dab297785) 创建一个存储帐户。

用例 2:修改 Terraform 后端，将状态文件存储在 Azure Cloud 上的 Azure 存储帐户中。

参考:[地形安装](/javarevisited/automation-lets-sail-together-terraform-hello-world-on-windows-10-27ae3be1f869)和[地形和天蓝色标识](/@chaskarshailesh/terraform-and-azure-identity-f3a179ad1846)然后继续

**用例 1 执行:-**

初始化地形

[![](img/6becfa2c6a31a027d7f6fc1cbd92a538.png)](https://javarevisited.blogspot.com/2020/08/top-5-courses-to-learn-terraform-in.html)

运行 Terraform 计划以创建计划。

![](img/198bf9c4a7ca03ce49925cce366867d3.png)

应用计划。

![](img/7f62496374bb92cf614d17c4cd61c32c.png)

**成功**:从 portal 确认用容器创建了存储帐户，以保存状态。

![](img/dfbce345066f55409b630948b6f67cd5.png)![](img/1fc5bbf82829c37a58007bad161c0c70.png)

**用例 2 执行:-**

初始化地形

![](img/25f1a9c2eaf0b0769f0c171a566752c4.png)

运行 Terraform 计划以创建计划。

![](img/6a9f1cb2023abc3d389f7ddfea2b2f31.png)

创建的地形文件列表。

![](img/fcb89072c1ffdedc0ac73c281075a515.png)

创建 backend.tf 以写入远程存储

![](img/8a9919e462a7937c71c651c11a6dd181.png)![](img/b14927c1598e6f33f34832b9a9672b59.png)

**成功**:从 portal 确认 Terraformn 状态文件已在存储帐户—容器中创建。

![](img/c29658372ed41e1e747555b41152018b.png)![](img/8596ce71329e579f34142297c9ebb309.png)

让我们继续一起航行…..！！