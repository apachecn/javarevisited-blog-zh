# 在 s3 存储桶之间复制 TB 数据

> 原文：<https://medium.com/javarevisited/copying-tbs-of-data-between-s3-buckets-8438dde7dadb?source=collection_archive---------0----------------------->

[![](img/ed0b74e75de3f0c2148497e0e7334a93.png)](https://medium.com/@javinpaul/top-5-aws-training-courses-to-crack-amazon-web-service-solutions-architect-associate-certification-3f4affa8f660)

# 问题陈述:

作为常规生产升级的一部分，我们尝试在 s3 存储桶中备份数据

项目数:1，000，344，大小:~130 GB

我们基本上是使用常规的 s3 命令启动备份，例如:

```
aws s3 cp --recursive s3://<bucket>>
aws s3 sync s3://<bucket> s3://<bucket>>
```

在执行过程中，我们注意到执行复制花费了数小时，没有办法让它更快，我们找到的唯一解决方法是在多个终端中并行运行这些 [aws 命令](/javarevisited/top-5-aws-training-courses-to-crack-amazon-web-service-solutions-architect-associate-certification-3f4affa8f660?source=collection_home---4------0-----------------------)，以便它们可以同时在不同的 s3 分区上操作，并更快地执行复制，这既不是一个优雅的解决方案，也不可扩展。

# 其他选项:

我们尝试了堆栈溢出和 AWS 论坛中提到的其他选项，如

## S3 批处理操作

S3 批处理操作似乎解决了这个问题，但目前它还不支持基于 KMS 密钥加密的对象。当我创建一个作业来拷贝启用了 KMS 密钥加密的存储桶的内容时，出现了以下错误:

使用了不支持的加密类型:SSE_KMS

当我进一步了解 AWS 文档时，它在“指定清单”一节中指出，不支持使用带有客户提供的密钥的服务器端加密(SSE-C)和带有 AWS KMS 托管密钥的服务器端加密(SSE-KMS)的清单

[https://docs . AWS . Amazon . com/Amazon S3/latest/dev/batch-ops-basics . html # specify-batch job-manifest](https://docs.aws.amazon.com/AmazonS3/latest/dev/batch-ops-basics.html#specify-batchjob-manifest)

## s3-dist-cp

s3-dist-cp 似乎很有希望，但是当我对一个具有接近 6 TB 数据的存储桶运行它时，该作业在运行“reduce”任务 40 分钟后失败，没有任何失败的明确指示

# 定制方法:

不幸的是，上面提到的方法都没有解决我们的问题，所以我们想出了这个方法。这种方法可以进一步优化，所以把它作为解决这个问题的第一步。

这是一个两步的过程，它是 shell 脚本和 spark 代码的结合。首先我们需要生成记录文件(带有对象键)，然后运行 spark 代码在多个任务中跨节点并行复制文件

## 生成记录文件:

我们需要生成一个文本文件，包含源 s3 存储桶(将被复制)中的项目的对象键，这可以通过在任何 [EC2 实例](/javarevisited/top-5-aws-training-courses-to-crack-amazon-web-service-solutions-architect-associate-certification-3f4affa8f660?source=collection_home---4------0-----------------------)上运行这个命令来完成:

```
aws s3 ls s3://test_bucket --recursive | awk '{print $4}' > /tmp/output.txt
```

**输出:**(仅每行一个对象键)

data/solution = 33/test1 . mov
data/solution = 33/test2 . mov 等

## [火花码](/javarevisited/5-free-courses-to-learn-apache-spark-in-2020-bdff2d60c800):

```
sql.read()
 .textFile(file)
 .repartition(2000)
 .flatMap((FlatMapFunction<String, String>) s -> Arrays.asList(s.split("\n")).iterator(), Encoders.STRING())
 .map((MapFunction<String, String>) s -> String.format("aws s3 cp %s s3://%s/%s", String.format("s3://%s/%s", source, s), target, s), Encoders.STRING())
 .foreachPartition((ForeachPartitionFunction<String>) iterator -> {
       while (iterator.hasNext())
         Runtime.getRuntime().exec(iterator.next()).waitFor();
 });
```

**星火提交:**

spark-submit-class com . S3 . S3 copy S3://test _ bucket/copier . jar test _ bucket back _ up _ bucket S3://test _ bucket/output . txt

args[0] →源桶

args[1] →目标存储桶

args[3] →上一步中生成的 s3 记录文件

这段代码将读取“output.txt”文件，并将其分成多个分区，然后跨多个节点并行运行它们。

# 特性试验

使用 15 个 EMR 核心节点，每个 m4.xlarge 实例类型，我们能够在不到 **40 分钟**的时间内复制 **5.5 TB** 的数据。因为我们只为使用 EMR 的时间付费，所以它具有成本效益(通过 SPOT 或 EC2 车队配置可以进一步降低成本),并且与之前的方法相比具有更大的可扩展性。

# Spark 提交:

spark-submit—conf spark . network . time out = 420000s—conf spark . executor . heartbeat interval = 410000s—conf spark . yarn . scheduler . mode = FAIR—conf spark . shuffle . service . enabled = true—conf spark . serializer = org . Apache . spark . serializer . kryoserializer—conf spark . executor . memoryoverhead = 1024—conf spark . driver . memoryoverhead = 1024—conf spark . executor . instances = 74—conf spark .