# 声明文件在 TypeScript 实现中的作用

> 原文：<https://medium.com/javarevisited/role-of-declaration-files-in-the-implementation-of-typescript-3a0108633c10?source=collection_archive---------4----------------------->

## 在本文中，我们将研究如何增强 JavaScript 库并通过声明文件添加语法糖。

![](img/0793dfc098803bc09f26ef9f9d231472.png)

**JavaScript** 开发最吸引人的一个方面是已经发布的、经过测试的、可供重用的大量外部 JavaScript 库。

> 本文摘自 Nathan Rozentals 所著的《掌握打字稿，第四版 》一书，该书是理解打字稿语言及其最新特性的综合指南。

# 申报文件

声明文件是 TypeScript 编译器使用的一种特殊类型的文件。它只在编译阶段使用，作为一种参考文件来描述 JavaScript。声明文件类似于 [C](/javarevisited/10-best-c-programming-courses-for-beginners-2c2c1f6bcb12) 或者 [C++](/javarevisited/top-10-courses-to-learn-c-for-beginners-best-and-free-4afc262a544e) 中使用的头文件或者 [Java](/javarevisited/top-5-java-online-courses-for-beginners-best-of-lot-1e1e240a758) 中使用的接口。它们只是描述了可用函数和属性的结构，但没有提供实现。在本章的这一节，我们将看看这些声明文件，它们是什么，以及如何编写它们。

# 声明文件类型

我们知道，声明文件使用*声明*和*模块*关键字来定义对象和名称空间。我们还看到，为了定义变量的自定义类型，我们可以像在 [TypeScript](/javarevisited/top-10-free-typescript-courses-to-learn-online-best-of-lot-44bce9da41d1) 中一样使用接口。声明文件允许我们使用与在 TypeScript 中相同的语法来描述类型。这些类型可以在普通 TypeScript 中使用类型的任何地方使用，包括函数重载、类型联合、类和可选属性。让我们快速地看一下这些技术，用几个简单的代码示例来进一步说明这个特性。本节将涵盖:

*   函数重载
*   嵌套命名空间
*   班级
*   静态属性和函数
*   抽象类
*   无商标消费品
*   条件类型和推理

我们将从函数重载开始。

# 函数重载

声明文件允许函数重载，其中同一函数可以用不同的参数声明，如下所示:

```
declare function trace(arg: string | number | boolean);
declare function trace(arg: { id: number; name: string });
```

这里，我们有一个名为 trace 的函数，它被声明了两次:一次是用一个名为 arg 的参数，类型为 string、number 或 boolean，另一次是用同一个 arg 参数，这是一个自定义类型。该重载声明允许以下所有有效代码:

```
trace(“trace with string”);
trace(true);
trace(1);
trace({ id: 1, name: “test” });
```

这里，我们练习了函数覆盖所允许的各种参数组合。

# 嵌套命名空间

声明文件允许嵌套模块名。这又转化为嵌套的名称空间，如下所示:

```
declare module FirstNamespace {
   module SecondNamespace {
      module ThirdNamespace {
          function log(msg: string);
      }
   }
}
```

这里，我们声明了一个名为`FirstNamespace`的模块，它公开了一个名为`SecondNamespace`的模块，后者又公开了第三个名为`ThirdNamespace`的模块。`ThirdNamespace`模块定义了一个名为 log 的函数。该声明将导致需要引用所有三个名称空间来调用日志函数，如下所示:

```
FirstNamespace.SecondNamespace.ThirdNamespace.log(“test”);
```

这里，我们显式引用每个命名空间，以便调用`ThirdNamespace`模块中的`log`函数。

# 班级

类定义是在模块定义中使用 Class 关键字指定的，就像在普通的 TypeScript 文件中一样，如下所示:

```
declare class MyModuleClass {
   public print(): void;
}
```

这里，我们声明了一个名为`MyModuleClass`的类，它有一个返回`void`的公共`print`函数。该类定义可以如下使用:

```
let myClass = new MyModuleClass();
myClass.print();
```

这里，我们正在创建一个已经在声明文件中声明的`MyModuleClass`类的实例。然后我们调用名为`myClass`的类实例的`print`函数。

在声明文件中声明一个类非常类似于为它定义一个接口。我们不提供函数的任何实现，我们只是向 TypeScript 编译器声明该类的存在以及它可以使用哪些属性和函数。

# 静态属性和函数

与我们可以在[类型脚本](/javarevisited/7-best-courses-to-learn-typescript-in-depth-58439e1ce729)中将属性或函数标记为静态的方式相同，我们可以在声明文件中使用相同的语法，如下所示:

```
declare class MyModuleStatic {
    static print(): void;
    static id: number;
}
```

这里，我们声明了一个名为`MyModuleStatic`的类，它有一个静态`print`函数和一个静态`id`属性。我们可以如下使用这些静态属性和函数:

```
MyModuleStatic.id = 10;
MyModuleStatic.print();
```

