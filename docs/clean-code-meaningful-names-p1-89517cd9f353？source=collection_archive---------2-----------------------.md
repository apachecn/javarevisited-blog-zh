# 干净代码—有意义的名称(p1)

> 原文：<https://medium.com/javarevisited/clean-code-meaningful-names-p1-89517cd9f353?source=collection_archive---------2----------------------->

现在我正在第二次阅读由*罗伯特·c·马丁*又名*鲍勃叔叔*写的伟大的书**干净的代码**，我认为写下它试图传达给我们的最重要的想法是个好主意。今天，我将重点讨论第二章。

## 有意义的名字

名字在软件中随处可见。我们给变量、函数、参数、类和包命名。我们命名我们的源文件和包含它们的目录。我们将 JAR 文件、WAR 文件和 EAR 文件命名为。我们不停地命名。

选择一个好名字需要时间，但是省下的钱比需要的多。所以请注意你的名字，当你找到更好的名字时就换掉它们。

变量、函数或类的名字应该回答所有的大问题。它应该告诉你**它为什么存在，它做什么，以及它是如何被使用的**。如果一个名字需要注释，那么这个名字就不会暴露它的意图。

```
public List<int[]> getThem(){
   List<int[]> list1 = new ArrayList<int[]>();
   for(int[] x: theList){
      if(x[0] ==4)
         list1.add(x);
   }
   return list1;
}
```

为什么很难判断这段代码在做什么？没有复杂的表达式。间距和缩进合理。只提到了三个变量和两个常数。甚至没有任何花哨的类或多态方法，只有一个数组列表。

问题不在于代码的简单性，而在于代码的隐含性，即代码本身的上下文不明确的程度。准则隐含地要求我们知道以下问题的答案:

*   清单上有哪些种类的东西？
*   *列表*中项目的第零个下标的意义是什么？
*   价值 4 的意义是什么？
*   我如何使用返回的列表？

代码示例中没有这些问题的答案，但它们可能会出现。假设我们在一个扫雷游戏中工作。我们发现棋盘是一个名为【theList 的单元格列表。让我们将其重命名为 *gameBoard。*棋盘上的每个单元格都由一个简单的数组表示。我们进一步发现，第零个下标是状态值 4 的位置，表示“已标记”。仅仅通过给这些概念命名，我们就可以大大改进代码:

```
public List<int[]> getFlaggedCell(){
   List<int[]> flaggedCells = new ArrayList<int[]>();
   for(int[] cell: gameBoard){
      if(cell[STATUS_VALUE] == FLAGGED)
         flaggedCells.add(cell);
   }
   return flaggedCells;
}
```

请注意，代码的简单性没有改变。它仍然有完全相同数量的运算符和常数，以及完全相同数量的嵌套层次。但是代码变得更加清晰了。

我们可以更进一步，为单元格编写一个简单的类，而不是使用一个*int 数组。*它可以包含一个意图揭示功能(称之为 *isFlagged* ) 来隐藏神奇的数字。它产生了新版本的函数:

```
public List<Cell> getFlaggedCell(){
   List<Cell> flaggedCells = new ArrayList<Cell>();
   for(Cell cell: gameBoard){
      if(cell.isFlagged())
         flaggedCells.add(cell);
   }
   return flaggedCells;
}
```

有了这些简单的改名，就不难理解是怎么回事了。这就是选择好名字的力量。

## 结论

当你在构建一个新的函数，一个新的方法，或者仅仅是一个简单的变量时，你应该在你的头脑中有一个想法“我怎样做才能创建一个易读的东西？”。