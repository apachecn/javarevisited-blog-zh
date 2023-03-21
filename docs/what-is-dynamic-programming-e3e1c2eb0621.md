# 什么是动态编程？

> 原文：<https://medium.com/javarevisited/what-is-dynamic-programming-e3e1c2eb0621?source=collection_archive---------3----------------------->

## 问题解决—优化问题

## 我们需要知道的关于动态编程的一切

嗨，伙计们👋,

![](img/a9f2ea27528a4933a577076e20c318eb.png)

泰勒·尼克斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在这一集里，我们将学习[动态编程](/javarevisited/6-best-dynamic-programming-courses-for-coding-interviews-14744060923c)，一种解决计算机科学中一些数学难题的方法。动态规划是解决问题中广泛使用的方法之一。在进入动态编程部分之前，我们需要理解一些基本的东西。

**你了解优化问题吗？**

*   从所有可能的选项中寻找最优解的任务被称为优化问题。
*   比方说，让我们假设，你需要从 Moratuwa 到 Petta。你的目标是找到一个划算的解决方案。这两个地点之间有两条路线，如加勒路线公共汽车和莫拉图瓦路线公共汽车。但是加勒路线的巴士比莫拉图瓦路线贵。两者都是可行的方案。然而，基于我们的目标，Moratuwa 路线是一个最佳解决方案。这就是优化问题。

**你了解解题中的递归吗？**

