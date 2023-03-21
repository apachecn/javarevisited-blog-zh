# 关于 ConcurrentModificationException 集合实现的深入说明

> 原文：<https://medium.com/javarevisited/an-in-depth-note-about-concurrentmodificationexception-on-collection-implementations-8e69cb1c2b68?source=collection_archive---------2----------------------->

*关于这篇文章，我们正在讨论关于* `*ConcurrentModificationException*` *。我们正在讨论的领域有什么是* `*ConcurrentModificationException*` *？当此异常引发。如何解决* `*ConcurrentModificationException*` *？..etc*

![](img/7582896c7f6cf80cbbdfe007c2e7e412.png)

照片由 [Hannah Busing](https://unsplash.com/@hannahbusing?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

首先，让我们看看什么是真正的`ConcurrentModificationException`。