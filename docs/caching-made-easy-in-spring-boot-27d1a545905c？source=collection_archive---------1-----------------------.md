# Spring Boot 让缓存变得简单

> 原文：<https://medium.com/javarevisited/caching-made-easy-in-spring-boot-27d1a545905c?source=collection_archive---------1----------------------->

![](img/87a3e629f33578621bc17443f7580597.png)

阿卡什·卡帕拉维尼的照片

缓存是从多方面改善系统的常见做法。它有助于使您的系统具有弹性、可伸缩性、快速，甚至根据您的用例节省一些钱。

如果数据库中的一些值经常被读取，那么缓存它们是个好主意。这可能是您发送给新注册用户的电子邮件的内容、每天都在变化的引人注目的横幅文本，或者是您禁止用户参与可疑活动的小时数。

如果这些被频繁访问，您可以将它们存储在应用程序属性中，甚至存储在您的代码中，但是之后您必须重新部署以使更改生效。这些是最有可能由业务团队设置的事情类型，所以如果他们决定改变某些东西，部署应用程序是一种矫枉过正的行为。所以你最好给他们一个门户来更新它们，在你的应用程序中，你把它们存储在数据库和缓存中，比如 redis。

好了，说够了，让我们看看代码。假设我们的应用程序中有以下实体:

只是简单的键值存储来存储通用配置。但事实是，我们的应用程序非常受欢迎，每秒钟被访问一千次。由于每秒创建 1000 个数据库连接有点耗费资源，或者我们的数据库可能是云中的 nosql，需要为每个读、写操作付费……所以我们决定缓存它。

我们的计划是，每当我们试图从数据库中读取时，首先检查缓存。如果在缓存中找到，就从缓存中读取，而不是访问数据库。如果没有找到，那么从数据库中读取并填充缓存。

现在让我们看看**的 KeyValuePairServiceImpl** 可能是什么样子:

在这个方法 **checkInCache** 中，我们首先在 redis 中查找这个键，如果没有找到，那么从 db 中读取并用这个方法 **getFromDb** 更新缓存。

在**保存**方法中，我们保存在数据库中，同时更新缓存。

是的，我明白了，与 redis 相关的逻辑可以被分离到不同的服务或存储库中，这可能会使这个文件看起来不那么难看。但你还是要在某个地方处理这件事，对吗？

嗯，实际上你不必！关于 spring 的事情是，它有如此多的常见用例，以至于在你了解它之后，你会觉得它是如此的明显，以至于你开始觉得有点愚蠢。至少当我第一次在 Spring 中发现**缓存抽象的时候，我是这样认为的。**

相信我，非常简单直观。

让我们重写**KeyValuePairServiceImpl**，这样就不用担心缓存什么的了。只有方法和存储库。

看到了吗？我们完全去除了与 redis 的交互，而不仅仅是重构。完全移除。这里的每一个方法应用程序都将直接与数据库交互。

现在，如果我们想让 **findByKey** 方法可缓存，我们只需要在它上面添加一个注释。这是什么？……有什么猜测吗？？

是的，字面意思是 **@Cachable** ，里面有一些参数。

```
@Override
@Cacheable(value="myAwesomeCache",key="#key")
public KeyValuePair findByKey(String key) {

        return keyValuePairRepository.findByKey(key);

}
```

在这里， **myAwesomeCache** 只是一个任意的名字，不强制但建议使用。因为多个应用程序使用同一个 redis 实例是很正常的。可能会出现这样的情况，多个应用程序用同一个键缓存某些东西！该值参数有助于防止这种情况。

**#key** 这里很重要。它将对应于我们希望在缓存期间用作键的带注释的方法参数。如果方法看起来像这样:

公钥值对 findByKey(字符串 myKey)

@Cacheble 批注的 key 参数的值需要是 **#myKey** 。

还剩下一点点配置。在您的应用程序的 RedisConfig 中，您必须添加另一个名为**RedisCacheConfiguration**的 **Bean** 。这里是我的 Redis 配置看起来像:

让我们看一些行动。为了测试，我很快添加了一个与服务交互的控制器类。使用它，我们将输入一些数据并尝试检索它。

首先让我们用下面的 cURL 创建一个问候条目。

```
curl -X POST "http://localhost:8080/" -H  "accept: */*" -H  "Content-Type: application/json" -d "{\"key\":\"greetings\",\"value\":\"Hello people of the world!\"}"
```

在检索它之前，让我们看看我的 redis 是什么样子的:

```
127.0.0.1:6379> KEYS *;127.0.0.1:6379>
```

是啊，空的。

我还使用了 [postgres](/javarevisited/7-best-free-postgresql-courses-for-beginners-to-learn-in-2021-3bf369d73794) 作为我的 db，并从应用程序属性中打开了 sql 日志记录。因此，每当数据库命中发生时，我们将能够在控制台中看到它。

现在让我们试着检索问候。

```
➜  curl -X GET "http://localhost:8080/greetings" -H  "accept: */*"{"id":4,"key":"greetings","value":"Hello people of the world!"}
```

因此，它返回了我们插入的问候语，并在控制台中找到了以下日志:

```
Hibernate: select keyvaluepa0_.id as id1_0_, keyvaluepa0_.key as key2_0_, keyvaluepa0_.value as value3_0_ from key_value_pair keyvaluepa0_ where keyvaluepa0_.key=?
```

让我们再次尝试检索相同的条目。但是..这次控制台中没有日志，也就是数据库中没有命中！

如果我们看一下 redis 中可用的键，我们会看到:

```
127.0.0.1:6379> KEYS *;127.0.0.1:6379> 1) "myAwesomeCache::greetings"
```

好的，Redis 中有一个条目，关键字是**问候语**。这意味着在查询数据库之前，应用程序会查看缓存并做出响应。现在，如果您从 redis 中删除，它将再次从数据库中提取。

现在，假设您必须更新问候语。你可以很容易地用控制器做到这一点，它将更新数据库。但是旧值仍将在缓存中。为了解决这个问题，我们需要像这样注释 save 方法:

```
@Override
@CacheEvict(value = "myAwesomeCache", key = "#keyValuePair.key")
public void save(KeyValuePair keyValuePair) {
    keyValuePairRepository.save(keyValuePair);}
```

它要做的是，在保存到[数据库](/javarevisited/7-free-courses-to-learn-database-and-sql-for-programmers-and-data-scientist-e7ae19514ed2)之前，它将从 redis 中删除与 out 参数 keyValuePair 的 key 属性具有相同键的条目。因此，下次检索时，新值将被填充到缓存中。

可能会出现这样一种情况，您不想缓存所有内容。例如，在这种情况下，我们可以使用 KeyValuePair 来存储许多东西，并且只想缓存包含满足所需条件的键的对象。比如以“frequent _”字符串开头。这是很有可能的:

```
@Cacheable(value="myAwesomeCache",key="#key", condition = "#key.startsWith('frequent_')")
```

今天就到这里，我相信我已经表达了我想要分享的想法。如果您有任何反馈，请随时提供。

这个演示的所有代码都可以在这里下载。

注意安全，戴口罩！

快乐缓存！