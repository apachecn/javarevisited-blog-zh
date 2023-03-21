# 利用多线程并行计数排序并保持稳定性

> 原文：<https://medium.com/javarevisited/paralleling-counting-sort-using-multi-threads-and-maintaining-the-stability-3a947e8a3475?source=collection_archive---------3----------------------->

假设你有一个数组 ***arr[0，1，..【T2 n-1】***进行排序，要用 ***p*** 数量的线程来这样做。

为了简单起见，我们假设最小元素的值为 ***0*** ，最大元素的值为 ***m*** 。如果最小值不等于 ***0*** ，那么在将元素分配给线程时可以缩放这些值。

将你的数组分成 ***p*** 个大块，每个大块最多有 ***层(m/p) + 1*** 个不同的元素值。