*   顾名思义，[递归](https://javarevisited.blogspot.com/2021/11/top-5-courses-to-learn-recursion-for.html)就是通过反复递归解决小问题来解决大问题。
*   也可以用来解决优化问题。

[动态规划](https://javarevisited.blogspot.com/2021/03/top-dynamic-programming-problems-for-coding-interviews.html)也是一种用于优化问题的算法。对于优化问题还有其他方法，例如贪婪方法、遗传算法等。不再拖延，让我们进入这一集的讨论。

[![](img/70e2134ceda8f48ebfd4381fdbc1ecf4.png)](https://javarevisited.blogspot.com/2021/03/top-dynamic-programming-problems-for-coding-interviews.html)

由 [Max Harlynking](https://unsplash.com/es/@harlynkingm?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**动态规划(DP)** 是一种算法技术，通过将优化问题分解为更小的子问题，并利用整体问题的最佳解决方案由其子问题的最佳解决方案决定的事实，来解决优化问题。当我告诉你这个的时候，你的大脑会突然想到“哦哦等等，它是 T2 递归对吗😳?"但答案不是。让我们看看两者有何不同。

当一个函数可能被调用并自己执行时，就会出现递归，而动态编程是通过将大问题分解成小问题来解决大问题的过程。[递归](https://www.java67.com/2021/07/recursion-programming-exercises-in-java.html)是一个函数可以被调用并自己执行，而动态编程是通过将问题分解成子问题来解决困难问题的过程。动态编程可以不使用递归技术。我已经通过下面的[斐波那契数列例子](https://javarevisited.blogspot.com/2020/05/fibonacci-series-in-java-8-with.html)阐明了递归和动态编程的区别。

```
**# Fibonacci** **Series by Resursive Approach** def fib(n): 
    if (n == 0):
      return 0 
    if (n == 1):
      return 1 
    return fibo(n-1) + fibo(n-2)**# Fibonacci** **Series by DP Approach** def fib(n):
    return fiboRec(n, new int[n+1])def fiboRec(n,memo):
    if (n == 0)
       return 0
    if (n == 1): 
       return 1
    if (memo[n] == 0): 
       memo[n] = fibRec(n-1, memo) + fibRec(n-2, memo)
    return memo[n]
```

让我们去找菲波(5)。

> **在递归方法中**

我会相应地显示这些电话

*   纤维(5) = >纤维(4)，纤维(3)
*   纤维(4) = >纤维(3)，纤维(2)
*   纤维(3) = >纤维(2)，纤维(1)
*   纤维(3) = >纤维(2)，纤维(1)
*   fib(2) => fib(1)，fib(0)
*   fib(2) => fib(1)，fib(0)
*   纤维(1) => 1
*   fib(2) => fib(1)，fib(0)
*   纤维(1) => 1
*   纤维(1) => 1
*   fib(0) => 1
*   纤维(1) => 1
*   fib(0) => 1
*   纤维(1) => 1
*   fib(0) => 1

这里会有很多递归调用。你可以看到 fib(3)和 fib(2)被反复调用。这也是反复计算的。这样做是无效的。

> **在动力定位方法中**

*   纤维(5) = >纤维(4)，纤维(3)
*   纤维(4) = >纤维(3)，纤维(2)
*   纤维(3) = >纤维(2)，纤维(1)
*   fib(2) => fib(1)，fib(0)
*   纤维(1) => 1
*   fib(0) => 1

只有这样的递归计算才会发生，因为数组包含了的斐波纳契数列值，此时这些值小于 5。对于[斐波纳契数列](https://www.java67.com/2019/03/nth-fibonacci-number-in-java-coding.html)来说，这是一种比递归方法更有效的方法。

让我们先看看问题的特征，这些特征告诉我们可以使用 DP 来解决问题，然后再继续了解解决 DP 挑战的替代方法。

*   **重叠子问题**:子问题的空间必须有限；也就是说，必须反复面对同样的子问题。
*   **最优子结构**:问题的最优解包括子问题的最优解，这意味着总体最优解可以由其子问题的最优解构成。

## 使用动态编程解决问题

*   如果一个问题同时具有上述两个性质，那么它可以通过动态规划来解决。动态编程解决方案可以通过两种方式实现。

![](img/970ffdde0de65eeca5330f91ff8d1f6d.png)

照片由[普里西拉·杜·普里兹](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

1.  **自上而下的记忆**
2.  **自下而上的制表方法**

## 自上而下的记忆

在这种方法中，我们试图通过递归地解决较小的子问题来解决较大的问题。我们缓存我们解决的每个子问题的结果，这样如果它被调用了几次，我们就不必再去解决它了。我们可以简单地返回保存的结果。记忆是一种保存以前解决的子问题的结果的方法。

## 自下而上的制表方法

与自顶向下技术相反，制表避免了重复。我们用这种方法“自下而上”地解决问题(即通过对问题大小排序，首先解决所有相关的子问题)。这通常通过填充一个 n 维表来完成。然后根据表格的结果计算出顶层/原始问题的答案。

下面的视频将有助于进一步理解动态编程中自顶向下和自底向上的方法。

出于理解的目的，许多问题通过动态规划来解决，例如 [0/1 背包问题](https://javarevisited.blogspot.com/2021/03/top-dynamic-programming-problems-for-coding-interviews.html)，矩阵链乘法问题，杆切割问题等。你可以很容易地在互联网上找到这些解决方案。这些问题的解决方案将有助于非常清楚地阐明动态编程背后的概念。

**贪婪的方法**

贪婪方法也被用于解决优化问题。但是在某些情况下，它可能比动态编程次优。贪婪方法用于必须做出一系列决策以达到最优解的情况。它在给定时间做出最佳决策，并选择局部最优的选项，以期获得全局最优的结果。然而，它并不总是提供最佳解决方案。但是在诸如最小生成树、图着色、最短路径问题等各种问题上非常有效。即使动态规划解决了 0/1 背包问题(二进制背包问题，这意味着你不允许将任何项目作为分数)，贪婪方法也可以很容易地用于分数背包问题。

![](img/9cfc3044243ed81b431b03a8c72ea377.png)

拉奎尔·马丁内斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我相信你学到了一些关于[动态编程](https://javarevisited.blogspot.com/2020/04/5-essential-skills-to-crack-coding-interviews.html)的非常关键的要点，以便在现实世界的应用中使用。如果您有任何问题或任何澄清，不要犹豫，通过回复部分与我联系。感谢您花费宝贵的时间阅读这篇博客，我相信这将激励您继续阅读我的其他博客 [***这里***](https://sthenusan.medium.com/) *。*

*喜欢这篇文章吗？成为* [***中等会员***](https://sthenusan.medium.com/membership) *继续学习没有任何限制。如果你使用上面的链接，我会收到你的一部分会员费，不需要你额外付费。提前感谢。*