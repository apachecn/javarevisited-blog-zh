# 使用 Maven 在优步 JAR 中运行 Groovy

> 原文：<https://medium.com/javarevisited/run-groovy-within-uber-jar-using-maven-646e9864b0a4?source=collection_archive---------3----------------------->

## 创建一个混合了 Groovy 和 Java 代码的可执行 JAR

![](img/a5e775f8d266cee0eb3ee31e01a2efc6.png)

由[维达尔·诺德里-马西森](https://unsplash.com/@vidarnm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

从命令行运行 Groovy 脚本相对简单:惟一的先决条件是安装了 Groovy 二进制文件，然后可以使用运行任何 Groovy 脚本。甚至依赖项管理也很容易——只需用@Grape 和 [Groovy 的 Grape](http://docs.groovy-lang.org/latest/html/documentation/grape.html) 机制为您完成完整的依赖项检索过程。

但是，有时事情会更复杂，您可能会混合使用 Java 和 Groovy 代码、一些外部依赖项，甚至可能是资源文件。在这种情况下，上述方法将不起作用，您可能需要求助于使用所谓的“uber JAR ”,通过构建一个可执行文件，在一个包中包含您需要的所有内容，从而使您的部署更加容易。

# 优步罐子

超级 JAR 是一个普通术语，指稍微升级的 JAR，它不仅包含您的代码，还包含您的代码所依赖的所有依赖项，所有这些都打包在一个 JAR 文件中。这是打包应用程序的一种非常方便的方式，只需使用几个命令就可以轻松部署和运行。

例如，一旦您将您的 uber JAR 文件复制到目标位置，运行您的应用程序就变得非常容易:

```
java -jar uber.jar
```

这很简单，不是吗？

一些框架，如 [Spring Boot](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e) 不费吹灰之力就能给你超级罐子。然而，如果您有一个普通的旧 Java 应用程序，事情就不再那么简单了。

本指南将向您展示当您拥有一个包含多个依赖项的项目时，如何创建 uber JARs。为了使事情稍微复杂一点，让我们假设一个 Groovy 文件而不是 Java 文件包含您的 main 方法。

# 编译 Groovy 代码

默认情况下，Maven 将只编译存储在`src/main/java`下的 Java 代码。不过，通常情况下，那里不会有任何 Groovy 代码。您将把您的 [Groovy](https://javarevisited.blogspot.com/2017/08/top-5-books-to-learn-groovy-for-java.html) 存储在`src/main/groovy`中。

要编译 Groovy 代码，第一步是添加 Groovy 依赖项，然后指示 [Maven](/javarevisited/6-best-maven-courses-for-beginners-in-2020-23ea3cba89) 在哪里找到 Groovy 类，以便它可以编译它们。您还需要包含 Groovy 编译器插件:

启动 POM 文件来编译 Groovy 代码

现在，让我们用 main 方法创建一个 Groovy 文件。为简单起见，我们将它直接存储在根包中:

带有 main 方法的 Groovy 脚本示例

# 组装 JAR 文件

为了创建实际的 JAR 文件，我们将使用 [Maven 汇编插件](http://maven.apache.org/plugins/maven-assembly-plugin/)。只需几个配置设置，这个插件就允许您创建 uber JARs，其中不仅包括您的 Java/Groovy 代码，还包括您的代码所依赖的所有依赖项(以及它们的依赖项——完整的树)。

添加 Maven 汇编插件后，POM 文件将如下所示:

完整的 POM 文件，带有 Maven 汇编插件

这里有两点需要注意。首先，在`mainClass`元素中引用了主类:

```
<mainClass>Main</mainClass>
```

如果`Main`类属于一个包，那么您需要在它前面加上包名。比如:`a.b.c.Main`

第二个有趣的元素是`finalName`元素。该元素指定组装的 JAR 的名称。在我们的例子中，这个[罐子](https://javarevisited.blogspot.com/2017/06/how-to-get-jar-files-of-jackson-libary-for-JSON.html)将被命名为`uber.jar`。

# 包装和运行

就是这样。现在，在运行 JAR 之前，您需要通过运行`mvn package`来创建它，JAR 将在 Maven 包阶段创建。一旦有了 JAR，就可以将它部署到目标环境中，并使用 Java 二进制文件及其`-jar`标志来运行它:

```
java -jar uber.jar
```

感谢阅读！我希望你觉得这个指南有用。Maven Assembly Plugin 只是允许您创建可执行 jar 的插件之一。另一个流行的插件是 Maven Shade 插件。两个插件都很好，尽管它们的配置有很大不同。由你决定哪一个更适合你的用例。