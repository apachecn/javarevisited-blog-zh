# 在 AWS EC2 上部署 Java Spring Boot 应用程序

> 原文：<https://medium.com/javarevisited/deploying-a-java-spring-boot-application-on-aws-ec2-71a8c67a84a1?source=collection_archive---------1----------------------->

这是关于在 AWS EC2 服务器上部署 JAVA Spring Boot 项目的简短说明。

假设你已经用 Ubuntu OS 创建了一个 [EC2 实例](/javarevisited/7-best-aws-ec2-amazon-elastic-compute-cloud-online-courses-for-beginners-in-2021-f7a1a55ea719)，并用 puTTy 连接到它。

# MySQL 数据库

## 安装 MySQL

```
sudo apt install mysql-serversudo mysql_secure_installationsudo mysql
```

## 创建用户并授予权限