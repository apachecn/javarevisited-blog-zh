# 计数并重新排列

> 原文：<https://medium.com/javarevisited/count-and-rearrange-2efb209330d3?source=collection_archive---------6----------------------->

![](img/5050a05f7ec2504e16bc2b86ca892385.png)

## 问题陈述:

万寿之露的任务是从一组已排序的数字中找出一个给定数字的计数，然后将这些数字推到数组的前面，以便于拾取和计数。

## 讨论的解决方法:

*   使用循环的暴力方法
*   使用[二分搜索法](https://en.wikipedia.org/wiki/Binary_search_algorithm)的优化方法

## 这个编码问题的关键是:

*   了解一下[二分搜索法](/javarevisited/binary-search-in-java-algorithm-eca288cb9bc2)
*   了解[交换数组中的元素](https://javarevisited.blogspot.com/2015/03/how-to-reverse-array-in-place-in-java.html)

```
For example:If the numbers are 1, 1, 2, 2, 2, 2, 6 and you are given number 2 to count and push it to the front then, the final output would look likeOutput: 2, 2, 2, 2, 1, 1, 6
```

**能做到时间复杂度为 O(logn)空间复杂度为 O(1)吗？只考虑寻找和计算数字**

现在，暂时忘记你被给予的找出解决方案的时间复杂性，现在，想一想找到问题的解决方案的最基本的方法。

让我们把问题分解成两个子问题:-
1。求给定数字
2 的计数。将给定的数字排列在所有其他数字的开头

![](img/db13df6df9006575497c2d59cc840c05.png)

让我们编码

## 天真的解决方案:

该问题的**强力**解决方案是使用 [**线性搜索**](https://javarevisited.blogspot.com/2020/01/how-to-implement-linear-or-sequential-search-in-java.html#axzz6VYKcmyZz) 来计算该数字的出现次数，然后将该数字重新排列到所有其他数字的前面。

我们来看看这个方法的算法:

```
start function searchElementandSwapp(array, array_size, number){
     count = 0
     initialIndex = 0 /* Get the count of the number and the index where it was first found */ start for (index is 0 to index is less than array_size increment index by 1)
          start if(array[index] is equal to number and count = 0)
                increment count by 1
                set initialIndex as index
          end if
          start if(array[index] is equal to number and count != 0)
                increment count by 1
          end if
     end for /* Now swapp the numbers */ start for (index is 0 to index is less than array_size and array[initialIndex] is equal to number increment  by 1)
        start if(array[initialIndex] is equal to number and initialIndex is less than array_size)
             swapp array[index] and array[initialIndex]
             increment initialIndex by 1
        end if
     end forend searchElementandSwapp
```

**该算法的执行时间为 O(n ),完成运算的空间为 O(1)**

这个算法对于大规模的数组肯定会失败，对于更大规模的数组效率很低，那么我们应该怎么想来进一步优化它呢？

你听说过一个[T21 二分搜索法](https://javarevisited.blogspot.com/2019/04/top-20-searching-and-sorting-algorithms-interview-questions.html)的算法吗？

这个问题满足了所有二分搜索法的使用规则，因为数组是完全排序的，我们可以在时间 **O(logn)** 内找到我们想要的元素！

## 二分搜索法方法:

让我们讨论首先找到元素在数组中出现的位置的算法:

```
start function searchElement(array, start_index, end_index, number) start if(end_index is less than 1)
           return -1
       end if mid_index = start_index + (end_index - start_index) / 2

       start if(array[mid_index] is equal to number)
            return mid_index
       end if start if(array[mid_index] is less than number)
            searchElement(array, mid_index + 1, end_index, number)
       end if start else
            searchElement(array, start_index, mid_index - 1, number)
       end elseend searchElement
```

现在，在搜索之后，我们将需要[计算一个给定数字在数组](/javarevisited/20-array-coding-problems-and-questions-from-programming-interviews-869b475b9121)中的出现次数，为此我们可以将算法写成

```
start function countElement(array, array_size, number) getIndex = searchElement(array, 0, array_size - 1, number) /* if the element is not present */ start if(getIndex is equal to -1)
         return 0
     end if

     count = 0
     leftCountIndex = getIndex
     rightCountIndex = getIndex
     initialIndex = 0 /* this will let us know the first position where the number was found to swapp it later on */ /* count left side */

     start while(leftCountIndex is greater than or equal to 0 and array[leftCountIndex] is equal to number)
           increment count by 1
           set initialIndex to leftCountIndex
           decrement leftCountIndex by 1
     end while /* count right side */ start while(rightCountIndex is less than the array_size and   array[rightCountIndex] is equal to number)
           increment count by 1
           incrementrightCountIndex by 1
     end while print count     return initialIndexend countElement
```

现在，我们有了 **initialIndex** ，所以我们可以很容易地用它来交换前面的数字**(如天真的交换方法所示，我们将使用相同的方法)**，然后嘣！你在预期的时间内解决了问题。

现在，既然你已经知道了问题背后的[算法](/javarevisited/20-algorithms-coding-problems-to-crack-you-next-technical-interviews-23191f229788)，那么你可以用你想要的编程语言编写你自己的功能性**代码**。

# 分析:

*   时间复杂度:`O(logn)`，其中 **n** 是给定数组的大小，这是因为我们使用了二分搜索法方法。
*   空间复杂度:`O(1)`，因为我们没有使用任何额外的空间来存储元素。

**不断学习，不断成长，不断探索！**

**万事如意！**

更多有趣和信息丰富的文章和提示，请关注我的 [**Medium**](https://swapnilkant11.medium.com/) **和**[**Linkedin**](https://www.linkedin.com/in/swapnil-kant-279a3b148/)