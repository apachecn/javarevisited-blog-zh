# Java -可运行与可调用

> 原文：<https://medium.com/javarevisited/java-runnable-vs-callable-786aa706775d?source=collection_archive---------0----------------------->

在本文中，我将讨论两个多线程概念，runnable 和 callable。

![](img/c459ee1e69c4847147359934e655ac09.png)

## 1-什么是可运行的？

Runnable 是一个接口，实现它的类将在线程中执行。在这里，您可以看到 Runnable 接口。所有需要在线程中执行的逻辑都在被覆盖的 run 方法中。你会注意到它是一个 void 方法。

```
@FunctionalInterface
public interface Runnable {public abstract void run();
}
```

## 2-什么是可赎回的？

我写的关于 [Runnable](http://javarevisited.blogspot.sg/2012/01/difference-thread-vs-runnable-interface.html#axzz59pNoKNHl) 的所有内容对 [Callable](https://javarevisited.blogspot.com/2016/08/useful-difference-between-callable-and-Runnable-in-Java.html#axzz6e8hmwujv) 接口都有效，除了一点，**返回类型。**方法*调用*完成执行后会返回任意类型。

```
@FunctionalInterface
public interface Callable<V> {V call() throws Exception;
}
```

## Runnable 和 Callable 的区别是什么？

正如我们之前谈到的，这两个接口的主要区别在于 Callable 接口的 call 方法会返回值。当您希望跟踪线程的结果时，这在许多情况下会很有用。我们来看看以下场景。

在下面的例子中，我们实现了 Runnable 接口，并在 [run 方法](https://javarevisited.blogspot.com/2012/03/difference-between-start-and-run-method.html#axzz6vPUwyVzv)中监听消息。因为不需要返回任何东西，所以在这里使用 Runnable 接口很方便。

```
public class RunnableClass implements Runnable {
    private MessageListener messageListener;

    public RunnableClass(MessageListener messageListener) {
        this.messageListener = messageListener;
    }

    @Override
    public void run() {
        messageListener.subscribe(message -> {
           // handle message 
        });
    }
}
```

下面，你可以看到一个[可调用接口](http://www.java67.com/2013/01/difference-between-callable-and-runnable-java.html)的例子。我们将原始字符串作为构造函数参数和内部调用方法，我们将它传递给 StringValidator 的 isValid 方法，其内部实现现在并不重要(考虑它是一个耗时的字符串验证器)。在这种情况下，我们可能需要知道验证是否成功完成，所以我们使用 implemented Callable 来了解它。

```
public class CallableClass implements Callable<Boolean> {
    private String raw;

    public CallableClass(String raw) {
        this.raw = raw;
    }

    @Override
    public Boolean call() {
        return StringValidator.isValid(raw);
    }
}
```

还有另一个重要的区别，我们处理异常的方式。正如您在前两个片段中看到的，run 方法不会抛出任何东西，而 call 方法会抛出 Exception。所以，这意味着我们可以在调用方法中传播任何异常，但是我们必须在运行方法中处理所有异常。

最后，我们将看看线程是如何执行它们的。对于 runnables，

```
ExecutorService executorService = Executors.newSingleThreadExecutor(); // or any other executor
Runnable r = new RunnableClass(new MessageListener());// you can pick one of the following
new Thread(r).start();
CompletableFuture.runAsync(r);
executorService.execute(r);
```

对于可调用的，

```
ExecutorService executorService = Executors.*newSingleThreadExecutor*();
Callable<Boolean> callable = new CallableClass("sometext");Future<Boolean> result = executorService.submit(callable);
System.out.println(result.get().booleanValue()); // result future will return result when ready, this will also throws InterruptedException, ExecutionException but omitted here for clean sight. 
```

在本教程中，我谈到了什么是 Runnable 和 Callable 接口，以及它们之间的区别。我希望你喜欢它。

[](https://github.com/kurular4/medium-java/tree/master/runnable-callable) [## medium-Java/runnable-可在主 kurula 4/medium-Java 上调用

### 关于 Java 的文章中的代码。在 GitHub 上创建一个帐户，为 kurura 4/medium-Java 开发做贡献。

github.com](https://github.com/kurular4/medium-java/tree/master/runnable-callable)