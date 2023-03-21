# Java -排序线程执行

> 原文：<https://medium.com/javarevisited/java-ordering-thread-executions-8ac8bb764144?source=collection_archive---------1----------------------->

![](img/de259ffb1f88ba3e0d9fd8e20401d00f.png)

在这篇文章中，我将讨论如何给不同的线程排序，让它们按照我们希望的顺序运行。当然，有传统的方法来确保秩序，但我想给你展示一种优雅的方法，叫做 **CompletableFuture** ，它是在 [Java 8](/hackernoon/top-5-java-8-courses-to-learn-online-2db57d9dfb8d) 中引入的。

## **1-什么是 CompletableFuture？**

CompletableFuture 是一种新的执行线程的方式，正如你现在看到的，使用起来非常简单。让我们看看如何用它来执行一个线程。首先，我们创建一个可运行的类。

```
public class TextDownloader implements Runnable {

    @Override
    public void run() {
        // Download text from server
        System.*out*.println("I am downloading the file containing text to a directory");
        try {
            Thread.*sleep*(2000); // for simulating download wait time
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

在`TextDownloader` 课上，我正在模拟下载操作，这需要一些时间来完成。当然，我们希望在一个单独的线程中运行下载操作，如下所示。

```
CompletableFuture
        .*runAsync*(new TextDownloader());
```

看多轻松好看！现在让我们添加另一个部分，并处理下载的文本。当我们下载完文本(作为文件)后，我们将它保存到文件系统中的某个位置，以便以后处理。这是关键部分，我们需要确保下载阶段已经完成，以便我们可以在以后访问它进行处理。这是处理器类

```
public class TextProcessor implements Runnable {

    @Override
    public void run() {
        // Process downloaded text
        System.*out*.println("I am taking the text from the location and processing the text");
    }
}
```

我们按如下方式运行它们，以确保有序

```
CompletableFuture
        .*runAsync*(new TextDownloader())
        .thenRunAsync(new TextProcessor());
```

*然后 RunAsync* 方法确保通过 *runAsync* 执行的线程完成，然后运行给它的线程。在上面的例子中，我们假设第二个线程已经知道第一个[线程](https://javarevisited.blogspot.com/2014/07/top-50-java-multithreading-interview-questions-answers.html)将下载的文件放在文件系统中的什么地方。但是如果没有呢？让我们看看供应商如何处理它。

```
public class TextDownloader2 implements Supplier<String> {

    @Override
    public String get() {
        System.*out*.println("I am downloading the file containing text to a directory");
        try {
            Thread.*sleep*(2000); // for simulating download wait time
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "pathToDownloadedFile";
    }
}
```

上面，你可以看到一个文本下载器的更新版本。不同之处在于，现在它实现了一个供应商接口，该接口的 get 方法返回值。我们可以将返回值传递给处理器类，这样它就知道在哪里可以找到下载的文件。更新的文本处理器类现在正在实现消费者接口，以消费供应商提供的输出。

```
public class TextProcessor2 implements Consumer<String> {

    @Override
    public void accept(String s) {
        // Process downloaded text
        System.*out*.println("I am taking the text from the location and processing the text");
        System.*out*.println("Path: " + s);
    }
}
```

以及我们如何一起使用它们

```
CompletableFuture
        .*supplyAsync*(new TextDownloader2())
        .thenAccept(new TextProcessor2());
```

您也可以直接在*接受*中提供您的逻辑，如下所示

```
CompletableFuture
        .*supplyAsync*(new TextDownloader2())
        .thenAccept(path -> {
            // some logic
        });
```

CompletableFuture 类中有不同的方法可供您使用，例如*thenacceptsync*或*theapply*，但我现在不想让文章变得更长。

在本教程中，我谈到了排序线程执行。我希望你喜欢它。

[](https://github.com/kurular4/medium-java/tree/master/completable-future) [## 中级 java/completable-master kurula 4 的未来/中级 Java

### 关于 Java 的文章中的代码。在 GitHub 上创建一个帐户，为 kurura 4/medium-Java 开发做贡献。

github.com](https://github.com/kurular4/medium-java/tree/master/completable-future)