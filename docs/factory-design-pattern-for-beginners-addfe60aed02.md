# 面向初学者的工厂设计模式

> 原文：<https://medium.com/javarevisited/factory-design-pattern-for-beginners-addfe60aed02?source=collection_archive---------2----------------------->

![](img/4ca255270638a36ec6c59264117ec8b6.png)

安特·罗泽茨基在 [Unsplash](https://unsplash.com/s/photos/factory?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

工厂设计模式与生成器和单例设计模式一样，属于[设计模式](/javarevisited/7-best-online-courses-to-learn-object-oriented-design-pattern-in-java-749b6399af59)的创造性设计模式类别。

它为我们提供了一种机制，其中工厂方法有助于创建和实例化对象。在这种情况下，父类将保存对子类的引用。

换句话说，我们提供了 ***抽象*** ，其中对象创建隐藏在工厂方法中。我们只需传递指示需要创建哪个对象的参数，然后工厂会为我们返回已创建的对象。

对象的构造 ***与对象的*** 分离，这在遵循工厂设计模式的同时形成了额外的优势。

子类的公共实现可以保存在父类中，因此代码的可维护性也可以在这里实现。代码重复被删除，因此我们可以实现不重复自己(D.R.Y)原则。

让我们动手看看这个模式是如何实现的。

首先让我们创建一个包含语言类型的[枚举](https://javarevisited.blogspot.com/2011/08/enum-in-java-example-tutorial.html)。该枚举将具有语言 Id 和语言名称。

```
**public enum** LanguageType {
    ***GERMAN***(1, **"German"**),
    ***FRENCH***(2, **"French"**);

    **private int languageId**;
    **private** String **languageName**;

    LanguageType(**int** languageId, String languageName) {
        **this**.**languageId** = languageId;
        **this**.**languageName** = languageName;
    }
}
```

现在我们要求有一个 ***超*** 的类语言。超类可以是一个 [**抽象类，也可以是一个**](http://javarevisited.blogspot.sg/2013/05/difference-between-abstract-class-vs-interface-java-when-prefer-over-design-oops.html#axzz4pk4W5ie3) **接口。**

```
**public abstract class** Language {
    **public abstract** String greet();
}
```

现在，当超类或父类被创建时，让我们创建子类，它将扩展父类。这是德语课:

```
**public class** GermanLanguage **extends** Language{
    @Override
    **public** String greet() {
        **return "Guten Morgen"**;
    }
}
```

这就是法语课:

```
**public class** FrenchLanguage **extends** Language{
    @Override
    **public** String greet() {
        **return "Bonjour"**;
    }
}
```

现在父类和子类都已经就位，让我们看看如何创建工厂类。下面是 LanguageFactory 类:

```
**public class** LanguageFactory {
    **public static** Language getLanguage(**int** languageId) {
        **if** (languageId == ***GERMAN***.getLanguageId())
            **return new** GermanLanguage();
        **else if** (languageId == ***FRENCH***.getLanguageId())
            **return new** FrenchLanguage();
        **return null**;
    }
}
```

在这种情况下，我们匹配传递给 getLanguage()方法的 languageId，并相应地创建对象。

现在让我们看看这个实现是如何工作的。

```
**public class** LanguageImplementation {
    **public static void** main(String[] args) {
        Language germanLanguage = LanguageFactory.*getLanguage*(LanguageType.***GERMAN***.getLanguageId());
        Language frenchLanguage = LanguageFactory.*getLanguage*(LanguageType.***FRENCH***.getLanguageId());

        System.***out***.println(**"German Language Greeting : "** + germanLanguage.greet());
        System.***out***.println(**"French Language Greeting : "** + frenchLanguage.greet());
    }
}
```

我们在这里看到的输出是:

```
German Language Greeting : Guten Morgen
French Language Greeting : Bonjour
```

这就是我们的实现。

我们使用工厂设计模式实现的目标:

1.  将对象的创建与对象分离。
2.  抽象。
3.  遵循不重复自己的原则。

那就这样吧。如果您有任何问题，请在评论框中告诉我。