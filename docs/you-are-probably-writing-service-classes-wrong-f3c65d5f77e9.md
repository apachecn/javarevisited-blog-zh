# 您可能写错了服务类

> 原文：<https://medium.com/javarevisited/you-are-probably-writing-service-classes-wrong-f3c65d5f77e9?source=collection_archive---------1----------------------->

![](img/1720a3d974721caed2ed42fd6ff9347d.png)

克拉克·范·德·贝肯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Java EE 和 Spring 已经成为企业级 Java 应用程序的事实上的标准，尤其是 web 应用程序。但本文并不谈论 [Java EE](/javarevisited/top-7-online-courses-to-learn-java-ee-jakarta-ee-in-2020-216c1a5eea99) 或 [Spring](/javarevisited/10-best-online-courses-to-learn-spring-framework-in-2020-f7f73599c2fd) ，而是一个标准应用程序的各层。

在这篇文章中，我将挑战大多数人是如何设计他们的应用程序的，从`controller`层到`dao`层。然而，我的主要目标将是服务层，因为我经常看到这一层被滥用得最多。

为了使它更有趣和相关，我将举一个例子。首先，我将向你展示一个传统的设计，并检查它。接下来，我将向您展示一个据称更好的设计，并指出它的优点。我们开始吧。

它是一个银行系统，保存着人们的账户和钱。要求是，

1.  账户需要维持最低余额。
2.  资金可以通过符合账户余额的支票在账户间转移。

# 传统实现(贫血模型)

假设我们使用的是 [JPA](/javarevisited/5-best-spring-data-jpa-courses-for-java-developers-45e6438be3c9) ，让我们创建一个实体类。

```
@Entity
class Account {
    Long accountNo;
    Long balance;
}
```

类似的一个 DTO 类，

```
class AccountDto {
    Long accountNo;
    Long balance;
}
```

最重要的是服务类(业务逻辑)，

```
class AccountService { AccountDao accountDao; public void processCheque(ChequeDto cheque){
        var fromAccount = accountDao.load(cheque.from());
        var toAccount = accountDao.load(cheque.to());
        if(fromAccount.balance-MIN_BALANCE > cheque.amount){
           fromAccount.balance -= cheque.amount;
           toAccount.balance += cheque.amount;
        }
    }}
```

如果上面的设计对你来说没问题，让我问你下面的问题，

## 这是面向对象的吗？

从业务问题来看，我们是否同意“Account”是这里的主要域实体(我不是指 hibernate 实体)？但是在设计中，`Account`类没有任何行为。行为从领域实体中提取出来，并写入服务类中。

`balance`是域实体`Account`的属性，但是我们从外部操纵它。

