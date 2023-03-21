# 计算给定数字的阶乘中尾随零的个数。

> 原文：<https://medium.com/javarevisited/count-the-number-of-trailing-zeros-in-the-factorial-of-a-given-number-953f569d4c9d?source=collection_archive---------1----------------------->

[![](img/3c77e4d7a4754f2d43a85164b7ed9297.png)](https://www.java67.com/2015/09/factorial-in-java-using-recursion-and-loop.html)

# **问题陈述**

给定一个数，找出它的阶乘的尾随零的个数。

# **例子:**

```
**Input:** n = 5**Output:** 1Factorial of 5 is 120 which has one trailing 0.**Input:** n = 20**Output:** 4Factorial of 20 is 93326215443944152681699238856266700490715968264381621468592963895217599993229915608941463976156518286253697920827223758251185210916864000000000000000000000000 which has 4 trailing zeroes.**Input:** n = 100**Output:** 24Factorial of 20 is 93326215443944152681699238856266700490715968264381621468592963895217599993229915608941463976156518286253697920827223758251185210916864000000000000000000000000 which has 24 trailing zeroes.
```

在这篇博客中，我们将讨论两种方法来计算这些尾随零的数量。

# **蛮力逼近**

强力方法是我们想到的第一种方法，在我们的例子中，我们想到的第一种方法是首先[找到给定数字](https://javarevisited.blogspot.com/2012/04/java-program-to-find-factorial-of.html)的阶乘，然后计算尾随零的数量。

正如我们已经知道的，简单的迭代和递归方法对于[寻找 100](https://javarevisited.blogspot.com/2015/08/how-to-calculate-large-factorials-using-BigInteger-Java-Example.html) 这样的大数的阶乘没有用，所以我们将从一开始就使用结转方法，这样我们就可以计算每个正数的尾随零。

# **算法**

1.  用进位法求给定数字的阶乘。
2.  将计数变量初始化为 0。
3.  使用 reverse for loop 来检查数字是否为 0，方法是取 10 的模。
4.  如果数字为零，则将计数加 1。
5.  打印该计数。

下面是上述方法的实现

# **输出**

尾随零的数量— 24

**时间复杂度** : O(N log N！)

**辅助空间:** O(n)

# **高效方法**

我们知道，一个数的阶乘是该数与小于该数且大于 0 的其他自然数的乘积，在大多数情况下，该数有一些尾随零。这背后的原因是数字 10 或它的因子 2 和 5 的存在。

如果我们对阶乘进行[质因数分解](http://javarevisited.blogspot.sg/2015/03/how-to-find-largest-prime-factor-of.html)，我们将得到 2 和 5 的计数，我们可以用它来找到尾随零的数量。

我们举个例子来了解一下

```
**Input:** n = 5**Prime Factors —** 2x2x2x3x5**Output:** 1 — we have only 1 factor of 5Factorial of 5 is 120 which has only 1 trailing zero.**Input:** n = 11**Prime Factors —** 28x34x52x7**Output:** 2–2 factors of 5Factorial of 20 is 39916800 which has 2 trailing zeroes.
```

从上面的例子中可以清楚地看到，在一个数的[阶乘中找到因子 5 将会给出这个数的阶乘中尾随零的确切数目。下面是我们必须编码以找到尾随零的数量的步骤。](https://www.java67.com/2015/09/how-to-use-biginteger-class-in-java.html)

# **算法**

1.  将计数变量初始化为 0
2.  使用 for 循环将数字除以 5 的幂，如 5、25、125 等
3.  如果除法的结果大于 1，则按该数增加计数值。
4.  打印或返回计数，因为这将是我们的答案或尾随零的数量

这种方法的美妙之处在于，我们不必先找到数字的阶乘来计算尾随零。

# **输出**

尾随零的数量是 24

**时间复杂度** : O(log n)

**辅助空格** : O(1)

很明显，就时间和空间复杂度而言，第二种方法在寻找尾随零的数量方面取得了明显的胜利。其中强力方法需要 N log N 的时间和 O(n)的空间复杂度，另一种方法仅需要 log N 的时间和线性空间来寻找给定数字的阶乘的尾随零的数量。

你也可以看看我的其他项目

1.  [计算给定数字的阶乘](https://tekolio.com/how-to-calculate-the-factorial-of-a-given-number/)
2.  [合并两个排序后的数组](https://tekolio.com/how-to-merge-two-sorted-arrays/)
3.  [如何旋转数组](https://tekolio.com/how-to-rotate-an-array-in-java/)