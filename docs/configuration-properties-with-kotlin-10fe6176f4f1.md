# Kotlin 的配置属性

> 原文：<https://medium.com/javarevisited/configuration-properties-with-kotlin-10fe6176f4f1?source=collection_archive---------1----------------------->

## 如何将 Spring Boot 配置属性与 Kotlin 数据类结合使用

![](img/93207d58a56c07d417764a9ec7ca8e6c.png)

[飞:D](https://unsplash.com/@flyd2069?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/props?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

自从在 Spring Boot 2.2.0 中引入了`@ConstructorBinding`注释以来，通过使用基于构造函数的属性绑定来创建不可变的配置属性类已经成为可能。本文描述了如何结合使用`@ConstructorBinding`和 [Kotlin 数据类](https://kotlinlang.org/docs/data-classes.html)来创建不可变的、干净的、简洁的配置属性类。除了基础知识之外，我们还将了解:使用嵌套属性、设置默认值、处理验证以及创建属性列表。

# 基础

让我们首先创建一个`data class`来保存我们的属性。

`@ConfigurationProperties(prefix = "person")`注释告诉 Spring 我们想要将外部属性绑定到这个类。`@ConstructorBinding`注释表明配置属性应该通过构造函数参数而不是通过调用 setters 来绑定。

因为我们在`@ConfigurationProperties`中指定了前缀`person`，Spring 将寻找`person.name`和`person.age`属性。我们需要声明这些属性，例如在一个`application.properties`文件中。

最后，我们必须启用对`@ConfigurationProperties`注释 beans 的支持。最简单的方法是使用`@EnableConfigurationProperties`注释。就个人而言，我喜欢在主应用程序类上设置这个注释，但是将它设置到任何 Spring 管理的组件也可以。

此时运行应用程序将产生一个具有我们指定的名称和年龄的 PersonProperties 对象。

```
PersonProperties(name=Don Draper, age=44)
```

# 嵌套属性

在使用配置属性类时，我经常遇到的一个问题是需要嵌套属性。幸运的是，`@ConstructorBinding`注释类的嵌套成员也将通过它们的构造函数自动绑定。因此，向我们现有的`PersonProperties`类添加一个`Address`就像创建一个数据类并为我们新的`person.address`属性声明值一样简单。

此时运行应用程序将产生以下 PersonProperties 对象:

```
PersonProperties(name=Don Draper, age=44, address=Address(street=Arden Road, city=New York))
```

# 默认值

您可能遇到的另一件事是需要有默认值的属性。设置默认值有几种方法，但是对于 Kotlin，我更喜欢使用允许您为构造函数参数提供默认值的语言特性。

这个例子展示了如何为`hairColor`添加一个默认值为`brown`的新属性。默认值可以通过在我们的`.properties`文件中为属性(在本例中为`person.hairColor`)指定一个值来覆盖。

# 确认

由于有权访问属性文件的每个人都能够更改这些值，所以您可能希望进行一些验证，以确保没有错误的数据进入您的系统。这可以通过多种方式实现，但是对于 Kotlin 来说，我发现使用`require`函数是最优雅的解决方案。

在我们的例子中，`age`属性具有负值是没有意义的，我们可以如下验证这个要求:

如果我们通过设置`person.age=-10`来更新`.properties`文件并运行应用程序，Spring 将无法启动，因为我们为`age`属性输入了一个无效值。

```
***************************
APPLICATION FAILED TO START
***************************Description:Failed to bind properties under ‘person’ to com.example.PersonProperties:Reason: Age cannot be negativeAction:Update your application’s configuration
```

# 列表

有时有必要将属性定义为列表。Kotlin 端不需要任何特殊的动作，因为 Spring 能够毫无问题地将属性绑定到一个列表。唯一略有不同的是您在`.properties`文件中定义属性的方式。在我们示例的上下文中，我们可能想要添加一个`Child`属性列表。

此时运行应用程序将产生一个带有 2 个子对象的`PersonProperties` 对象:

```
PersonProperties(name=Don Draper, age=44, hairColor=brown, children=[Child(name=Sally Beth Draper), Child(name=Robert Draper)], address=Address(street=Arden Road, city=New York))
```

# 结论

`@ConstructorBinding`的引入打开了将 Kotlin 数据类与 Spring Boot 配置属性结合使用的大门。再加上 Kotlin 语言的特性，比如构造函数参数的默认值和`require`函数，这允许我们以优雅的方式声明不可变的配置属性。

感谢阅读。我希望这有所帮助。如果您有任何问题或反馈，请随时回复。