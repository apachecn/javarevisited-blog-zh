# 一天内将代码库迁移到基于注释的 spring 事务

> 原文：<https://medium.com/javarevisited/migrating-code-base-to-annotation-based-spring-transaction-in-1-day-5f55fb68198e?source=collection_archive---------1----------------------->

# 为什么我们需要搬家？

*   我们有很多样板代码，比如“beginTransaction()”、“commit()”和“rollback()”，考虑到这个领域的进步，我觉得这些代码已经过时了，不再相关，所以少代码或没有代码总是好代码。
*   在代码中有多个路径，我们开始注意到 prod 中的错误，如“事务不活动”，“无法提交已经关闭的事务”，因为用户操作将失败。的…