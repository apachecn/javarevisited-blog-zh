# 如何洗牌

> 原文：<https://medium.com/javarevisited/how-to-shuffle-a-deck-of-cards-d6e60ed234ac?source=collection_archive---------0----------------------->

洗牌对任何玩过纸牌游戏的人来说听起来都很平常。这可能是人们坐下来打牌时做的第一件事。我们为什么要洗牌？—这样，在我们开始玩之前，牌组中的任何现有图案都应该消失！这听起来可能微不足道，但对于像赌场这样人们玩游戏并涉及金钱的地方，随机洗牌*是极其重要的。*

*![](img/280539133a1b61ac3476274740e009fa.png)*

*照片由[阿莫尔·泰亚吉](https://unsplash.com/@amoltyagi2?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄*

# *什么是随机洗牌？*

*简单来说，随机洗牌就是 ***所有可能的排序都是同等可能性*** 的洗牌。如果不是，人们可以利用一些组合比其他组合更可能出现的事实。这可能导致**在游戏中作弊，有足够经验的人知道某些排序或排列可以给他们更好的获胜机会**！你可以阅读 [***在线扑克***](https://www.developer.com/guides/how-we-learned-to-cheat-at-online-poker-a-study-in-software-security/) 的一个例子，他们描述了洗牌算法如何不产生随机结果！*

# *洗牌的正确方法*

*让我们假设我们有一个包含整数的大小为 N 的数组 A，而不是一副卡片。对于一副牌，N 应该是 52，但是让我们考虑一个一般的情况。*

# ***克努特洗牌***

*使用的技术被称为“Knuth 洗牌”。它包括[迭代数组](https://www.java67.com/2018/12/how-to-remove-objects-or-elements-while-iterating-Arraylist-java.html),同时执行以下操作—*

1.  *对于数组中可以从 0 到 N-1 变化的任何索引 I(因为 N 是数组中元素的数量)，**我们在 I 和 N-1 之间挑选一个随机索引，即来自数组的不可见部分的随机索引。***
2.  *交换一个[randomIndexFromUnseenPart]和一个[i]。*

***这种算法用在不同的口味，也叫**[**‘费雪-耶茨洗牌’**](https://en.wikipedia.org/wiki/Fisher–Yates_shuffle)。*

## *履行*

*正如我一直做的那样，我将坚持用 [Java](/javarevisited/10-best-places-to-learn-java-online-for-free-ce5e713ab5b2) 展示实现。*

```
*public void shuffleRandom(int[] deck) { for(int i=0; i<N; i++) { /*
        Generate random number between i and N(exclusive).
        You can also use ***nextInt()*** method of [Random](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html) class
        in java, instead of Math.random()
     */
     int randomIndexFromUnseenPart = i + (int)((N-i)*Math.random());

     // Swap with deck[i]
     int temp = deck[i];
     deck[i] = deck[randomIndexFromUnseenPart];
     deck[randomIndexFromUnseenPart] = temp;
   }
}*
```

## *从看不见的部分计算随机指数*

*让我们看看上面的代码片段，其中有这样一行—*

```
*int randomIndexFromUnseenPart = i + (int)((N-i)*Math.random());*
```

*我们如何保证这不会导致数组索引问题？它会超过 N(索引越界)吗？！让我们来看一个快速证明—*

*Math.random()生成一个介于 0(含)和 1(不含)之间的数字*

*简而言之—*

```
 *0 <= Math.random() < 1*
```

***为简单起见，我们将 Math.random()的结果称为 k。***

*因此，*

```
*0 <= k < 1*
```

*现在，让我们**将上述不等式的所有方面乘以(N-i)** ，这给出—*

```
*0 <= (N-i)*k < N-i*
```

*接下来，让我们把 I 加到所有边上*

```
 *i <= i + (N-i)*k < N*
```

***看不等式的中项！！—不是和**一样吗*

```
*int randomIndexFromUnseenPart = i + (int)((N-i)*Math.random());*
```

*因此我们可以有把握地说*

```
*i <=  randomIndexFromUnseenPart < N*
```

***好啦！所以没有数组索引问题！***

*对于一副普通的纸牌，N=52。下一次有人让你高效地洗牌时，你知道如何在线性时间内完成— ***即 O(N)！！****