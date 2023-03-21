# 如何让你的代码读起来像一首诗？漂亮的布尔变量

> 原文：<https://medium.com/javarevisited/how-to-make-your-code-reads-like-a-poem-beautiful-boolean-variables-3842bb037f1f?source=collection_archive---------2----------------------->

![](img/b9253b5885d4009f5454c92470dfb568.png)

代码读起来应该像写得很好的散文。这就是为什么你应该小心布尔值。

这篇文章是名为 [clean code:在 7 天内提升你的编程生涯](https://rebrand.ly/sppc-m-booleans)的 Clean Code 深度课程的一部分，你可以去看看。它目前以 87%的折扣出售。

[![](img/dc9c4358d48d8c1502f7fa18c29da201.png)](https://rebrand.ly/sppc-m-booleans)

因为你经常在 if 语句中使用它们，所以你有机会让代码读起来非常非常好，或者让代码看起来非常糟糕，难以阅读。让我们看一个例子:

```
if(person.hunger < 6.8 && person.money >= getClosestOpenResurant(person.location).dinnerPrice && day() < 6){
   // make an order
}
```

你是第一次看到这个代码，你明白吗？还是你需要一些时间考虑一下？我相信你最终会明白的，但是你需要时间。现在让我们提取布尔值并给它们命名。让我们看看这是否能让代码读起来更好:

```
boolean personIsHungry = person.hunger < 6.8;
boolean personCanAffortDinner = person.money >= getClosestOpenResurant(person.location).dinnerPrice;
boolean isWeekDay = day() < 6;
if(personIsHungry && personCanAffortDinner && isWeekDay) {
   // make an order
}
```

现在想象一下第一次看到这段代码。让我们来读一下 *if* 语句。*如果一个人饿了，而且他能负担得起晚餐，而且是工作日，那么就点餐。我认为这样更好，你不觉得吗？但是我还是不喜欢。看，前两个布尔值是和 Person 对象一起工作的。这个逻辑可以封装在对象内部。此外，我们可以为工作日逻辑创建一个对象:*

```
class Person {
   // fields here

   pubic boolean isHungry() {
      return hunger < 6.8
  }

   public boolean canAffortDinner() {
      return person.money >= ResturantUtils.getClosestOpenResurant(person.location).dinnerPrice
   }
}class TimeOperations {
   public static boolean isWeekday() {
      return day() < 6;
   }
}if(person.isHungry && person.canAffortDinner && TimeOperations.isWeekDay) {
   // make an order
}
```

现在我们有了一个包含布尔方法的 Person 类。我们有一个 TimeOperations 类，它有一个 *isWeekday* 方法。现在我们来看看 if 语句。这是一行可读的代码。如果你第一次看到这个，你就不会有理解发生了什么的问题。这是因为我们将细节隐藏在命名的代码块中。您不仅应该对布尔值这样做，还应该对您正在编写的所有代码这样做。

![](img/82e538ed14c65a6214e74f33fb6e9e60.png)

我看到很多人这样写代码:

```
public void deleteUser(int id) {
   if (loggedUser != null) {
      if(loggedUser.sessionExpired()) {
         // log erorr and throw
      }
      if(loggedUser.isNotAdmin()) {
         // log erorr and throw
      }
      userRepository.delete(id);
   } else {
      // throw
   }
}
```

有很多巢。嵌套代码很难阅读。尽量避免。你看到我们在哪里删除用户了吗？这是我们的主要逻辑。剩下的就是细节了。看看 main 方法逻辑是如何包装检查和细节的。如果我需要理解这个逻辑，我将需要浏览围绕它的所有其他代码，然后我将转到我想看的那一行。我们可能不关心其他代码。在这种情况下，我们可以独立于业务逻辑进行验证。为此，我们只需翻转第一个 *if* ，然后复制其后的其他检查，如下所示:

```
public void deleteUser(int id) {
   if (loggedUser == null) {
      // throw
   }
   if(loggedUser.sessionExpired()) {
       // log erorr and throw
   }
   if(loggedUser.isNotAdmin()) {
      // log erorr and throw
   } userRepository.delete(id);	
}
```

当然，这对于这个例子是有效的。你可能有不同的情况。试着把检查和细节从主逻辑中分离出来。

看看我们是如何解开业务逻辑的。好多了。我想让你看两件事:

1.  首先——看到两种方法之间的区别并思考它们。
2.  第二，想办法让 deleteUser 方法更干净。我们能做什么来使业务逻辑成为视图的中心。

![](img/8a431b0ed34c31c21ce30618c9e1dfe7.png)

我们能做的是…没错，伙计们，在单独的方法中提取验证，如下所示:

```
public void deleteUser(int id) {
   validatePermissions();
   userRepository.delete(id);
}private void validatePermissions() {
   if (loggedUser == null) {
      // throw
   }
   if(loggedUser.sessionExpired()) {
      // log erorr and throw
   }
   if(loggedUser.isNotAdmin()) {
      // log erorr and throw
   }
}
```

现在来看看 deleteUser 方法，这样更好。我们有一行用于验证，一行用于实际的方法逻辑。如果我们想看到验证，我们进入`validatePermissions` 方法。同样，我们不需要用细节来污染方法。看看这两种方法的区别。你可以清楚地看到哪个更好。

**出发前—我们令人惊叹的干净代码课程目前以 87%的折扣提供**

如果你喜欢这篇文章，你会喜欢我们的课程:

*   46 场讲座
*   3 小时以上的内容
*   做一个真实的项目来测试你的知识
*   27 个可下载资源
*   30 天退款保证

你可以在这里报名参加课程— [干净代码:7 天暴涨你的编程生涯](https://rebrand.ly/sppc-m-booleans)

[![](img/dc9c4358d48d8c1502f7fa18c29da201.png)](https://rebrand.ly/sppc-m-booleans)