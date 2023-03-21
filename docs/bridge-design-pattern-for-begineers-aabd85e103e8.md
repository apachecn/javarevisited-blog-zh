# 初学者的桥梁设计模式

> 原文：<https://medium.com/javarevisited/bridge-design-pattern-for-begineers-aabd85e103e8?source=collection_archive---------1----------------------->

![](img/1b59f4a403cfee9d2dfcd5d539ab4a16.png)

杰克·安斯蒂在 [Unsplash](https://unsplash.com/s/photos/bridges?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

Bridge 设计模式是四人帮设计模式之一。

桥梁设计模式属于结构设计模式。它帮助我们分离抽象和实现。

因此抽象和实现可以分别开发和固定。

让我们看看实时的例子。假设你去一家酒店点一些食物。

正在准备的食物是抽象的。你不关心里面发生了什么。您唯一关心的是，实现是否正确，以及食物是否在适当的时间提供给您。

为你提供食物的服务员在准备食物的厨师和将要享用食物的你之间架起了一座桥梁。

让我们动手看看这个模式是如何实现的。

考虑一个**通知**的例子。安卓手机有推送通知，我们也可以通过短信收到通知。

```
package bridgedesignpattern;

public abstract class Notification {
    NotificationSender notificationSender;

    public Notification(NotificationSender notificationSender) {
        this.notificationSender = notificationSender;
    }

    abstract public void send();
}
```

通知类将有一个数据成员作为`NotificationSender`，也将有一个`send()`方法。NotificationSender 类如下所示。

```
public interface NotificationSender {
    public void sendNotification();
}
```

现在我们将有两个不同的类用于“文本通知”和“推送通知”。

```
package bridgedesignpattern;

public class PushNotification extends Notification {
    public PushNotification(NotificationSender notificationSender) {
        super(notificationSender);
    }

    @Override
    public void send() {
        notificationSender.sendNotification();
    }
}
```

下面是“TextNotification”类。

```
public class TextNotification extends Notification {

    public TextNotification(NotificationSender notificationSender) {
        super(notificationSender);
    }

    @Override
    public void send() {
        notificationSender.sendNotification();
    }
}
```

我们将有一个发送通知的`PushNotificationSender` 类。

```
package bridgedesignpattern;

public class PushNotificationSender implements NotificationSender {
    @Override
    public void sendNotification() {
        System.*out*.println("Push Notification sending.......");
    }
}
```

类似的还有`TextNotificationSender` 类会发送文本通知。

```
package bridgedesignpattern;public class TextNotificationSender implements NotificationSender {
    @Override
    public void sendNotification() {
        System.*out*.println("Text Notification sending.......");
    }
}
```

现在让我们看看代码是否有效:

```
package bridgedesignpattern;

public class NotificationSenderImplementation {
    public static void main(String[] args) {
        *//For push notification* NotificationSender notificationSender = new PushNotificationSender();
        Notification notification = new PushNotification(notificationSender);
        notification.send();

        *//For text notification* NotificationSender textNotificationSender = new TextNotificationSender();
        Notification textNotification = new TextNotification(textNotificationSender);
        textNotification.send();
    }
}
```

完成后，我们的输出如下:

```
Push Notification sending.......
Text Notification sending.......
```

# 我们取得了什么成就？

*   解耦:实现和[抽象](https://javarevisited.blogspot.com/2017/04/difference-between-abstraction-and-encapsulation-in-java-oop.html)是解耦的，两者都可以单独开发。
*   跨平台开发的较好解决方案。
*   它有助于两个不兼容的接口协同工作。

编码快乐！！！！