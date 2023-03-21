# 用 Java 动态编程实现矩阵链乘法

> 原文：<https://medium.com/javarevisited/implementation-of-matrix-chain-multiplication-using-dynamic-programming-in-java-e8828c1e9f9b?source=collection_archive---------5----------------------->

我们已经在上一篇文章中清楚地讨论了使用动态编程的[矩阵链乘法。在本文中，我们将使用 Java 来实现它。](https://himnickson.medium.com/matrix-chain-multiplication-using-dynamic-programming-a-brief-explanation-with-an-example-c565aa21ebec)

## 伪代码

```
// Matrix A[i] has dimension dims[i-1] x dims[i] for i = 1..n
MatrixChainOrder(int dims[])
{
    // length[dims] = n + 1
    n = dims.length - 1;
    // m[i,j] = Minimum number of scalar multiplications (i.e., cost)
    // needed to compute the matrix A[i]A[i+1]...A[j] = A[i..j]
    // The cost is…
```