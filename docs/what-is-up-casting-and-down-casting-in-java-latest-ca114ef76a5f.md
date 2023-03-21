# Java 中什么是向上强制转换和向下强制转换？

> 原文：<https://medium.com/javarevisited/what-is-up-casting-and-down-casting-in-java-latest-ca114ef76a5f?source=collection_archive---------0----------------------->

## 面向对象编程

## 为你下次面试准备的 java 编程中向上造型和向下造型的完整指南。

![](img/29e38ebb9ed3867db261ccc102fba581.png)

照片由 Clément H 在 Unsplash 上拍摄

在本文中，我将解释 Java 中的[转换特性。通过阅读这篇文章，你将能够理解与铸造相关的概念和程序。何时使用它们以及如何有效地写它们。](https://javarevisited.blogspot.com/2012/12/what-is-type-casting-in-java-class-interface-example.html)

> 铸造意味着将一种类型转换成另一种类型。

## 在 java 中，我们有两种类型的造型:

## 1.向上抛

## 2.向下铸造

现在，让我们首先了解定义:

# 1.向上投射:

将子类对象分配给父类引用。

```
Syntax for up casting : Parent p = new Child();
```

这里`**p**`是一个父类引用，但指向子对象。

*这个引用* `*p*` *可以访问父类的所有方法和变量，但只能访问子类中被覆盖的方法。*

***向上转换给了我们访问父类成员的灵活性，但是使用这个特性不可能访问所有的子类成员。我们可以访问子类的一些指定成员，而不是所有成员。例如，我们只能访问子类中被覆盖的方法。***

# 2.向下投射:

将父类引用(指向子类对象)分配给子类引用。

```
Syntax for down casting : Child c = (Child)p;
```

这里 **p** 指向子类的对象，正如我们在前面的例子中看到的，现在我们将这个引用 **p** 转换为子类引用 **c** 。

***现在这个子类引用 c 可以访问子类以及父类的所有方法和变量。***

> 例如，如果我们有两个类，比如说`**Machine**` 和`**Laptop**` ，它们扩展了`**Machine**` 类。现在对于上抛来说，每台笔记本电脑都将是一台机器，但是对于下抛来说，每台机器可能都不是笔记本电脑，因为可能有一些机器可以是`**Printer**`、`**Mobile**`、**、**等。
> 
> **因此向下转换并不总是安全的，我们在进行向下转换之前显式地写类名。**这样它就不会在编译时给出错误，但如果父类引用没有指向适当的子类，它可能会在运行时抛出`[**ClassCastExcpetion**](https://javarevisited.blogspot.com/2012/12/how-to-solve-javalangclasscastexception-java.html)` <https://javarevisited.blogspot.com/2012/12/how-to-solve-javalangclasscastexception-java.html>。

让我们看下面的例子来理解这一点:

```
Machine machine = new Machine ();
Laptop laptop = (Laptop)machine;//this won't give an error while compiling
//laptop is a reference of type Laptop and machine is a reference 
//of type Machine and points to Machine class Object .
//So logically assigning machine to laptop is invalid 
//because these two classes have different object strucure.
//And hence throws ClassCastException at run time . 
```

> 为了去掉`ClassCastException`,我们可以使用`[instanceof](https://javarevisited.blogspot.com/2015/12/10-points-about-instanceof-operator-in-java-example.html)` [操作符](https://javarevisited.blogspot.com/2015/12/10-points-about-instanceof-operator-in-java-example.html)在向下转换的情况下检查类引用的正确类型。

## 举个例子，

```
if(machine instanceof Laptop){
    Laptop laptop = machine;
    //here machine must be pointing to Laptop class object .
}
```

我试图用例子解释向上转换和向下转换的用例。试着理解这个程序，并阅读评论以获得完整的解释。

**上抛和下抛**

## 输出:

```
Machine has started!!!
Laptop has started!!!
Laptop has stopped!!!
Laptop has started!!!
Laptop has started!!!
Laptop has started!!!
Laptop has stopped!!!
```

# 为什么向下转换不安全？

在下面的程序中，我解释了为什么向下转换是不安全的。我也向你展示了你什么时候会得到[*ClassCastException*](https://javarevisited.blogspot.com/2012/12/how-to-solve-javalangclasscastexception-java.html)。

一个例外的例子是向下转换

## 输出:

```
Machine has started!!!
Laptop has started!!!
Laptop has stopped!!!
Exception in thread “main” java.lang.ClassCastException: com.vikram.Machine cannot be cast to com.vikram.Laptop
at com.vikram.Main.main(Main.java:38)
```

*本文到此为止。希望你喜欢这篇文章。*

# 类似内容可以关注[维克拉姆古普塔](https://medium.com/u/2c3b611409dc?source=post_page-----ca114ef76a5f--------------------------------)。

## ***你可能喜欢阅读下面的博客，了解常见的 java 面试概念和问题。***

</@basecs101/do-you-know-nested-and-inner-classes-in-java-latest-b270e0988091>  </@basecs101/do-you-know-the-tight-and-loose-coupling-in-the-oop-latest-b03f8e600115>  </@basecs101/confused-with-enum-here-is-an-article-to-clear-it-latest-e39d88fe7c66> 