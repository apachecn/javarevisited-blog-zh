# 在 MacOS 上启动 Mongo DB 集群

> 原文：<https://medium.com/javarevisited/start-mongo-db-cluster-on-localhost-a87de276595f?source=collection_archive---------1----------------------->

![](img/9af073c55ca5c8e936438a9f0c86bc17.png)

将 mongo 作为独立的单个实例启动非常简单。但是在生产环境中，mongo db 总是以集群格式运行。

因此，如何在您的本地机器上或者在临时实例上或者出于测试目的复制相同的集群格式 [mongo db](https://javarevisited.blogspot.com/2019/01/top-5-mongodb-online-training-courses.html) 变得至关重要。

这些步骤包括:

1.  下载 mongo 二进制文件(Mongo 安装)
2.  为集群的不同节点创建数据目录
3.  启动指向不同端口的 mongo 节点(因为我们在一台机器上运行所有节点)
4.  启动副本集
5.  随时可以使用

## 1.下载 mongo 二进制文件

从 [mongo 官网](https://www.mongodb.com/try/download/community)下载任意 mongo 库。

提取本地目录中的二进制文件。假设我们提取了/Users/my user/Documents/MongoDB 中的二进制文件。

## 2.为集群的不同节点创建数据目录

现在，为集群的每个节点创建不同的数据目录。为简单起见，让我们用节点本身的名称来创建目录。

> mkdir-p/Users/my user/data/MongoDB/rs0–0
> 
> mkdir-p/Users/my user/data/MongoDB/rs0–1
> 
> mkdir-p/Users/my user/data/MongoDB/rs0–2

创建指向 mongo 二进制文件的软链接，如下所示:

> sudo CP/Users/my user/Documents/MongoDB/bin/*/usr/local/bin
> 
> sudo ln-s/Users/my user/Documents/MongoDB/bin/*/usr/local/bin

## 3.启动指向不同端口的 mongo 节点

> CD/Users/my user/Documents/MongoDB/bin

mongod — replSet rs0 —端口 27017—bind _ IP localhost—dbpath/Users/my user/data/MongoDB/rs0–0—oplogSize 128

在终端中打开新标签，并启动另一个节点

> mongod — replSet rs0 —端口 27018—bind _ IP localhost—dbpath/Users/my user/data/MongoDB/rs0–1—oplogSize 128

在终端中打开另一个标签，并启动第三个节点

> mongod — replSet rs0 —端口 27019—bind _ IP localhost—dbpath/Users/my user/data/MongoDB/rs0–2—oplogSize 128

## 4.启动副本集

现在，打开新选项卡并连接到在端口 27017 上运行的节点，如下所示:

> 蒙戈港 27017

现在你在 mongo 控制台。现在，我们需要使用以下命令启动副本集:

> rs.initiate({
> … _id: "rs0 "，
> …成员:[
> … {
> … _id: 0，
> …主机:" localhost:27017"
> … }，
> … {
> … _id: 1，
> …主机:" localhost:27018"
> … }，
> … {
> … _id: 2，
> …主机:" localhost:227。

## 5.随时可以使用。

现在您的 mongo 集群已经准备好了，您可以将它与您的应用程序连接起来。

希望这是有益的。快乐学习！！！