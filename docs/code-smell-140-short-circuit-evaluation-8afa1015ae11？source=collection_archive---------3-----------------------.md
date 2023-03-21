# 代码气味 140 —短路评估

> 原文：<https://medium.com/javarevisited/code-smell-140-short-circuit-evaluation-8afa1015ae11?source=collection_archive---------3----------------------->

## 我们在最初的编程课程中学习捷径。我们需要记住为什么。

[![](img/5b097db1db197440cbfe1b504a59e661.png)](https://javarevisited.blogspot.com/2015/01/difference-between-bitwsie-and-logical.html)

> *TL；DR:在评估布尔条件时偷懒*

# 问题

*   副作用
*   双向断层
*   性能问题

# 解决方法

1.  用短路代替完整的评估

# 语境

我们在 101 计算机课程中学习布尔。

布尔的真值表对数学来说很棒，但是作为软件工程师，我们需要更聪明。

短路评估帮助我们偷懒，甚至建立无效的完整评估。

# 示例代码

## 错误的

```
<?if (isOpen(file) & size(contents(file)) > 0)
  // Full evaluation 
  // Will fail since we cannot retrieve contents 
  // from not open file
```

## 对吧

```
<?if (isOpen(file) && size(contents(file)) > 0)
  // Short circuit evaluation 
  // If file is not open it will not get the contents
```

# 侦查

[X]自动

我们可以在开发人员使用完全评估时警告他们。

# 标签

*   布尔代数学体系的

# 例外

不要使用短路作为 IF 替代方案。

如果操作数有副作用，这是另一种代码味道。

# 结论

大多数编程语言都支持短路。

他们中的许多人把它作为唯一的选择。

我们需要喜欢这种表达方式。

# 关系

[](https://blog.devgenius.io/code-smell-97-error-messages-without-empathy-952c320976d8) [## 代码气味 97——没有同理心的错误消息

### 我们应该特别注意用户(和我们自己)的错误描述。

blog.devgenius.io](https://blog.devgenius.io/code-smell-97-error-messages-without-empathy-952c320976d8) [](https://blog.devgenius.io/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) [## 代码气味 69 —大爆炸(JavaScript 荒谬的铸件)

blog.devgenius.io](https://blog.devgenius.io/code-smell-69-big-bang-javascript-ridiculous-castings-870f20469322) 

# 更多信息

*   [维基百科](https://en.wikipedia.org/wiki/Short-circuit_evaluation)

> 编写一个没有合同的类就像生产一个没有规范的工程组件(电路、VLSI(超大规模集成)芯片、桥、引擎……)。没有专业工程师会考虑这个想法。

*伯特兰·梅耶*

[](https://blog.devgenius.io/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](https://blog.devgenius.io/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](https://blog.devgenius.io/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](https://blog.devgenius.io/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)