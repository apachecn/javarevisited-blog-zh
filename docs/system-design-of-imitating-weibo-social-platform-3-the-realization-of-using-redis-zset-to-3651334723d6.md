# 模仿微博社交平台的系统设计[3]——利用 redis zset 存储好友关系的实现

> 原文：<https://medium.com/javarevisited/system-design-of-imitating-weibo-social-platform-3-the-realization-of-using-redis-zset-to-3651334723d6?source=collection_archive---------1----------------------->

## 利用 redis zset 存储朋友关系的实现

![](img/a7a47535a593fcaf843c831f5b7d7e08.png)

**Redis 有序集(sorted set)**

[Redis](https://javarevisited.blogspot.com/2022/02/top-5-courses-to-learn-redis.html) 有序集合和集合一样，也是 string 类型元素的集合，不允许有重复成员。

不同之处在于，每个元素都与一个 double 类型的分数相关联。Redis 通过分数将集合成员从小到大排序。

有序集的成员是唯一的，但是分数可以重复。

例子

```
redis 127.0.0.1:6379> ZADD runoobkey 1 redis
(integer) 1
redis 127.0.0.1:6379> ZADD runoobkey 2 mongodb
(integer) 1
redis 127.0.0.1:6379> ZADD runoobkey 3 mysql
(integer) 1
```

Redis 相关命令:

[扎德关键得分 1 成员 1【得分 2 成员 2】](https://www.runoob.com/redis/sorted-sets-zadd.html)

实际的 api 与命令分数和成员位置相反

redisTemplate 的相关 API:

```
redisTemplate.opsForZSet().add(K key, V value, double score)
```

具体 java 代码的实现:

使用 redis 的 zset，设置额外的得分时间

用 zset 会更好，各种操作的时间复杂度，占用 CPU 都比 list 好。

使用 rang 方法获取被跟踪的所有人的 uid 集合

如果缓存已经失效，从[数据库](/javarevisited/top-5-sql-and-database-courses-to-learn-online-48424533ac61)查询，并从新设置 redis，并设置失效时间！

<https://javarevisited.blogspot.com/2022/06/best-system-design-and-analysis-books.html>  <https://www.java67.com/2019/09/top-5-courses-to-learn-system-design.html>  <https://javarevisited.blogspot.com/2022/03/how-to-prepare-for-system-design.html> 