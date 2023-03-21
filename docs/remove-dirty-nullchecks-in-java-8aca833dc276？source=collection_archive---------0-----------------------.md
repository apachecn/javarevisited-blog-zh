# 移除 Java 中的脏空值检查

> 原文：<https://medium.com/javarevisited/remove-dirty-nullchecks-in-java-8aca833dc276?source=collection_archive---------0----------------------->

[![](img/0f820eca2cb0a715aebd4b1239c7322d.png)](https://javarevisited.blogspot.com/2012/06/common-cause-of-javalangnullpointerexce.html)

当我用 Java 编写代码时，我总是担心 NullPointerException。

作为开发人员，我应该提供没有错误的代码。

对于一些项目，我使用过 javascript，我在 Javascript 中看到了一个美丽/惊人的东西。也就是说，我们可以检查执行一些函数，如果值不为空，否则，它不会执行。

```
let name = user ?. name;
```

这个？。在技术上等同于

```
(user !== null || user !== undefined)             ? user.name             : undefined;
```

## 我们不能用 Java 来做这个吗？

我们可以创建一些通用方法如下。

```
public static <I, O> O executeViaNullSafer(I value, Function<I, O> executorFunction) {
     return value != null ? executorFunction.apply(value) : null;   
}
```

***该函数接收值和一个可执行函数。如果该值不为空，函数将执行，可执行函数的输出将从该方法返回，否则该方法将返回空。***

这是一个像 javascript 一样的单行方法？。接线员。

## 怎么会？

例如，如果我需要修剪一些字符串类型的变量。显然，我们需要在修剪之前检查变量是否为空，因为如果变量为空，那么它将提供一个 [NullPointerException](https://www.java67.com/2021/05/how-to-solve-nullpointerexception-in-java.html) 。

**正常代码**

```
public String trimValue(String value){
 if(value==null){
   return null;
}else{
   return value.trim();
} 
```

我们可以用现代 java 改进这段代码，如下所示。

```
public String trimValue(String value){
 return value==null) ? null  : value.trim();
```

**让我们试试我们的 executeViaNullSafer 方法**

```
String trimmedValue = executeViaNullSafer(value, val -> val.trim());
```

我们也可以使用方法引用

```
String trimmedValue = executeViaNullSafer(value,String::trim);
```

让我们来看看 javascript 的例子

```
String name = executeViaNullSafer(user, User::getName);
```

这就是我在 java 应用程序中减少这些空检查的方法。我也发布了 NullUtil maven 依赖项。

[https://search . maven . org/artifact/io . github . rcvaram/NullUtil/1.0/ja](https://search.maven.org/artifact/io.github.rcvaram/NullUtil/1.0/ja)r

```
<dependency>
   <groupId>io.github.rcvaram</groupId
   <artifactId>NullUtil</artifactId>
   <version>1.0</version>
</dependency>
```

Github 网址→[https://github.com/rcvaram/NullUtil](https://github.com/rcvaram/NullUtil)

这种依赖还为你做了什么。

*   **nullutil . executevianullsafer**方法将对非空值执行给定函数，当值为空时，它将返回空值，而不是执行函数。
*   方法将对非空值执行给定的函数，如果值为空，它将返回给定的默认值。
*   **NullUtil.executeMutator 方法**对引用类型值进行变异，不返回任何内容。如果该值为 null，它将不会执行任何操作。
*   **NullUtil.replaceNull** 方法会用给定的 defaultValue 替换空值。当值不为空时，它将返回值。
*   **NullUtil.hasText** 方法用于检查字符串值是否有任何一种文本。
*   **NullUtil.trimValue** 方法将被用来修剪字符串，而不会导致空指针异常。