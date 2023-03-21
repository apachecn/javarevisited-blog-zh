# Java 接口

> 原文：<https://medium.com/javarevisited/interface-in-java-99e6838addff?source=collection_archive---------1----------------------->

![](img/7e57286293a6ef918d63aff6addf6d50.png)

接口是 Java 的一个非常重要的特性，它的定义以关键字 **interface 开始。**很像一个班，有些不同。基本上，通过 interface 关键字，你可以在 java 中声明和定义一个接口。

**接口**接口 _ 名称

{

………………………………….

}

一个[接口](https://javarevisited.blogspot.com/2012/04/10-points-on-interface-in-java-with.html)基本上指定了方法声明并包含字段或变量。

**接口**接口名称

{

int a = 5；

void 方法 1()；

}

注意:**任何接口的方法都是隐式的**[**public**](https://javarevisited.blogspot.com/2012/10/difference-between-private-protected-public-package-access-java.html#axzz6j8KhisSX)**和**[**abstract**](http://javarevisited.blogspot.sg/2017/07/is-it-possible-to-have-abstract-method-in-final-class-java.html#axzz4xXS86IVo)**，变量是** [**public static 和 final**](http://javarevisited.blogspot.sg/2017/01/how-public-static-final-variable-works.html) **。**

如何实现一个接口？

**接口** Ione

{

int a = 5；

void 方法 1()；

}

A 类**实现** Ione{

公共 void 方法 1(){

system . out . println(a)；

}

}

java 中的类基本都是通过关键字**实现**来实现接口。

现在看一个例子，

接口 A {

*作废*打印()；

}

类 B 实现了一个

{

公开*作废*打印()

{

system . out . println(" It is warm up ")；

}

}

公共类 Main {

公共静态*void*main(String[]*args*){

A a//接口引用

a = new B()；//接口引用 a 引用的 B 类实例

a . print()；

}

}

你可以在上面的程序中看到，接口 A 包含一个由实现接口 A 的类 B 定义和实现的方法 print()。由于一个[接口](https://www.java67.com/2012/09/what-is-difference-between-interface-abstract-class-java.html)的方法在该接口中没有任何实现，它们由实现该接口的类来实现。

现在仔细看看主类。一个接口创建了一个引用 **a** ，但是这个引用引用了实现这个接口的 B 类实例。基本上，问题是，**接口不能被实例化**。它创建引用，并且该引用引用实现该接口的任何类的实例。

看另一个例子，

公共接口 myInterface {

*int*a = 10；//公共、静态和最终

*void* 显示()；//公共和抽象

}

类 myClass 实现 myInterface {

public *void* display() //实现抽象方法

{

System.out.println("好吧！");

}

public static*void*main(String[]*args*){

my class m = new my class()；

m .显示器()；

system . out . println(" my interface 中的最终值 a "+a)；

}

}

**要点记住:**

1.无法实例化接口。

2.接口没有构造函数。

**一个接口可以扩展另一个接口:**

I1 界面{

*double*x = 4.444；

*虚空*method 1()；

}

接口 I2 扩展 I1 {

*双*y = 5.555；

*void*method 2()；//默认情况下为公共静态

}

A1 类实现 I2 {

public*void*method 1(){

System.out.println("来自 I1:"+(x+y))；

}

公共*无效*方法 2(){

system . out . println(" From I2:"+(x+y))；

}

}

公共类 myClass{

public static*void*main(String[]*args*){

a1a = new A1()；

a . method 1()；

a . method 2()；

}

}

这里 I2 接口扩展了接口 I1 并继承了它的属性。因此，当类 A 实现 I2 时，它也可以使用 I1 的属性。

**接口的多重扩展:**

包博客；

公共接口 IOne {

*void*print 1()；

}

公共接口 ITwo {

*void*print 2()；

}

公共接口 IThree 扩展了 IOne，ITwo{

*void*print 3()；

}

包博客；

公共类主要实现 IThree{

公共*作废* print1(){

system . out . println(" IOne 的方法")；

}

public *void* print2(){

system . out . println(" ITwo 的方法")；

}

public *void* print3(){

system . out . println(" I three 的方法")；

}

public static*void*main(String[]*args*){

Main m1 = new Main()；

m1 . print 1()；

m1 . print 2()；

m1 . print 3()；

}

}

**嵌套接口:**

一个接口可以被声明为一个类或另一个接口的成员。这样的接口被称为**嵌套接口**。一个嵌套的接口可以声明为**公共**、[私有、**保护**。](https://javarevisited.blogspot.com/2012/03/private-in-java-why-should-you-always.html)

封装嵌套接口；

公共 A 类{

公共接口 Nestin

{

*布尔*非负(*int*x)；

}

}

B 类实现了一个*。*巢蛋白

{

public *boolean* 非负(*int*x)

{

return*x*0？假:真；

}

}

封装嵌套接口；

公共类 Main {

public static*void*main(String[]*args*){

A.巢蛋白 i1 =新 B()；

System.out.println(i1 .非负(-1))；

system . out . println(i1 . nonnegative(5))；

}

}

相关主题:

[为什么 java 不支持多重继承？](https://javarevisited.blogspot.com/2011/07/why-multiple-inheritances-are-not.html)

[Java 中的继承](https://javarevisited.blogspot.com/2012/10/what-is-inheritance-in-java-and-oops-programming.html)