一个只有属性而没有行为的对象是完全可以的。比如说喜欢值对象(`AccountDTO`)。但是将一个对象的行为从该对象中部分或全部移除，是违背[面向对象设计原则](https://javarevisited.blogspot.com/2018/07/10-object-oriented-design-principles.html)的。

OOP 有 4 个基本概念: *APIE* ！

1.  抽象
2.  多态性
3.  遗产
4.  包装

在这 4 个当中，我们已经成功地摧毁了抽象和封装。

a.`Account`类的`balance`属性应该被封装，但是它可以从外部公开访问。

b.如何更新`Account`类的封装属性`balance`，根本不是从`Account`类的用户中抽象出来的。更确切地说，行为是在类之外的，所以绝对没有抽象。

你可能会说你不喜欢 OOP，你不理解它。那很好。但是所有范例都鼓励抽象和封装。

## 什么是服务等级？

服务类是一个无状态的单例，这是由设计决定的。它应该是编排业务的一个薄层。

**控制器层**:负责与 web 客户端通信。API 公开了契约、HTTP 响应代码等。这里不应该有任何商业逻辑。

**存储库/ DAO 层**:与数据存储通信。没有商业逻辑。可以把它想象成数据存储的 API。

**服务层**:一个*瘦*无状态层，协调域层。它里面没有商业逻辑。

这种没有任何行为的厚服务层和域对象的设计被称为贫血模型。

# 领域驱动的设计

当我们看到传统贫血模型的缺点时，让我们按照 [OOP 范例](https://javarevisited.blogspot.com/2012/12/what-is-object-in-java-or-oops-example.html)重写解决方案。

薄服务层。不知道任何商业逻辑。

```
class ChequeService { public void processCheque(ICheque cheque){
       cheque.process();
   }}
```

`Cheque`类、

```
class Cheque implements ICheque{ public void process(){
       var fromAccount = accountDao.load(from);
       var toAccount = accountDao.load(to);
       fromAccount.deductAmount(amount)
       toAccount.depositAmount(amount)
   }}interface ICheque {
    void process();
}
```

还有`Account`类，

```
class Account implements IAccount { static final Long MIN_BALANCE = 1000;
    private Long balance;
    private Long accountNo; public void depositAmount(Long amount){
        balance += amount; 
    } public void deductAmount(Long amount){
        if(balance-amount<MIN_BALANCE)
            throw new MinimumAccountBalanceException(balance, amount);
        balance -= amount; 
    }}interface IAccount {
    void depositAmount(Long amount);
    void deductAmount(Long amount);
}
```

**以下是本设计的基本变化，**

1.  `Account`类是*封装*的`balance`属性和*抽象*的`balance`更新过程。
2.  处理支票的实现也在`Cheque`类中*抽象*。
3.  服务类不受任何业务逻辑或规则的约束。

现在让我们来看看我们的新设计带来了什么好处，

## **打开扩展-关闭修改**

由于我们的领域对象现在功能非常强大，所以添加新特性变得非常容易。同时，域对象执行抽象和封装来保护外部行为。阅读更多关于*开合原理*[https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)的信息。

假设我们推出更多的交易服务，如即期汇票服务、网上银行服务。所有这些服务都处理帐户余额。现在，如果余额存款/取款逻辑发生变化，我们将只在`Account`类中进行更改。

这种改变对这些服务来说是透明的。然而，对于一个不完善的设计，我们将单独地改变所有这些服务中的代码。这意味着更多的开发工作、测试工作和更多引入 bug 的机会。

## **多态性和遗传**

我们现在可以将多态和继承应用到我们的域对象中。既然我提到了继承，我想提一下 https://en.wikipedia.org/wiki/Composition_over_inheritance

你注意到我是如何在领域驱动设计中使用接口的吗？这是为了减少服务层和域层(域对象驻留的地方)之间以及域层内部的耦合。在大多数情况下，这些类通过接口相互认识，即*契约*(接口很漂亮，不是吗？).所以`Cheque`只要满足`IAccount`的契约，一个类可以使用任何种类的账户。并且可以有具有不同实现/业务逻辑的不同类型的账户。例如，活期账户每次扣款收取 0.5%的费用，并且没有最低余额要求。

```
class CurrentAccount implements IAccount {
    private Long balance;
    private Long accountNo; public void depositAmount(Long amount){
        balance += amount; 
    } public void deductAmount(Long amount){
        balance -= amount + amount * 0.005;
    }}
```

现有的支票服务和`Cheque`类将与此无缝协作。这同样适用于*多变形*的`Cheque`类。

类似地，我们可以用 balance 和 account 创建一个抽象类`AbstractAccount`和我们的 account 类`extend`，即应用*继承*。但是使用继承有一些缺点，因此通常建议使用*合成而不是继承*。

而这仅仅是为了这个商业问题。坚持领域驱动设计的更大的项目在很多方面受益，例如，在团队之间用无处不在的语言更好的交流，最小的变化影响，灵活性，等等。

# 什么是领域驱动设计？

上面的解决方案是领域驱动设计的一个例子，由 Eric Evans 在他的名著*领域驱动设计:解决软件核心的复杂性中创造并推广。*

> 领域驱动设计是软件代码的结构和语言应该与业务领域相匹配的概念。—维基百科

这是一个巨大的概念，这篇文章的目标不是覆盖领域驱动设计，而是让你体验一下。

# 结论

贫血模型基本上是一个*反模式*，大多数人不假思索地跟随它。我们应该更有意识地设计我们的应用程序。从长远来看，它肯定会回报我们。如果这篇文章让你对 DDD 感兴趣，那对我来说是一个胜利。