# TDA(告诉不要问)。

> 原文：<https://medium.com/javarevisited/tda-tell-dont-ask-c42a9d6a592?source=collection_archive---------6----------------------->

![](img/836abbb41f6e42dbe1a223076c66233b.png)

TDA(告诉不要问)

我想简短地写一个小帖子，关于一个有趣而有特色的[面向对象原则](/javarevisited/10-oop-design-principles-you-can-learn-in-2020-f7370cccdd31)，被称为 TDA 原则([告诉不要问](http://iamkoch.com/2013/10/oo-principles-tell-dont-ask/))。

TDA 鼓励**【不要】**从对象中检索状态，**【如果】**客户端代码将使用它来决定下一步做什么。

在我看来，这一点之所以重要，是因为我们希望避免客户的决定依赖于一些不受他们控制的状态。同样，不必要地向客户端代码暴露对象的状态(除非[不可变](http://javarevisited.blogspot.sg/2013/03/how-to-create-immutable-class-object-java-example-tutorial.html))可能并不总是令人满意的。我们应该尽可能地避免给客户代码关于他们正在调用的对象的状态的线索。

让我们来看一个不太担心这个原则的例子(问而不说的方法——与告诉而不问相反):

乍一看，这段代码看起来并没有错，但问题是，保镖对象的逻辑依赖于人的年龄。这种看不见的逻辑依赖，使软件不那么面向对象(不能自己做决定)。

这听起来可能很滑稽，但在“告诉不要问”的场景中，这个人应该知道如果不满 18 岁就不能参加聚会。

现在，我们可以通过“告诉不要问”来做同样的事情:

可以有不同的实现方式，但是我喜欢“[编程到一个接口，而不是一个实现](http://www.fatagnus.com/program-to-an-interface-not-an-implementation/)”。如你所见，决定是由人做出的，我们避免给客户代码关于状态的线索。因此，基于它们调用的对象的状态的危险的客户端决策不再可能。

*最初发布于*[http://javing . blogspot . com/2013/12/in-matters-of-style-swim-with-current . html](http://javing.blogspot.com/2013/12/in-matters-of-style-swim-with-current.html)