这里，我们可以看到静态函数或属性的声明遵循相同的使用规则，就好像我们已经在[类型脚本](https://javarevisited.blogspot.com/2018/07/top-5-courses-to-learn-typescript.html#axzz5QyVwWVg3)中定义了它一样。

# 抽象类

声明文件可以定义[抽象类](https://javarevisited.blogspot.com/2013/05/difference-between-abstract-class-vs-interface-java-when-prefer-over-design-oops.html)和函数如下:

```
declare abstract class MyModuleAbstract {
    abstract print(): void
}
```

这里，我们定义了一个名为`MyModuleAbstract`的抽象类，它有一个名为`print`的函数，这个函数也被标记为抽象的。我们可以在 TypeScript 文件中使用这个抽象类声明，如下所示:

```
class DerivedFromAbstract extends MyModuleAbstract {
    print() { }
}
```

这里，我们定义了一个名为`DerivedFromAbstract`的类，它扩展了声明文件中的`MyModuleAbstract`类。

> 注意，我们还需要在这个类定义中提供一个`print`函数的实现，因为 print 函数在类声明中被标记为抽象函数。

# 无商标消费品

声明文件允许使用通用语法，如下所示:

```
declare function sort<T extends number | string>
    (input: Array<T>): Array<T> { }
```

这里，我们声明了一个名为`sort`的函数，它使用通用语法将`T`的类型指定为`number`或`string`，或者两者都是。这个`sort`函数有一个名为`input`的参数，它是一个类型为`T`的数组，并返回一个类型为`T`的数组。该声明将允许以下用法:

```
let sortedStringArray = sort([“first”, “second”]);
let sortedNumericArray = sort([1, 2, 3]);
```

这里，我们定义了一个名为`sortedStringArray`的变量来保存对排序函数的调用的返回值，在这里我们将一个字符串数组作为唯一的参数传入。`sortedStringArray`变量的类型将是`Array<string>`。接下来，我们定义一个名为`sortedNumericArray`的变量来保存对 sort 函数的另一个调用的返回值，在这里我们传递一个数字数组作为唯一的参数。`sortedNumericArray`变量将属于`Array<number>`类型。

# 条件类型

声明文件还可以定义条件类型和分布式条件类型，如下所示:

```
declare type stringOrNumberOrBoolean<T> =
    T extends string ? string :
    T extends number ? number :
    T extends boolean ? boolean : never;
```

这里，我们声明了一个名为`stringOrNumberOrBoolean`的类型，它使用通用语法定义了一个类型`T`。如果`T`扩展了`string`，那么类型将是`string`。如果`T`扩展了`number`，那么类型就是`number`。如果`T`的类型扩展了`boolean`，那么类型就是`boolean`。如果`T`没有扩展这些类型中的任何一个，那么类型将是 never。

我们现在可以使用这种分布式条件类型，如下所示:

```
type myNever = stringOrNumberOrBoolean<[string,number]>;
```

这里，我们定义了一个名为`myNever`的类型，它是名为`stringOrNumberOrBoolean`的分布式条件类型的结果，并且定义了一个类型`T`作为`string`和`number`的元组。由于这个元组与我们正在检查的任何类型都不匹配，`myNever`变量的类型将是 never。

# 条件类型推理

声明文件允许条件类型推断，如以下示例所示:

```
declare type inferFromPropertyType<T> =
    T extends { id: infer U } ? U : never;
```

这里，我们有一个名为`inferFromPropertyType`的类型，它将从类型`T`的名为`id`的属性中推断出名为`U`的类型。如果`id`属性不存在，则类型为`never`。我们现在可以如下使用这种类型:

```
type myString = inferFromPropertyType<{ id: string }>;
type myNumber = inferFromPropertyType<{ id: number }>;
```

这里，我们定义了两个类型，名为`myString`和`myNumber`，它们使用了名为`inferFromPropertyType`的推断条件类型。由于`id`属性的类型是第一行中的`string`，那么`myString`将是类型`string`。第二行代码中的`id`属性的类型是`number`，因此`myNumber`将是`number`类型。

# 申报文件摘要

我们在本文中看到的是，TypeScript 提供的类型规则和类型技术都可以在声明文件中使用。声明文件的目的是提前告诉 TypeScript 编译器 JavaScript 库的结构是什么样子。我们已经看到，我们可以在声明文件中使用所有的 TypeScript 关键字和语言特性。

# 关于作者

**Nathan Rozentals** 已经用 [C](/javarevisited/9-free-c-programming-courses-for-beginners-2486dff74065) 、 [C++](/javarevisited/10-best-c-and-c-programming-books-for-beginners-and-experienced-programmers-eb5ee8dbdc5a) 、 [Java](/javarevisited/10-free-courses-to-learn-java-in-2019-22d1f33a3915) 和 [C#](/javarevisited/9-free-c-c-sharp-courses-and-tutorials-for-beginners-and-intermediate-programmers-best-of-lot-dc8c793aab31?source=---------16------------------) 编写商业软件超过 30 年。他在 2012 年 10 月首次发布 TypeScript 后的一周内就开始使用它，并意识到在编写 JavaScript 时 TypeScript 可以提供多大的帮助。

Nathan 的 TypeScript 解决方案现在控制物联网设备中的用户界面，作为销售点解决方案的独立应用程序运行，提供复杂的应用程序配置网站，并用于任务关键型服务器 API。

> 本文摘自 Nathan Rozentals 所著的《掌握 TypeScript，第四版 》一书，该书是理解 TypeScript 语言及其最新特性的综合指南。如果你喜欢这篇文章，那么你也会喜欢这本书。