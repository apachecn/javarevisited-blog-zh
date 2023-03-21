# 模仿微博社交平台系统设计[2] —使用 Redis 的 hash 数据结构实现类帖子功能

> 原文：<https://medium.com/javarevisited/imitation-of-weibo-social-platform-system-design-2-use-the-hash-data-structure-of-redis-to-7d687c4a158b?source=collection_archive---------0----------------------->

## 使用 [Redis](https://javarevisited.blogspot.com/2022/02/top-5-courses-to-learn-redis.html) 的哈希数据结构实现 post-like 函数

![](img/e6383ce3c5d5dac1fcd7559e1998b4c6.png)

**背景:**

Redis 基本数据结构

五种数据结构

这五种数据结构分别是 STRING(字符串)、LIST(列表)、SET(集合)、HASH(哈希)、ZSET(有序集)；

*   [字符串](/javarevisited/top-21-string-programming-interview-questions-for-beginners-and-experienced-developers-56037048de45?source=collection_home---4------0-----------------------):包括字符串、整数、浮点数；
*   List:一个链表，链表上的每个节点都是一个字符串，遵循队列的访问格式——先进先出，即从链表的末尾插入，弹出[链表的头](/javarevisited/top-20-linked-list-coding-problems-from-technical-interviews-90b64d2df093)；
*   集合:它内部是一个容器，它不允许相同的元素存在，每个值都是唯一的；
*   Hash:是由[键值对](https://javarevisited.blogspot.com/2020/09/10-examples-of-concurrenthashmap-in-java.html)组成的无序哈希表，其键也不允许重复；
*   有序集合:以集合为基础进行排序；

**Redis Hset 命令**

语法

Redis Hset 命令的基本语法如下:

```
redis 127.0.0.1:6379> HSET KEY_NAME FIELD VALUE
```

例子

```
instance
redis 127.0.0.1:6379> HSET myhash field1 "foo"
OK
redis 127.0.0.1:6379> HGET myhash field1
"foo"

redis 127.0.0.1:6379> HSET website google "[www.g.cn](http://www.g.cn)" #set a new domain
(integer) 1

redis 127.0.0.1:6379>HSET website google "[www.google.com](http://www.google.com)" # cover an old domin
```

相似表的结构设计

redisTemplate 的相关 API:

Java 实现

为什么使用 stringredistemplate？