# 如何在 Java 中使用 Open CSV 读取 CSV 文件

> 原文：<https://medium.com/javarevisited/how-to-read-csv-file-using-open-csv-in-java-6796db168870?source=collection_archive---------1----------------------->

> 最初发表于[https://asyncq.com](https://asyncq.com/how-to-read-csv-file-using-open-csv-in-java)

## 介绍

CSV 文件是在服务器之间存储、交换结构化数据的常用方式之一，也是另一种流行的结构化数据格式。有很多库可以读取 CSV 文件，其中一个流行的库是 [OpenCSV](https://javarevisited.blogspot.com/2015/06/2-ways-to-parse-csv-files-in-java-example.html) 。在本文中，我们将使用 OpenCSV 库通过 Java 读取 CSV 文件。

## 输入文件

我使用来自 [kaggle](https://www.kaggle.com/datasets/prasertk/netflix-daily-top-10-in-us) 的《网飞日报》十大数据集作为我的输入文件。