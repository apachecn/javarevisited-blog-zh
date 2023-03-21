# 回到 Java 基础——第 2 部分:JAR

> 原文：<https://medium.com/javarevisited/back-to-the-basics-of-java-part-2-the-jar-6e923685d571?source=collection_archive---------1----------------------->

![](img/a638ffdc58ba1e13e02b828142a2c77d.png)

在第一部分中，我介绍了一个简单的例子，解释了如何正确使用类路径。我也将在这一部分使用这个例子，所以如果您还没有创建它，您可以在继续之前从第一部分复制它，如果您想继续的话。

[](https://ludvigwesterdahl.medium.com/back-to-the-basics-of-java-part-1-classpath-47cf3f834ff) [## 回到 Java 基础——第 1 部分:类路径

### 我最喜欢的编程语言一直是 Java，巧合的是这也是我用过的第一种语言。如果…

ludvigwesterdahl.medium.com](https://ludvigwesterdahl.medium.com/back-to-the-basics-of-java-part-1-classpath-47cf3f834ff) 

# 冲突

![](img/bab3e70e6011163aa484ebdc8b8b5e00.png)

jar 文件只是一种用于打包 java 类文件和相关资源以供分发的归档文件。如果我们还记得上一篇文章，我们的文件结构是这样的。

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

让我们[创建一个包含类文件的可执行 jar](https://javarevisited.blogspot.com/2012/03/how-to-create-and-execute-jar-file-in.html) 。首先，我们导航到根目录。

## 第一次尝试——混乱的方式

然后我们运行这个命令[1]。

```
> jar --create --file project.jar bin/myprogram/Main.class bin/myprogram/utils/Util.class
```

*   ***-创建(或-c)*** :创建一个新的档案
*   ***- file(或-f)*** :存档的文件名
*   *:要包含在存档中的文件*

*让我们运行它。*

```
*> java -jar project.jar
no main manifest attribute, in project.jar*
```

*它似乎找不到清单[。所以我们来考察一下里面装的是什么。这里有三个命令，它们给出了完全相同的输出，所以我将展示所有这些命令，但从现在开始将使用最后一个命令。](https://javarevisited.blogspot.com/2013/01/how-to-fix-failed-to-load-main-class-manifest-attribute-jar-java-eclipse.html)*

```
*> jar --list --file project.jar (alternative 1)
> jar -t -f project.jar         (alternative 2)
> jar -tf project.jar           (alternative 3)
META-INF/
META-INF/MANIFEST.MF
bin/myprogram/Main.class
bin/myprogram/utils/Util.class*
```

*   ****- list (or -t)*** :列出一个罐子的内容*
*   ****- file(或-f)***:jar 文件*

*看起来不太好。我们来看看货单上有什么。*

```
*> jar --extract --file project.jar (alternative 1)
> jar -xf project.jar              (alternative 2)*
```

*   ****-extract(or-x)***:从广口瓶中提取内容物*

*然后打印清单。*

```
*> cat META-INF/MANIFEST.MF
Manifest-Version: 1.0
Created-By: 13.0.2 (Oracle Corporation)*
```

*这是可修复的吗？当然，我会很快显示修复。最基本的解决方法是用类路径更新清单。*

```
*> echo "Class-Path: bin/" > manifest.txt
> jar --update --file project.jar --manifest manifest.txt*
```

*   ****-update(or-u)***:更新一个 jar 文件*
*   ****- manifest(或-m)*** :要包含信息的清单文件*

*我们再来一遍。*

```
*> java -jar project.jar
no main manifest attribute, in project.jar*
```

*好吧，同样的问题，因为它不是一个可执行的 jar，jar 清单中没有主类。然而，我们可以通过在类路径中指定 jar 来运行它。*

```
*> java -cp project.jar myprogram.Main
Here is 1337*
```

*让我们通过添加一个主类属性来更新我们的清单文件，并更新 jar。*

```
*> echo "Main-Class: myprogram.Main" >> manifest.txt
> jar -u -f project.jar -m manifest.txt
Aug 30, 2021 11:52:26 AM java.util.jar.Attributes read
WARNING: Duplicate name in Manifest: Class-Path.
Ensure that the manifest does not have duplicate entries, and that blank lines separate individual sections in both your manifest and in the META-INF/MANIFEST.MF entry in the jar file.*
```

*您可以忽略此警告，因为如果我们检查清单。MF 文件如上所示，你会看到它看起来很好。*

```
*> jar -xf project.jar
> cat META-INF/MANIFEST.MF
Manifest-Version: 1.0
Created-By: 13.0.2 (Oracle Corporation)
Class-Path: bin/
Main-Class: myprogram.Main*
```

*但是如果您愿意的话，您可以用一个包含 main-class 属性的新行覆盖 manifest.txt 文件，它将在清单中被更改。MF 文件放在 jar 中。*

*或者，您可以在命令行上指定 main-class 属性来更新 jar，这是一种快速更改属性的方法。*

```
*> jar -u -f project.jar --main-class myprogram.Main
> java -jar project.jar
Here is 1337*
```

*   ****-main-class(or-e)***:改变清单中的 Main-Class 属性*

*现在我们可以像平常一样运行它了。*

```
*> java -jar project.jar
Here is 1337*
```

*好吧，这很混乱。让我们试着把事情简单化。首先，让我们清除文件的目录以重新开始。*

```
*> rm -rf META-INF/ manifest.txt project.jar*
```

## *第二次尝试——更简单的方法*

*现在我试着让它更简洁一点。*

```
*> jar -c -e myprogram.Main -f project.jar -C bin/ myprogram/Main.class -C bin/ myprogram/utils/Util.class*
```

*   ***-C** :更改目录**，**包含提供的文件(2 个参数标志，注意空格)*

*我们来考察一下内容。*

```
*> jar -tf project.jar
META-INF/
META-INF/MANIFEST.MF
myprogram/Main.class
myprogram/utils/Util.class*
```

*已经在这里你可以看到它看起来好多了，没有任何地方的 *bin* 目录。我们应该可以直接运行这个。*

```
*> java -jar project.jar
Here is 1337*
```

*好的，很好。然而，我们实际上可以稍微简化一下命令。*

```
*> jar -cfe project.jar myprogram.Main -C bin/ .
> java -jar project.jar
Here is 1337*
```

*这里我指定了目录和一个通配符，使用了点(.)来获取 *bin* 目录中的所有内容。*

## *边注*

*这里参数的顺序很重要。例如，如果您这样做:*

```
*> jar -cfe myprogram.Main project.jar -C bin/ .*
```

*该命令会成功，但最终会得到一个名为 myprogram 的文件。主类属性为 project.jar 的 Main。*

```
*> ls
bin  lib  myprogram.Main src> java -jar myprogram.Main
Error: Could not find or load main class project.jar
Caused by: java.lang.ClassNotFoundException: project.jar*
```

## *图书馆罐子*

*对于这一部分，我将把 Util.class 移到它自己的 jar 文件中，类似于我在本系列第一部分解释[类路径](https://javarevisited.blogspot.com/2011/01/how-classpath-work-in-java.html#axzz6uq12fuKh)时所做的。*

*因此新的文件结构将如下所示。*

```
*ClasspathProject/
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
            ---> Util.class*
```

*我们将运行以下命令来生成我们的 jar 文件。*

```
*> jar -cfe project.jar myprogram.Main -C bin/ .
> jar -cf projectutils.jar -C lib/ .*
```

*现在，如果我们尝试运行 project.jar，我们显然会得到一个错误，因为它找不到 Util 类。*

```
*> java -jar project.jar
Exception in thread "main" java.lang.NoClassDefFoundError: myprogram/utils/Util
    at myprogram.Main.main(Main.java:8)
Caused by: java.lang.ClassNotFoundException: myprogram.utils.Util
    at   java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:602)
    at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:178)
    at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:521)
... 1 more*
```

*[![](img/fe4db3b8e85a491bc27e55ba37df2fe0.png)](https://javarevisited.blogspot.com/2018/02/how-to-fix-exception-in-thread-main.html)*

*那么我们该怎么做呢？如果我们还记得关于类路径的文章，我们可以像这样在类路径上指定 jar。*

```
*> java -cp project.jar:projectutils.jar myprogram.Main
Here is 1337*
```

*但是，如果我们想让 project.jar 成为一个可执行的 jar 文件，以便上面失败的命令能够工作，该怎么办呢？好吧，那我们需要像这样使用清单。*

```
*> echo "Main-Class: myprogram.Main" > manifest.txt
> echo "Class-Path: projectutils.jar" >> manifest.txt
> jar -cfm project.jar manifest.txt -C bin/ .
> java -jar project.jar
Here is 1337*
```

*注意，Class-Path 属性是创建的 jar 文件的相对路径。*

*我希望这篇类似学习过程的文章对您理解创建 jar 有所帮助。感谢您的阅读，如果有写错的地方或可以改进的地方，请给我一个赞或评论！*

***参考文献***

*[1]jar 命令
[https://docs . Oracle . com/en/Java/javase/16/docs/specs/man/jar . html](https://docs.oracle.com/en/java/javase/16/docs/specs/man/jar.html)*

*[2]使用清单文件:基础知识
[https://docs . Oracle . com/javase/tutorial/deployment/jar/Manifest index . html](https://docs.oracle.com/javase/tutorial/deployment/jar/manifestindex.html)*