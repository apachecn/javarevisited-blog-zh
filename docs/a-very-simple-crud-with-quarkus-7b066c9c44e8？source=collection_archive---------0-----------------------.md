# 一个非常简单的带有夸库的 CRUD

> 原文：<https://medium.com/javarevisited/a-very-simple-crud-with-quarkus-7b066c9c44e8?source=collection_archive---------0----------------------->

如何用 Quarkus，Panache 和 H2 实现一个简单的 CRUD

嗨伙计们！今天我想和你分享一个使用 Quarkus 为数据库实体实现 CRUD 服务的简单方法。如果你错过了，你可能想读一下我之前关于 Quarkus 的文章来设置这个项目。为了实现 CRUD 服务，我们将遵循几个简单的步骤来设置依赖关系、实现实体并创建通过 REST 公开的服务。对于数据库部分，我将使用一个 H2 内存数据库，只是为了验证概念。

# 项目的设置