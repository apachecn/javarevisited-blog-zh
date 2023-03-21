# Java 泛型备忘单

> 原文：<https://medium.com/javarevisited/java-generics-cheat-sheet-2b178948233?source=collection_archive---------0----------------------->

![](img/144d3184900aac14d0c693a17d893c53.png)

我们在周围看到了仿制药，它无处不在，但仍然如此神秘。让我们试着理解泛型的基础。

# **为什么是仿制药？**

1.  编译时更强的类型检查。
2.  使程序员能够实现通用算法，这些算法可以应用于各种各样的对象，而不必为每种类型编写多次。

# **通用类**

让我们看一个简单的例子

```
**public class** Node<T> {
    **private** T **data**;

    **public** Node(T data) {
        **this**.**data** = data;
    }

    **public void** setData(T data) {
        **this**.**data** = data;
    }
}
```

菱形运算符中的泛型 T 是类型。我们可以使用不同的类型，如基本类型或自定义类，如下所示:

```
Node<String> myStringNode = **new** Node<>(**"Hello world!!"**);
Node<Integer> myIntegerNode = **new** Node<>(100);
Node<Number> myNumberNode = **new** Node<>(11.23);
```

# **通用方法**

让我们看看泛型方法的一些简单例子。

```
**public static** <T> **void** print(T type) {
    System.***out***.println(type.toString());
}

**public static void** main(String[] args) {
    *print*(**new** Integer(100));
    *print*(**new** Float(1.11));
    *print*(**new** Person(**"Danerys"**, **"Targaryen"**));
}

**public static class** Person {
    String **firstName**;
    String **lastName**;

    MyClass(String firstName, String lastName) {
        **this**.**firstName** = firstName;
        **this**.**lastName** = lastName;
    }

    @Override
    **public** String toString() {
        **return firstName** + **" "** + **lastName**;
    }
}
```

# 边界类型参数

当您想要限制类型参数时，可以使用这些参数。例如，我们可以定义一个函数，不管参数的类型如何，它都至少返回两个参数。因此，该函数将接受任何实现类似接口的类。

```
**public static** <T **extends** Comparable<T>> T getMinimum(T t1, T t2) {
    **if** (t1.compareTo(t2) < 0) {
        **return** t1;
    }
    **return** t2;
}

**public static void** main(String[] args) {
    System.***out***.println(*getMinimum*(100,20));
    System.***out***.println(*getMinimum*(**"aman"**,**"rohan"**));
}output:
20
aman
```

# 无限通配符

假设我们有一个类*形状*和另一个类*矩形*。*矩形*延伸*形状*。现在假设我们有一个列表< *形状* >和列表< *矩形* >

*矩形*列表不是*形状*列表，尽管*矩形*延伸了*形状*。*形状*的集合不是*矩形*集合的超类型。

下面的代码将抛出一个编译时异常，因为列表<*形状* >与列表< *矩形* >不同

```
**private static void** printArea(List<Shape> list){
    **for** (Shape shape:list){
        System.***out***.println(shape.getArea());
    }
}

**public static void** main(String[] args) {
    List<Rectangle> rectangles = **new** ArrayList<>();
    *printArea*(rectangles);//will throw compile time error here
}

**public interface** Shape{
    **int** getArea();
}

**public static class** Rectangle **implements** Shape{
    **public int** getArea(){
        **return** 1*2;
    }
}

**public static class** Circle **implements** Shape{
    **public int** getArea(){
        **return** 314;
    }
}
```

我们可以使用通配符来解决这个问题

问号(？)被称为泛型编程中的通配符。它表示未知类型。

```
**private static void** printArea(List<? **extends** Shape> list){
    **for** (Shape shape:list){
        System.***out***.println(shape.getArea());
    }
}
```

这将适用于所有实现形状接口的类

# **类型橡皮擦**

为了让泛型无缝工作，java 编译器使用了类型擦除器。

什么是橡皮擦？

类型擦除器是 java 编译器使用的一种技术，它使我们的生活变得更加简单。它基本上删除了所有的泛型类型，用普通的类和接口来代替，以便 JVM 理解。它执行以下所有操作:

a.如果所有泛型类型是无界的，则用对象替换它们

b.用其绑定替换所有泛型类型

c.如有必要，插入类型转换

d.生成桥接方法以保持扩展泛型类型中的多态性

这是在类型擦除过程中添加的一个额外的方法，以避免不明确的情况

例如

```
**public class** Node<T> {
    **private** T **data**;

    **public** Node(T data) {
        **this**.**data** = data;
    }

    **public void** setData(T data) {
        **this**.**data** = data;
    }
}**public class** MyNode **extends** Node<Integer> {

    **public** MyNode(Integer data){
        **super**(data);
    }

    @Override
    **public void** setData(Integer data) {
        **super**.setData(data);
    }
}
```

键入 eraser 后，生成的代码如下:

```
**public class** Node {
    **private** Object **data**;

    **public** Node(Object data) {
        **this**.**data** = data;
    }

    **public void** setData(Object data) {
        **this**.**data** = data;
    }
}**public class** MyNode **extends** Node {

    **public** MyNode(Integer data){
        **super**(data);
    }

    **public void** setData(Integer data) {
        **super**.setData(data);
    }

    @Override
    **public void** setData(Object data) {
        setData((Integer)data);
    }
}
```

编译器增加了一个额外的方法来保持多态性

```
@Override
**public void** setData(Object data) {
   setData((Integer)data);
}
```

因此，每当我们使用泛型类型时，编译器都会为我们做一些繁重的工作。它使用橡皮擦生成所有的代码，否则就必须由我们来编写。

泛型是一个很棒的工具，它让我们可以编写抽象出逻辑的地方，然后这个逻辑可以应用于给定类型的所有对象。集合框架是一个很好的例子，说明了泛型是如何使我们的生活变得更简单的，使用它我们可以使我们的代码更干净，避免重复。

我认为泛型是每个 Java 开发人员都应该掌握的工具。