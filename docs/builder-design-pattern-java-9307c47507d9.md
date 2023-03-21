# 构建器设计模式— java

> 原文：<https://medium.com/javarevisited/builder-design-pattern-java-9307c47507d9?source=collection_archive---------1----------------------->

![](img/84e993b5a0e16ef1869c1a9657ce87ff.png)

# 构建器模式的定义

> 构建器模式是一种设计模式，允许使用正确的操作顺序逐步创建复杂的对象。构造由一个 director 对象控制，该对象只需要知道它要创建的对象的类型。

# 在哪里使用构建器模式？

当你有一个简单的对象时，这种模式不是很有用，但是当你开始有一个更复杂的对象并且想要有一个清晰的代码时，你可以毫不犹豫地使用它

例如:

参数数量有限的构造函数不会让你的代码不可读。
但是，如果现在你有这样的东西呢:

传递给构造函数的参数数量会使代码行不可读。
是的，在你初始化你的对象之后，你仍然可以使用 setter 来改变一个属性的值。但是，如果您的所有字段都是 [final](https://javarevisited.blogspot.com/2016/09/21-java-final-modifier-keyword-interview-questions-answers.html) ，因为您想要将它们呈现为[不可变的](/javarevisited/how-to-create-an-immutable-list-list-and-map-in-java-5ac1254c128?source=---------31------------------)呢？(在本文的其余部分中它们将是什么)或者如果一些字段是强制性的而其他的不是呢？

# 构建器模式的实现

我使用 Lombok 不对 getter 和默认构造函数[进行编码，因为](https://javarevisited.blogspot.com/2014/01/why-default-or-no-argument-constructor-java-class.html)[构建器模式](http://javarevisited.blogspot.sg/2012/06/builder-design-pattern-in-java-example.html)使代码行数加倍。这是我的`HumanBuilder`的实现，现在我如何用它来声明一个人类呢？你不会看到任何困难(或者我应该说阅读？).

这将给出这个输出

```
Human{
  name='Erwan', 
  lastName='null', 
  age=30, 
  height='null', 
  weight='null', 
  eyesColor='null', 
  hairColor='null', 
  birthPlace='Saint Cyr l'Ecole', 
  birthDate=null, 
  numberOfSibling=2, 
  married=true
}
```

如您所见，我可以选择要初始化的参数数量及其顺序。