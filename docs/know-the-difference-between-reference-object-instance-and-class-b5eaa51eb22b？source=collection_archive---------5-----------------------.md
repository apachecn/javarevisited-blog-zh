# 知道引用、对象、实例和类的区别吗？

> 原文：<https://medium.com/javarevisited/know-the-difference-between-reference-object-instance-and-class-b5eaa51eb22b?source=collection_archive---------5----------------------->

## 面向对象编程

## 弄清楚编程中使用的这些术语。

![](img/29e38ebb9ed3867db261ccc102fba581.png)

**照片由** [**Clément H**](https://unsplash.com/@clemhlrdt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) **上** [**Unsplash**](https://unsplash.com/s/photos/java-programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本文中，我将解释 java 编程语言中最常用的术语。类、对象、实例和引用是您在编写代码时可能每天都会听到的一些术语。读完这篇文章后，你会了解这些术语及其不同的用法。

# 什么是课？

类是对象的蓝图/模板/表示或用户定义的数据类型。我们只为数百个对象编写一个类。使用 **class** 关键字后跟类名来定义一个类，然后定义类体。

## 示例:

```
class Student {
//instance variables (attributes)
//instance methods(behaviours)
}
```

使用 **class** 关键字将 **Student** 定义为一个类，然后在花括号内定义**实例** **变量、**和**实例方法**。

# 什么是对象？

对象是一个真实世界或软件入口，它具有属性(实例字段)和行为(实例方法)。对象是用堆中的 **new** 操作符创建的。例如**new class name()；。当类装入内存时，对象被实例化。**对象**也被称为**实例**。**

## 示例:

```
Student s1 = new Student();
```

这里使用堆中的 **new** 操作符创建了 **Student** 类型的对象，并在变量 **s1 中返回地址，**然后调用默认构造 **student()** 。

# 什么是参考？

Reference 保存对象或实例的地址。每当我们想调用实例方法时，我们就使用这个包含对象地址的引用。引用就像 C++指针。

例如。

```
Student s1 = new Student();
```

**s1** 是类型**学生**的引用，指向类型**学生**的对象，将用于访问实例变量和方法。

下面我用实例变量和方法写了学生类。并在 Main(驱动程序类)中创建了这个类的对象和引用。

**类、对象和引用的例子**

## 输出:

```
Name : Vikram
Name : Vikram Gupta
Name : Vikram Gupta
```

# Java 编译器什么时候添加默认构造函数？

如果一个类没有程序员提供的任何构造函数，那么 java 编译器将添加一个不带参数的默认构造函数，该构造函数将通过 super()调用在内部调用超类构造函数。这被称为默认构造函数。

你可以看到，我没有在 Student 类中添加默认构造函数，因此编译器将创建一个默认构造函数，并将其添加到该类中。

## 示例:

```
public ClassName()
{
    super();//parent class constructor call .
}
```

**注意**:在默认构造函数里面，还会添加一个 super()调用，来调用超类构造函数。[在学生类的情况下，超类是对象类]并且每个类都在内部继承对象类。

# 添加默认构造函数的目的:

构造函数的职责是初始化实例变量。如果没有实例变量，那么你可以选择从你的类中移除构造函数。

但是当你继承某个类时，你有责任调用超类构造函数来确保超类正确初始化它的所有实例变量。

这就是为什么如果没有构造函数，java 编译器会添加一个默认的构造函数，并调用一个超类构造函数。

> 注意:**【super()】**调用内部默认的构造函数一般是隐藏的。

*本文到此为止。希望你喜欢这篇文章。*

# 类似内容可以关注[维克拉姆古普塔](https://medium.com/u/2c3b611409dc?source=post_page-----b5eaa51eb22b--------------------------------)。