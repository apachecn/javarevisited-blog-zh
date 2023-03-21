# Mysql 中的每个表应该不超过 2000 万条数据，这是正确的吗

> 原文：<https://medium.com/javarevisited/is-it-correct-that-each-table-in-mysql-should-have-no-more-than-20-million-pieces-of-data-5beb20a1a0f2?source=collection_archive---------4----------------------->

![](img/94e9fa1823c6c44235aaecdd11d13f47.png)

Emile Perron 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

一般来说， [MySQL](/javarevisited/top-5-courses-to-learn-mysql-in-2020-4ffada70656f) 中每个表的数据量最好不要超过 2000 万条，否则会导致性能下降。**阿里巴巴的 Java 开发手册**也规定，只有当单个表的行数超过 500 万行或单个表的容量时，才推荐子数据库子表…