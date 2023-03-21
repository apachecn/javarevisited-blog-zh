# 回到 Java 基础——第 1 部分:类路径

> 原文：<https://medium.com/javarevisited/back-to-the-basics-of-java-part-1-classpath-47cf3f834ff?source=collection_archive---------0----------------------->

![](img/9977aaf1f7269a97f89273f72c2f96b7.png)

我最喜欢的编程语言一直是 Java，巧合的是这也是我用过的第一种语言。如果你和我一样，或者只是想了解更多关于令人敬畏的语言和技术 Java 的知识，那么这个由两部分组成的系列就是为你准备的。

在这些简短的系列中，您将了解以下内容。

*   第 1 部分:类路径
*   第二部分:罐子

那么，我为什么决定写这些文章呢？嗯，Java 有一些方面我没有完全理解(比如类路径)，我意识到这是因为我一直在使用 [IDE](/javarevisited/7-best-courses-to-learn-intellij-idea-for-beginners-and-experienced-java-programmers-2e9aa9bb0c05) 和[构建工具](/javarevisited/5-best-gradle-courses-and-books-to-learn-in-2021-93f49ce8ff8e)。

IDE 的问题是它隐藏了很多东西，这阻碍了全面的理解。换句话说，我认为是时候更熟悉 java cli 工具并回归基础了。

![](img/01fc0f5b9b6c15aba63e50f5bed6eeb7.png)

