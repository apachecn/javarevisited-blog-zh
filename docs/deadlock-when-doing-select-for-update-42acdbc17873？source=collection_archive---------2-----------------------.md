# 选择更新时出现死锁

> 原文：<https://medium.com/javarevisited/deadlock-when-doing-select-for-update-42acdbc17873?source=collection_archive---------2----------------------->

## 选择更新时出现死锁

![](img/85ac62d7a0f2cc5538a15626ea67e91f.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Muhammad Zaqy Al Fattah](https://unsplash.com/@dizzydizz?utm_source=medium&utm_medium=referral) 拍摄

**背景**

该项目要求表中的单个列自动递增，但是在 where 条件下，并不是该列的所有值都符合自动递增

在我想使用 select for update [mysql](/javarevisited/top-5-courses-to-learn-mysql-in-2020-4ffada70656f) 锁定表之后，查询的最大值将会增加…