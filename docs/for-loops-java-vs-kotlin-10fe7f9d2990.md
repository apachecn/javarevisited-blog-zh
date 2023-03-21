# For 循环:Java 与 Kotlin

> 原文：<https://medium.com/javarevisited/for-loops-java-vs-kotlin-10fe7f9d2990?source=collection_archive---------8----------------------->

Java 和 Kotlin 中一个非常快速的 for 循环变体！

![](img/2af731a35609fc029d5708d72feb3b68.png)

照片由[大卫·斯特雷特](https://unsplash.com/@daviidstreit?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/loops?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

循环是任何编程语言最基本的构件之一。不管您选择哪种语言，for 循环或它的变体肯定会存在。本着这种精神，今天我想对两种非常流行的 JVM 语言:Java 和 Kotlin 中的 for 循环进行一个比较。

## 直接访问元素

例如，让我们考虑一个需要迭代和打印的数字列表。

在 Java 中:

```
List<Integer> numbers = new ArrayList(){{
  add(3);
  add(6); 
  add(9); 
  add(12);
}};for(Integer number: numbers) {
  System.out.println(number);
}
```

Kotlin 版本的代码将是(删除类型 decleration 作为 for 循环的一部分):

```
val numbers = listOf(3, 6, 9, 12)for(number in numbers) {
  println(number)
}
```

输出:

```
3
6
9
12
```

## 使用索引访问元素

不得不访问数组或列表等集合中的元素可能是编程 101。

用古老的 Java 语言来说就是:

```
List<Integer> numbers = new ArrayList(){{
    add(3);
    add(6);
    add(9);
    add(12);
}};for(int index=0; index<numbers.size(); index++) {
    System.*out*.println(numbers.get(index));
}
```

Kotlin 支持我们可以用于索引的范围(注意，for 循环的两个边界都是包含的):

```
val numbers = listOf(3, 6, 9, 12)for(index in 0..numbers.size-1) {
  println(numbers[index])
}
```

输出:

```
3
6
9
12
```

## 反向使用索引访问元素

如果我们想以相反的顺序访问列表中的元素，我们将在 Java 中执行以下操作:

```
List<Integer> numbers = new ArrayList(){{
    add(3);
    add(6);
    add(9);
    add(12);
}};for(int index=numbers.size()-1; index>=0; index--) {
    System.*out*.println(numbers.get(index));
}
```

在科特林:

```
val numbers = listOf(3, 6, 9, 12)for(index in numbers.size-1 downTo 0) {
  println(numbers[index])
}
```

输出:

```
12
9
6
3
```

## 使用带有自定义增量的索引访问元素

最后，如果我们正在寻找一个非 1 的增量或减量(这里以增量 2 为例):

Java 版本:

```
List<Integer> numbers = new ArrayList(){{
  add(3);
  add(6); 
  add(9); 
  add(12);
}};for(int index=0; index<numbers.size(); index=index+2) {
  System.out.println(numbers.get(index));
}
```

科特林版本:

```
val numbers = listOf(3, 6, 9, 12)for(index in 0..numbers.size-1 step 2) {
  println(numbers[index])
}
```

输出:

```
3
9
```

就这样结束了！我希望这有助于在用 Java 和 Kotlin 编写循环之间流畅地切换！