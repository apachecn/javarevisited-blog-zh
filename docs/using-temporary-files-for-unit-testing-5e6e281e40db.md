# 如何在 Java 中使用临时文件进行单元测试

> 原文：<https://medium.com/javarevisited/using-temporary-files-for-unit-testing-5e6e281e40db?source=collection_archive---------0----------------------->

![](img/4be160ed0063f56e49ec9d6c6df2853d.png)

有时我们需要创建临时文件，并在单元测试中使用它们。通常，我喜欢这样。

```
@Test void testSome() {
    File file;
    try {
        file = File.createTempFile("tmp", null);
    } catch (IOException ioe) {
        throw new RuntimeException(ioe);
    }
    //file.deleteOnExit(); // not recommended at all
    try {
        // do some with the file
    } finally {
        if (file.isFile() && !file.delete()) {
            // failed to delete the (existing) file
        }
    }
}
```

# 效用函数

随着测试用例数量的增长，我们可以像这样使用实用方法。

```
static <R> applyTempFile(Function<File, R> function) {
    File file;
    try {
        file = File.createTempFile("tmp", null);
    } catch (IOException ioe) {
        throw new RuntimeException(ioe);
    }
    try {
        return function.apply(file);
    } finally {
        if (file.isFile() && !file.delete()) {
            // failed to delete the (existing) file
        }
    }
}static <R, U> applyTempFile(BiFunction<File, U, R> function,
                            Supplier<U> supplier) {
    return applyTempFile(f -> function.apply(f, supplier.get()));
}static void acceptTempFile(Consumer<File> consumer) {
    applyTempFile(f -> {
        consumer.accept(f);
        return null;
    });
}static <U> void acceptTempFile(BiConsumer<File, U> consumer,
                               Supplier<U> supplier) {
    acceptTempFile(f -> consumer.accept(f, supplier.get()));
}// TODO: add methods for java.nio.file.Path
```

# 参数解析器

另一种方法是使用一个`[ParameterResolver](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/extension/ParameterResolver.html)`。

```
class TemporaryFileParameterResolver implements ParameterResolver { [@Documented](http://twitter.com/Documented)
    [@Retention](http://twitter.com/Retention)(RetentionPolicy.RUNTIME)
    [@Target](http://twitter.com/Target)({ElementType.PARAMETER})
    [@interface](http://twitter.com/interface) Temporary {
        boolean deleteOnExit() default true;
    } [@Override](http://twitter.com/Override)
    public boolean supportsParameter(
            ParameterContext parameterContext,
            ExtensionContext extensionContext)
            throws ParameterResolutionException {
        if (parameterContext.isAnnotated(Temporary.class)) {
            Parameter parameter = parameterContext.getParameter();
            Class<?> parameterType = parameter.getType();
            return File.class == parameterType
                   || Path.class == parameterType;
        }
        return false;
    } [@Override](http://twitter.com/Override)
    public Object resolveParameter(
            ParameterContext parameterContext,
            ExtensionContext extensionContext)
            throws ParameterResolutionException {
        Parameter parameter = parameterContext.getParameter();
        Temporary temporary =
            parameter.getAnnotation(Temporary.class);
        Class<?> parameterType = parameter.getType();
        if (File.class == parameterType) {
            File file;
            try {
                file = File.createTempFile("tmp", null);
            } catch (final IOException ioe) {
                throw new ParameterResolutionException(ioe);
            }
            if (temporary.deleteOnExit()) {
                file.deleteOnExit();
            }
            return file;
        }
        if (Path.class == parameterType) {
            Path path;
            try {
                path = Files.createTempFile(null, null);
            } catch (final IOException ioe) {
                throw new ParameterResolutionException(
                    "failed to create a temporary file", ioe);
            }
            if (temporary.deleteOnExit()) {
                Runtime.getRuntime().addShutdownHook(
                    new Thread(() -> {
                        try {
                            Files.deleteIfExists(path);
                        } catch (final IOException ioe) {
                            throw new RuntimeException(ioe);
                        }
                    }));
            }
            return path;
        }
        throw new ParameterResolutionException(
            "failed to resolve parameter for " + parameterContext);
    }
}
```

现在我们可以像这样使用 resolver 类。

```
@ExtendWith({TemporaryFileParameterResolver.class})
class SomeTest { @Test void testWithFile(@Temporary File file) {
    } @Test void testWithPath(@Temporary Path path) {
    }
}
```

# 小心`deleteOnExit`和`addShutdownHook`

根据 JDK 的源代码，`deleteOnExit`将文件的路径添加到一个`Set`中，`addShutdownHook`将[线程](https://javarevisited.blogspot.com/2018/06/top-5-java-multithreading-and-concurrency-courses-experienced-programmers.html)添加到一个`Map`中，并在 [JVM](https://javarevisited.blogspot.com/2019/04/top-5-courses-to-learn-jvm-internals.html) 关闭时清理它们。

这意味着过多的文件/引用可能会留在存储/内存中，直到一个干净的关闭过程。

这就是为什么我首先在注释上添加了`deleteOnExit`元素。

```
@ExtendWith({TemporaryFileParameterResolver.class})
class SomeExcessiveFileTest { @RepeatedTest(1048576) 
    void test(@Temporary(deleteOnExit = false) File file) {
        try {
            // do some with the file
        } finally {
            if (file.isFile() && !file.delete()) {
            }
        }
    } @RepeatedTest(1048576)
    void test(@Temporary(deleteOnExit = false) Path path) {
        try {
            // do some with the path
        } finally {
            boolean deleted = Files.deleteIfExists(path);
        }
    }
}
```

这就是关于如何使用临时文件进行单元测试，以及如何在测试后进行清理，以便您的应用程序在新的测试中始终处于良好的状态。

如果你想学习更多关于 Java 单元测试的知识，这里有一些有用的资源。

1.  [**面向初学者的 Java 单元测试**](https://click.linksynergy.com/deeplink?id=JVFxdTr9V80&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fjunit5-for-beginners%2F)
2.  [**用 JUnit&mock ITO 30 步**](https://click.linksynergy.com/deeplink?id=JVFxdTr9V80&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fmockito-tutorial-with-junit-examples%2F) 学习单元测试
3.  [**Java 测试驱动开发实践**](https://pluralsight.pxf.io/c/1193463/424552/7490?u=https%3A%2F%2Fwww.pluralsight.com%2Fcourses%2Ftest-driven-development-java)
4.  [**Java 开发者应该知道的 10 个测试库**](https://javarevisited.blogspot.com/2018/01/10-unit-testing-and-integration-tools-for-java-programmers.html)
5.  [**程序员前 5 名 JUnit 和单元测试课程**](https://javarevisited.blogspot.com/2019/04/top-5-junit-and-unit-testing-courses-java-programmers.html)

# 结束语

感谢您阅读本文。对于 Java 程序员来说，这是一个非常有用技巧。如果你喜欢这篇文章，那么请考虑以下我们在媒体上发表的文章。

如果你想在每篇新帖子上得到通知，别忘了在 Twitter 上关注**[**javarevited**](https://twitter.com/javarevisited)！**