# 代码味道 147 —方法太多

> 原文：<https://medium.com/javarevisited/code-smell-147-too-many-methods-5da19bc3427b?source=collection_archive---------2----------------------->

## *Util 类非常适合收集协议*

[![](img/64efd626deb50908855bc27bf80c0e9a.png)](https://javarevisited.blogspot.com/2017/10/clean-code-by-uncle-bob-book-review.html)

> *TL；博士:不要在你的课上添加一个偶然的协议*

# 问题

*   可读性
*   违反单一责任
*   凝聚力差
*   高耦合
*   低重用性

# 解决方法

1.  打破你的阶级
2.  [提取类](/javarevisited/refactoring-007-extract-class-5b0b5960d0e7)

# 相关重构

[](/javarevisited/refactoring-007-extract-class-5b0b5960d0e7) [## 重构 007 —提取类

### 行为在整个系统中重复。但是我们忽略了一个概念

medium.com](/javarevisited/refactoring-007-extract-class-5b0b5960d0e7) 

# 语境

我们倾向于在找到的第一个类中放置一个协议。

这不是问题。

我们只需要[重构](/javarevisited/7-best-courses-to-learn-refactoring-and-clean-coding-in-java-47bea3c67006)。

# 示例代码

## 错误的

```
public class MyHelperClass {
  public void print() { }
  public void format() { }
  // ... many methods more // ... even more methods 
  public void persist() { }
  public void solveFermiParadox() { }      
}
```

## 对吧

```
public class Printer {
  public void print() { }
}public class DateToStringFormater {
  public void format() { }
}public class Database {
  public void persist() { }
}public class RadioTelescope {
  public void solveFermiParadox() { }
}
```

# 侦查

[X]自动

大部分棉短绒算方法，警示我们。

# 关系

[](https://mcsee.medium.com/code-smell-124-divergent-change-48825bcc74d0) [## 代码气味 124 —不同的变化

### 你在课堂上改变了一些东西。你在同一个类中改变一些不相关的东西

mcsee.medium.com](https://mcsee.medium.com/code-smell-124-divergent-change-48825bcc74d0) [](https://levelup.gitconnected.com/code-smell-143-data-clumps-86cfe2f36a19) [## 代码气味 143 —数据块

### 有些物体总是在一起。我们为什么要分开他们？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-143-data-clumps-86cfe2f36a19) [](https://blog.devgenius.io/code-smell-94-too-many-imports-e8f44986db2) [## 代码气味 94 —太多导入

### 如果你的类依赖别人太多，就会耦合，脆弱。长长的进口清单是一个很好的指标。

blog.devgenius.io](https://blog.devgenius.io/code-smell-94-too-many-imports-e8f44986db2) [](https://blog.devgenius.io/code-smell-22-helpers-1d5057137ddb) [## 代码气味 22 —助手

### 你需要帮助吗？你要给谁打电话？

blog.devgenius.io](https://blog.devgenius.io/code-smell-22-helpers-1d5057137ddb) [](https://blog.devgenius.io/code-smell-34-too-many-attributes-2df68c7db040) [## 代码味道 34 —属性太多

### 一个类定义了具有许多属性的对象。

blog.devgenius.io](https://blog.devgenius.io/code-smell-34-too-many-attributes-2df68c7db040) 

# 更多信息

[重构大师](https://refactoring.guru/smells/large-class)

# 标签

*   内聚力
*   膨胀者

# 结论

分离类和协议是一个有利于小的和可重用的对象的好习惯。

# 信用

由[马尔钦·西蒙尼德斯](https://unsplash.com/@cinusek)在 [Unsplash](https://unsplash.com/s/photos/full) 上拍摄的照片

没有任何代码如此庞大、扭曲或复杂，以至于维护不会使它变得更糟。

杰拉尔德·温伯格

[](https://blog.devgenius.io/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](https://blog.devgenius.io/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://blog.devgenius.io/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](https://blog.devgenius.io/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)