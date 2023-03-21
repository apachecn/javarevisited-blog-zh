# 你可能不知道的可选

> 原文：<https://medium.com/javarevisited/what-you-might-not-know-about-optional-7238e3c05f63?source=collection_archive---------2----------------------->

## How Optional 可以帮助你去除讨厌的空指针错误

![](img/518bf575af0feadb6b3f494c654b7033.png)

照片由来自 [Pexels](https://www.pexels.com/photo/black-bearded-man-typing-on-laptop-at-home-6578421/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Andres Ayrton](https://www.pexels.com/@andres-ayrton?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

```
· [What’s the correct usage of ofNullable?](#8607)
· [Don’t return null, use Optional](#60f9)
· [Why is Optional#of necessary?](#4748)
· [Why Optional#of throws NullPointerException?](#127e)
· [You shouldn’t use Optional for method arguments](#607e)
```

# ofNullable 的正确用法是什么？