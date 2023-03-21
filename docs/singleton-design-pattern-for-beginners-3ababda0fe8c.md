# 适用于初学者的单例设计模式

> 原文：<https://medium.com/javarevisited/singleton-design-pattern-for-beginners-3ababda0fe8c?source=collection_archive---------3----------------------->

![](img/7b1c300dd2e6a3c858cf838141601302.png)

照片由[迪帕克·科斯塔](https://unsplash.com/@dkosta4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/architecture-indian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

Singleton 是 Java 中创造性的设计模式之一。创造性设计模式是那些用于创建对象的模式。

在单例设计模式的帮助下，我们可以将对象创建限制在一个实例中。一旦对象被实例化，同一个对象将在整个应用程序中被重用。

单体设计模式也是[访谈](/javarevisited/21-spring-mvc-rest-interview-questions-answers-for-beginners-and-experienced-developers-21ad3d4c9b82)的热门话题之一。从初学者到有经验的人，你应该会发现关于这种设计模式的问题。

# 履行

让我们看看[单例设计模式](https://www.java67.com/2020/05/5-ways-to-implement-singleton-design.html)是如何实现的。为了实现 Singleton，我们需要将类的构造函数设为私有，对象实例从静态`getInstance()`方法返回。

基本上有三种方法来实现单例设计模式。

## 急切实例化:

这里，对象是在声明的瞬间创建的。由于这种方法，内存被不必要地消耗了。从应用程序执行开始，内存就一直被占用，即使[单例对象](https://javarevisited.blogspot.com/2011/03/10-interview-questions-on-singleton.html)在后面的部分被使用。这基本上是一种耗费内存的方法，可能会导致[性能](/javarevisited/7-best-courses-to-learn-jvm-garbage-collection-and-performance-tuning-for-experienced-java-331705180686)下降。

```
public class SingletonEagerInstantiationExample {
    private static final SingletonEagerInstantiationExample *instance* = new SingletonEagerInstantiationExample();

    private SingletonEagerInstantiationExample() {
    }

    public SingletonEagerInstantiationExample getInstance() {
        return *instance*;
    }
}
```

## 惰性实例化:

顾名思义，只有在调用 `getInstance()` 方法时，对象才会被实例化。这是内存高效的方法。它确保只有在需要执行时才使用内存。

```
public class SingletonLazyInstantiationExample {
    private static SingletonLazyInstantiationExample *instance* = null;

    private SingletonLazyInstantiationExample() {}

    private static SingletonLazyInstantiationExample getInstance() {
        if (*instance* == null) {
            *instance* = new SingletonLazyInstantiationExample();
        }
        return *instance*;
    }
}
```

当涉及到[线程安全](https://www.java67.com/2015/09/thread-safe-singleton-in-java-using-double-checked-locking-pattern.html)时，上面的惰性实例化效率不高。多线程可以创建多个对象，这可能会否定使用单例方法的想法。因此，为了线程安全，又多了一种 Singleton 方法。

```
public class ThreadSafeSingleton {
    private static volatile ThreadSafeSingleton *threadSafeSingleton*;

    private ThreadSafeSingleton() {}

    synchronized public static ThreadSafeSingleton getInstance() {
        if (*threadSafeSingleton* == null) {
            *threadSafeSingleton* = new ThreadSafeSingleton();
        }
        return *threadSafeSingleton*;
    }
}
```

这里使用关键字 [***volatile***](https://javarevisited.blogspot.com/2011/06/volatile-keyword-java-example-tutorial.html) 是为了以防万一，如果任何线程对对象进行了任何更改，该更改会在所有线程中得到反映。

就这样，伙计们！！