现在让我们言归正传。在这一部分中，我将解释[什么是**类路径**以及如何正确(和不正确地)使用它。](http://www.java67.com/2012/08/what-is-path-and-classpath-in-java-difference.html)

# 类路径

类路径只是一个目录、JAR 文件和 ZIP 存档的列表，用来搜索类文件[1]。运行时需要知道在哪里可以找到你编译的类，所以这就是你在这里提供的。

关于如何指定路径，您应该注意一些事情。为了解释这一点，我将创建一个虚拟项目，结构见下文。

```
ClasspathProject/src/java/myprogram/
    |
    ---> Main.java
    |
    ---> utils/
        |
        ---> Util.java
```

这是源文件，让你知道我们在处理什么。

**Main.java**

**Util.java**

我将在接下来的部分使用这个结构，所以如果你想继续，我建议你在继续之前创建它。

## 关于包装的简短说明。

如果您是 java 新手，了解包通常被认为是文件结构的镜像会很有帮助。以 Utils.java 文件为例，它有 myprogram.utils 包，因为它在那个文件夹中。

编译器并不真正需要所提供的结构。但是，java 命令要求它运行。

让我们编译这个小项目。

首先我们创建一个 *bin* 文件夹来保存根项目文件夹中的编译内容，然后编译这两个文件[2]。

```
> mkdir bin
> javac src/java/myprogram/Main.java src/java/myprogram/utils/Util.java -d bin/
```

您现在应该有以下文件结构。

```
ClasspathProject/
    |
    ---> src/java/myprogram/
        |
        ---> Main.java
        |
        ---> utils/
            |
            ---> Util.java
    |    
    ---> bin/myprogram/
        |
        ---> Main.class
        |
        ---> utils/
            |
            ---> Util.class
```

如果我们现在移动到 *bin* 文件夹，我们可以运行主类。

```
> java myprogram.Main
Here is 1337
```

太好了，那为什么会这样呢？因为类路径的**缺省值**是**当前目录**。它需要找到两个类文件；Main.class 和 Utils.class，看起来怎么样，在哪里？给定当前的类路径 *bin* 和我们的主类 myprogram。Main 它将在这个目录中查找并尝试找到以下内容:

*   myprogram/Main.class
*   myprogram/utils/Util.class

我们知道它存在，所以程序运行成功。

现在让我们稍微改变一下。让我们**添加**一个新的文件夹 *lib* 在与 *bin* 相同的层次上，并移动 Utils.class，使其看起来像这样。

```
ClasspathProject/
    |
    ---> src/java/myprogram/
        |
        ---> Main.java
        |
        ---> utils/
            |
            ---> Util.java
    |    
    ---> bin/myprogram/
        |
        ---> Main.class
    |
    ---> lib/myprogram/
        |
        ---> utils/
            |
            ---> Util.class
```

## 错误 1-未指定类路径

如果我们现在移动到 *bin* 文件夹并尝试运行相同的命令，我们将得到一个错误，因为它找不到 Util.class 文件。

```
> java myprogram.Main
Exception in thread "main" java.lang.NoClassDefFoundError: myprogram/utils/Util
    at myprogram.Main.main(Main.java:8)
Caused by: java.lang.ClassNotFoundException: myprogram.utils.Util
    at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:602)
    at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:178)
    at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:521)
    ... 1 more
```

## 错误 2-将类文件添加到类路径中

我们将尝试通过将类文件添加到[类路径](https://javarevisited.blogspot.com/2011/01/how-classpath-work-in-java.html#axzz6uq12fuKh)来解决这个问题(我们从 *bin* 文件夹中运行所有这些命令)。

```
> java -classpath ../lib/myprogram/utils/Util.class myprogram.Main
Error: Could not find or load main class myprogram.Main
Caused by: java.lang.ClassNotFoundException: myprogram.Main
```

哎哟，现在它找不到 Main.class 文件了？因为如前所述，它将尝试在类路径中提供的指定目录中查找文件。我们没有提供目录。所以让我们改做那个吧。

## 错误 3-将一个目录添加到类路径中

```
> java -classpath ../lib/myprogram/utils/ myprogram.Main
Error: Could not find or load main class myprogram.Main
Caused by: java.lang.ClassNotFoundException: myprogram.Main
```

好吧，同样的错误。这里发生了什么？因为我们提供了一个 classpath 参数，它覆盖了默认的 class path 参数(当前目录),所以它在任何地方都找不到我们的 Main.class 文件。我们来补充一下。*在 Unix 系统上用冒号(:)分隔，在 Windows 上用分号(；).*

## 错误 4-向类路径添加更多路径

```
> java -classpath ../lib/myprogram/utils/:. myprogram.Main
Exception in thread "main" java.lang.NoClassDefFoundError: myprogram/utils/Util
    at myprogram.Main.main(Main.java:8)
Caused by: java.lang.ClassNotFoundException: myprogram.utils.Util
    at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:602)
    at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:178)
    at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:521)
    ... 1 more
```

好了，现在它似乎可以找到 Main.class 文件了。但是它**还是找不到**的 Util.class 文件。如前所述，它将尝试使用任何一个[类路径](https://javarevisited.blogspot.com/2012/10/5-ways-to-add-multiple-jar-to-classpath-java.html)来查找以下文件。

*   myprogram/Main.class
*   myprogram/utils/Util.class

在这种情况下，我们有两个类路径。

*   ../lib/myprogram/utils/
*   .

简而言之，它会尝试将类路径添加到所需的文件中，看看它们是否存在，我想这个过程应该是这样的。

```
Cp1 = ../lib/myprogram/utils/
Cp2 = ./
File1 = myprogram/Main.class
File2 = myprogram/utils/Util.classTrying to find File1Cp1 + File1 = ../lib/myprogram/utils/myprogram/Main.class
NOT FOUND
Cp2 + File1 = ./myprogram/Main.class
FOUNDTrying to find File2Cp1 + File2 = ../lib/myprogram/utils/myprogram/utils/Util.class
NOT FOUND
Cp2 + File2 = ./myprogram/utils/Util.class
NOT FOUND---> ERROR
```

## 最终解决

我们可以通过在 *lib* 下添加这些子文件夹来解决这个问题吗？我们当然可以，这是可行的。但是，我们不添加这些文件夹，而是修改 classpath 参数，使其看起来像这样。

```
> java -classpath ../lib/:. myprogram.Main
Here is 1337
```

这样做是因为它现在能够使用第一个路径找到 Utils.class 文件。

```
../lib/ + myprogram/utils/Util.class
FOUND
```

我希望这里的一些错误对你理解类路径有所帮助。似乎很多教程只给出了正确的做事方式，但我发现看到不正确的做事方式对我的学习有很大帮助。

[](https://ludvigwesterdahl.medium.com/back-to-the-basics-of-java-part-2-the-jar-6e923685d571) [## 回到 Java 基础——第 2 部分:JAR

### 在第一部分中，我介绍了一个简单的例子，解释了如何正确使用类路径。我将用这个例子…

ludvigwesterdahl.medium.com](https://ludvigwesterdahl.medium.com/back-to-the-basics-of-java-part-2-the-jar-6e923685d571) 

**参考文献**

[1] java 选项
[https://docs . Oracle . com/javase/7/docs/technotes/tools/windows/Java . html](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

[2] javac — Java 编程语言编译器
[https://docs . Oracle . com/javase/7/docs/technotes/tools/windows/javac . html](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/javac.html)