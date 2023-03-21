# JAVA-对象和类

> 原文：<https://medium.com/javarevisited/java-objects-classes-3567d2a9de9?source=collection_archive---------3----------------------->

## 什么是对象？什么是课？

![](img/363060c829216e313dae59fab0510375.png)

乔安娜·科辛斯卡在 [Unsplash](https://unsplash.com/s/photos/objects?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

众所周知，Java 是一种面向对象的语言。因此，它支持以下基本概念，如*继承、封装、多态、抽象、类、对象、实例和方法等。*

在本文中，我们将研究一下[类](https://javarevisited.blogspot.com/2011/10/class-in-java-programming-general.html)和[对象](https://javarevisited.blogspot.com/2012/12/what-is-object-in-java-or-oops-example.html#axzz6ikhp68Le)的概念。

**什么是对象？**

对象是具有状态和行为的实时实体。举个例子，猫有颜色、名字和品种等状态。猫也有吃东西和睡觉的行为。一个对象是一个类的实例，我们可以使用**“new”**关键字创建一个类的对象。

**JAVA 中的对象**

如果我们观察现实世界，我们将能够发现许多物体。如狗、猫和人类等。这些对象也有状态和行为。所以软件对象也有行为和状态。

因此，通过方法和状态表现出来的软件对象的行为存储在字段中。在软件开发中，对象到对象的通信是通过对对象的内部状态进行操作的方法和方法来完成的。

**什么是类？**

一个[类](https://javarevisited.blogspot.com/2020/05/object-oriented-programming-questions-answers.html)是一个模板，它描述了一个对象的状态和行为。

```
public class Cat { 
String breed; //state
int ageC
String color;void sleeping(){ //behaviour
}void hungry(){
}void eating(){
}
```

一个类可以有以下变量类型。

*   [**实例变量**](https://javarevisited.blogspot.com/2012/02/difference-between-instance-class-and.html) **-** 定义在类内但在方法外。当类被初始化时，这些变量也将被初始化。当方法完成时，这些变量将被销毁。
*   [**局部变量**](https://www.java67.com/2016/07/how-to-fix-variable-might-not-have-been-initialized-error-in-java.html) **-** 这些定义在方法、块或构造函数内部的变量称为局部变量。
*   [**类/静态变量**](https://javarevisited.blogspot.com/2011/11/static-keyword-method-variable-java.html) -【静态】关键字用来创建这些变量。这些变量是在类中声明的。