# 较少听到的控制反转类型

> 原文：<https://medium.com/javarevisited/the-less-heard-type-of-inversion-of-control-924e5586377a?source=collection_archive---------1----------------------->

控制反转是一种帮助我们编写松散耦合代码的设计原则。每当我们听到 IoC，我们就会自动想到[依赖注入](https://javarevisited.blogspot.com/2015/06/difference-between-dependency-injection.html)，这两个术语经常互换使用。然而，事实并非如此。这种普遍的误解促使我写了这篇文章。

[![](img/19693259d20702579e9b97a198367f40.png)](https://javarevisited.blogspot.com/2012/12/inversion-of-control-dependency-injection-design-pattern-spring-example-tutorial.html)

照片由[纳丁·沙巴纳](https://unsplash.com/@nadineshaabana?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 到底什么是控制反转？

IoC 是一个反转程序控制流的原则。它可以是关于创建一个复杂的对象，编写回调，声明性编码和许多其他事情。

IoC 是一个原则，而依赖注入是这个原则的一个实现。让我们快速看一个例子。

```
class OrderService{
    OrderService(){
        this.emailService = new EmailServiceImpl()
    }
}
```

这里的`OrderService`需要一个`EmailService`来完成它的业务逻辑。不过有个问题，*耦合*太多。`OrderService`不仅依赖于一个具体的`EmailService`类，它还自豪地承担了创建`EmailService`对象的责任(这本身可能是一个复杂、昂贵的操作)。

为了解决这个问题，我们可以将 ***反转*** 上的控件`EmailService`创建对象。

```
class OrderService{
    OrderService(IEmailService emailService){
        this.emailService = emailService;
    }
}
```

请注意`OrderService`不再控制`EmailService`的创建方式。另外，`OrderService`现在只能依赖于一个 [*抽象*](https://javarevisited.blogspot.com/2010/10/abstraction-in-java.html) ( `IEmailService`是一个`interface`)并且我们甚至可以在运行时选择实际的实现。这样，我们可以实现更低的耦合和更好的可测试性。

以上解决方案确实是 [*依赖注入*](https://www.java67.com/2012/09/top-10-java-design-pattern-interview-question-answer.html) 。这里的*依赖* ( `EmailService`)是*注入*到主体(`OrderService`)。

但是依赖注入并不是 IoC 的唯一形式。

[![](img/18c2eb1dd6d5c6bd2868d0ac27e02279.png)](https://www.java67.com/2022/01/top-5-courses-to-learn-design-patterns.html)

尤达说，国际奥委会还有另一种模式

# 服务定位器

服务定位器是另一种使用控制反转的设计模式。然而，与 DI 不同，依赖项不会注入到主题中。主体通过中央*注册表*按需获取依赖关系。

例如:`OrderService`根据用户偏好向用户发送通知。通知可以是寻呼机、短信或电子邮件(默认)。

```
class OrderService{
    void notify(User user){
        serviceLocator.getServiceBy(user.preference())
            .sendNotification(user);
    }
}class ServiceLocator {
    INotificationService getServiceBy(
            NotificationType notificationType){
         return switch(notificationType){
             case PAGER -> pagerService
             case SMS -> smsService
             else -> emailService
         }
    }
}
```

就像这里的 DI 一样，`OrderService`也不负责创建`EmailService`(或任何其他服务)，因此实现了低耦合。

然而，这里通过`ServiceLocator`实现了另一件奇妙的事情。`OrderService`不需要知道`NotificationService`的任何子接口，它为`notificationType`寻找合适的`NotificationService`的工作也被委托给服务定位器，它可以被重用。

如果我们用 DI 实现这个功能，那么`OrderService`应该依赖于所有类型的`NotificationService`，并且它不能委派寻找合适的`NotificationService`的任务。

# ServiceLocator 的缺点

1.  依赖关系不明显，而且由于运行时缺少依赖关系，更容易出错。
2.  所有主题都必须包含对服务定位器的引用。
3.  与 DI 相比更难测试

# 结论

当我们每天都在使用 DI 时，我们应该了解 DI 和 IoC 之间的细微差别，以及它们之间的关系。虽然 DI 对于大多数用例来说是更合适的选择，但是也有服务定位器大放异彩的场景。