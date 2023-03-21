# 用这两个概念从 Java 初学者到中级快速入门

> 原文：<https://medium.com/javarevisited/fastrack-from-beginner-to-intermediate-in-java-with-these-2-concepts-4fd4c996975?source=collection_archive---------1----------------------->

10 分钟内掌握 Java 技术

![](img/f8175c74730c38b2cd3d76932cb98728.png)

作者图片

这个博客解释了两个重要的概念，对初级和中级 Java 工程师都有帮助。如果您是一名前端工程师，希望提高您的 Java 知识，或者对 Java 或 Spring boot 比较陌生，请继续阅读，了解曾经使用过的最常用和最有用的概念之一。这将使你从编写冗长的方法转移到简明易懂的方法，并使你为自己是工程师而感到自豪。

工程师们喜欢他们的代码能够如此美妙地与他们的读者交流，而这正是采用这种技术所能实现的。你的代码将是可读性很强的，它将表达其存在的意图而不是内部机制(..和本质细节)

**学习 1:λ表达式**

你有没有想过有一天你不得不遍历一个集合，比如[列表](https://www.java67.com/2013/01/difference-between-set-list-and-map-in-java.html)？

以前，步骤包括:

1.  抓住列表的迭代器(对于这里的代码示例，想象一个定义为列表的果篮)
2.  开始一个 for 循环或 while 循环，条件是检查[迭代器](https://javarevisited.blogspot.com/2014/01/ow-to-remove-objects-from-collection-arraylist-java-iterator-traversing.html)是否指向最后一个水果
3.  打印出水果名称
4.  将迭代器指向下一个水果
5.  退回到 for 循环

```
Iterator listIterator = fruitBasket.iterator();
while (listIterator.hasNext()) {
	System.out.println(listIterator.next());
}
```

现在，想象一下你可以将上面的 4 行减少到 1 行……并且不需要显式地使用迭代器。λ表达式正是这么做的。在下面的例子中，forEach 方法抽象了为 Loop 显式打开[的需要。- >操作符指示编译器，跟在它后面的是一个方法体，需要为每个结果执行。](https://www.java67.com/2013/08/how-to-iterate-over-array-in-java-15.html)

```
fruitBasket.forEach(fruit -> System.out.println(fruit));
```

**学习 2:** [**Java 流**](/javarevisited/8-best-lambdas-stream-and-functional-programming-courses-for-java-developers-3d1836a97a1d)

上面介绍的 Lambda 表达式只是真正游戏——Java 流的热身。

在这个时代，数据无处不在。十之八九，你会遇到对大量数据执行数据操作的需求，通常伪装成一个[列表](https://javarevisited.blogspot.com/2011/05/example-of-arraylist-in-java-tutorial.html#axzz6qVaG06bu)，或者一个[散列表](https://www.java67.com/2013/02/10-examples-of-hashmap-in-java-programming-tutorial.html)。

从我们之前的例子，让我们说，现在你想如果你的水果篮含有草莓

```
fruitBasket = [“Apple”, “Orange”, “Mango”, “Strawberry”, “Mango”, “Apple”]
```

以前，如果没有 [Java Stream](/javarevisited/7-best-java-collections-and-stream-api-courses-for-beginners-in-2020-3ad18d52c38) ，你会写出这样的东西

```
public List<Fruit> searchFruitBasket(List<Fruit> fruitBasket, String searchFruit) {
List<Fruit> fruitFound = new ArrayList<>();
for(Fruit f: fruitBasket)
 {
     if(f.getName().equals(searchFruit))
       fruitFound.add(f);
 }

 return fruitFound;
}
```

但是使用 Java 流收集，您只需编写两行代码，并绕过创建临时的[垃圾收集器变量](https://www.java67.com/2020/02/50-garbage-collection-interview-questions-answers-java.html)，比如本例中的 fruit。

```
public List<Fruit> searchFruitBasket(List<Fruit> fruitBasket, String searchFruit) {
  return fruitBasket.stream()
      .filter(f -> f.getName().equals(searchFruit))
      .collect(Collectors.toList());
}
```

这里流有一个 [lambda 表达式](https://javarevisited.blogspot.com/2014/02/10-example-of-lambda-expressions-in-java8.html#axzz6ieZZarMY)(见下)应用于流的每个对象。

```
f -> f.getName().equals(searchFruit)
```

现在让我们分解代码中的新函数:

1.  [stream()](https://www.java67.com/2014/04/java-8-stream-examples-and-tutorial.html) :这个方法在幕后将水果列表作为元素序列提供给一个流(在幕后实例化并从编码器中抽象出来)。
2.  [filter()](https://www.java67.com/2018/03/java-8-stream-find-first-and-filter-example.html) :这是一个聚合操作符，我们应用于流中的每个元素。每个元素表示为 f。对于每个元素 f，我们检查它的名称是否等于我们要搜索的水果。如果为真，则保留该元素，如果为假，则从列表中删除该元素。正如您所看到的，布尔检查被抽象出来，由幕后的流执行。
3.  [收集()](https://www.java67.com/2018/06/java-8-streamcollect-example.html):这是至关重要的方法。在编译器看到 collect 方法之前，上述任何计算都不会完成。

本文将向您介绍 Java 中的两个重要概念。有很多有趣的事情可以和流一起做。查看我的下一篇文章，深入了解 Java 流。