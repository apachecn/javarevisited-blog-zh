# 工厂设计模式— Java

> 原文：<https://medium.com/javarevisited/factory-design-pattern-java-b755025437b5?source=collection_archive---------2----------------------->

## 工厂模式教程

![](img/84e993b5a0e16ef1869c1a9657ce87ff.png)

# 工厂模式的定义

> 在基于类的编程中， [**工厂方法模式**](https://javarevisited.blogspot.com/2015/06/difference-between-dependency-injection.html) 是一种创建模式，它使用工厂方法来处理创建对象的问题，而不必指定将要创建的对象的确切类。这是通过调用[工厂方法](http://javarevisited.blogspot.sg/2011/12/factory-design-pattern-java-example.html#axzz51cvxH5kW)来创建对象来完成的——要么在接口中指定并由子类实现，要么在基类中实现并可选地由派生类覆盖——而不是通过调用构造函数。

# 在哪里使用工厂模式

*   当一个类不知道需要创建什么子类时
*   当一个类希望它的子类指定要创建的对象时。
*   当父类选择创建对象到它的子类时。

# UML 示例

[![](img/69f0a8d3bf000f4970fd4c3525c7f728.png)](https://javarevisited.blogspot.com/2018/02/top-5-java-design-pattern-courses-for-developers.html)

# 工厂模式的实现

我们将创建一个人类[抽象类](https://www.java67.com/2017/08/difference-between-abstract-class-and-interface-in-java8.html)，然后是实现它的不同年龄阶段，然后我们将创建生成它的工厂。

## 人类抽象类

## 实现

每个实现[都会覆盖](https://www.java67.com/2015/08/top-10-method-overloading-overriding-interview-questions-answers-java.html)的 *getAge()* 方法，返回一个与年龄周期相对应的随机整数。

## 工厂

现在我们可以创建一个工厂，它将根据给定的信息生成一个人，这里我们将使用一个[枚举](https://javarevisited.blogspot.com/2011/08/enum-in-java-example-tutorial.html)来确保我们不会将不存在的值传递给工厂

现在我们可以使用工厂来制造人类

## 使用

这将产生如下所示的输出:

```
==============================
age : 8
==============================
==============================
age : 49
==============================
==============================
age : 99
==============================
```

以下是你可能喜欢的其他**设计模式文章**

[](/javarevisited/7-best-online-courses-to-learn-object-oriented-design-pattern-in-java-749b6399af59) [## 学习 Java 面向对象设计模式的 7 门最佳在线课程

### 每个程序员都应该学习设计模式来编写干净的代码，并成为更好的开发人员。

medium.com](/javarevisited/7-best-online-courses-to-learn-object-oriented-design-pattern-in-java-749b6399af59) [](/javarevisited/10-oop-design-principles-you-can-learn-in-2020-f7370cccdd31) [## 2021 年你可以学到的 10 个 OOP 设计原则

### 想要写出更好、更可靠的代码，能够经受住生产中时间的考验吗？这些设计原则会有所帮助。

medium.com](/javarevisited/10-oop-design-principles-you-can-learn-in-2020-f7370cccdd31) [](/javarevisited/7-best-books-to-learn-design-patterns-for-java-programmers-5627b93eefdb) [## Java 程序员学习设计模式的 7 本最佳书籍

### 设计模式是面向对象程序员的基本话题，比如 Java 和 C++开发人员。它甚至变得…

medium.com](/javarevisited/7-best-books-to-learn-design-patterns-for-java-programmers-5627b93eefdb)