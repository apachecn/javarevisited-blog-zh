# Java 中的泛型解释

> 原文：<https://medium.com/javarevisited/generics-in-java-explained-10bd559b3a94?source=collection_archive---------1----------------------->

## 到底什么是泛型？

最近，我不得不解释什么是泛型，以及它们在 Java 语言中是如何使用的。简单地说。几分钟之内。

我最后说，泛型是一种机制，它允许我们编写不关心它所处理的对象类型的代码，同时给编译器提供足够的信息来保护类型安全。类似于给编译器留下的注释。

![](img/a4ea1b90dcd5329358186bca2217c467.png)

凯利·西克玛在 [Unsplash](https://unsplash.com/s/photos/placeholder?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 我们不能总是用对象来表示任何类型

让我们从列表的经典例子开始。暂时忘记了 [List](https://javarevisited.blogspot.com/2016/01/9-difference-between-array-vs-arraylist-in-java.html#axzz6rDtB0US0) 和 [LinkedList](https://javarevisited.blogspot.com/2012/02/difference-between-linkedlist-vs.html#axzz5hP3QBNdL) 是泛型类型，下面是合法的 Java 并创建一个包含任何对象类型的列表。

```
List myList = new LinkedList();
```

这意味着这是合法的:

```
myList.add(“Hello”);
```

这也是合法的:

```
myList.add(1);
```

这种类型的混合感觉可疑。我们还可以写下面的循环吗？

```
for (Integer i : myList) System.out.println(i);
```

不。因为列表没有携带关于它的元素的类型信息，编译器不能验证变量 I 的每个赋值是否有效。相反，我们必须写:

```
for (Object i : myList) System.out.println(i);
```

如果我们打算将每个列表元素作为一个整数来处理，以执行一些算术运算，这就变得很难看:

```
for (Object i : myList) System.out.println(((Integer) i) * 2);
```

事实上，它变得如此丑陋，以至于任何不是整数的元素都会用一个 [ClassCastException](https://javarevisited.blogspot.com/2012/12/how-to-solve-javalangclasscastexception-java.html#axzz6qVaG06bu) 使应用程序崩溃。当使用 Object 来表示某个未知类型时，这就是问题所在:当假设该类型表示什么时，代码会变得脆弱。

## 通过使用类型参数修复问题

当我们利用[泛型类型](https://www.java67.com/2019/07/top-50-java-generics-and-collection-interview-questions.html)并编写以下代码时，这个问题就解决了，因为编译器现在可以为我们做所有的检查:

```
List<Integer> myList = new LinkedList<Integer>()myList.add(1);myList.add(“Hello”); // compiler error: now it is illegal.for (Integer i : myList) System.out.println(i * 2); // beautiful.
```

现在，我们可以确定这个列表只包含整数。

类型参数首先是编译器的指令。人类同胞排第二。

```
interface SomeCollection<T> { void add(T element);}
```

当我们声明一个像上面这样的接口时，我们告诉编译器它必须确保 add()方法的参数总是用户代码(T)指定的类型。不管它是什么类型，我们都不在乎，只要它总是一样的！

[泛型类型](https://javarevisited.blogspot.com/2012/08/how-to-write-parametrized-class-method-Generic-example.html#axzz6tWnd7QYO)也可以在方法级别使用。假设我们在某个类中声明了这个函数:

```
public <T,R> Optional<R> process(T input, Predicate<T> validator, Function<T,R> processor) { return validator.test(input) ? Optional.ofNullable(processor.apply(input)); : Optional.empty();}
```

这个函数有点复杂，但它仍然是一个合适的例子。这里，我们使用泛型类型告诉编译器两件事:

1.  验证器和处理器必须处理兼容的类型，用 t 表示。
2.  我们的函数返回一个[可选](https://javarevisited.blogspot.com/2017/04/10-examples-of-optional-in-java-8.html#axzz6ccm5KWKs)的实例，包装处理器的返回值，用 r 表示

同样，我们不关心这些是什么类型。我们只想确保它们是兼容的。

## 当泛型类型必须有一些标准语义时

可以使用 extends 关键字对泛型类型声明进行限制，以公开已知的接口。这样，泛型类型在方法或类中变得有意义和可用。然而，基本概念仍然存在。任何类型都可以用来代替泛型类型，只要它实现指定的接口。

```
class MyWrapper<T extends MyOtherInterface> { // use T in some manner}
```

## 结论

Java 中的泛型类型允许我们编写可以处理不同对象类型的高级代码，而无需知道确切的类型，同时保持类型安全。