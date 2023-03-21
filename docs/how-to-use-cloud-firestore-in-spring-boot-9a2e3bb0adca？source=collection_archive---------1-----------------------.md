# 如何在 Spring Boot 使用云 Firestore？

> 原文：<https://medium.com/javarevisited/how-to-use-cloud-firestore-in-spring-boot-9a2e3bb0adca?source=collection_archive---------1----------------------->

![](img/b3c70eeff6edd10f1d0b5bbc851de0a3.png)

美国国家海洋和大气管理局在 [Unsplash](https://unsplash.com/s/photos/fire-cloud?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

Firestore 是 Firebase platform 中提供的可管理、可扩展的文档存储。我为什么喜欢它？你可以开始使用它，每天获得 20K 的读写次数，而不用支付一分钱，也不用分享你的信用卡信息。20K 的读写对于试验和创建中小型应用程序来说绰绰有余。

Firebase 已经为不同的编程语言提供了一些功能丰富的客户端包。也有一个针对 Java 的，它的文档写得很好，可以开始使用。在这篇文章中，我想分享我是如何在我的项目中使用一些 moden java 和 spring goodies 来配置和访问 firestore 的。

首先，我在 firebase 控制台中创建了一个 firestore 项目，并获取了在我的应用程序中使用的访问密钥。然后我在我的 spring boot 项目中添加了依赖项 [firebase-admin](https://mvnrepository.com/artifact/com.google.firebase/firebase-admin/7.1.0) 。

然后我在我的项目中创建了一个名为 **firestore** 的包，并在里面创建了一些文件。

```
├── firestore
│   ├── AbstractFirestoreRepository.java
│   ├── DocumentId.java
│   └── FireStoreConfig.java
```

FireStoreConfig.java 包含一个 Bean，它读取和使用凭证并返回一个 FireStore 实例，该实例可用于访问集合和文档。这是我的 FireStoreConfig 类的样子:

这里，我采用了从 **application.properties.** 下载的凭证 JSON 的路径，尽管我更喜欢从环境变量中保存和检索凭证相关的内容，但在这种情况下，我无法找到这样的方法。

我们存储在 Firestore 中的每个文档都包含一个唯一的 Id。我创建了一个注释，这样我就可以注释我的实体类中的一个字段，它将被用作这个 Id。我将此命名为 **DocumentId** ，它看起来像:

最后，我写了 AbstractFirestoreRepository，这是一个抽象的泛型类。从这个类中，我可以创建任意数量的提供类型的具体存储库。这个抽象存储库通过以通用方式实现 firestore apis 来包含 CRUD 方法。因此，我不必为我为项目创建的每个集合编写相同的样板代码。看起来是这样的:

在这里，我试图让大多数方法不言自明。所有的公共方法只是我们如何在 firestore 中保存、获取或删除文档的反映。但是我想补充几句关于其他方法的话，这会帮助你更好地理解我在做什么。

getParameterizedType: 因为这是一个泛型类，这意味着我可以用它指定一个类型。比如我可以做一个这样的用户存储库:

```
public class UserRepo extends AbstractFirestoreRepository<User> {}
```

在一些像 *get* 和 *retrieveAll* 的方法中，我实际上需要 **User.class** 对象。这个 getParameterizedType 有助于获取指定为< T >的类型。

**getDocumentId:** 我之前提到过，要在 firestore 中存储一个文档，我们需要一个惟一的 Id，这个方法检查它接收到的对象中的每个字段的 DocumentId 注释。如果找到，它返回这个，如果没有，它返回一个 UUID。由于这个方法有 *protected* 修饰符，所以可以根据需要在子类中覆盖它。

现在，通过扩展这个方法，我可以在我的项目中创建尽可能多的库。示例存储库将如下所示:

这里是超级方法，第一个参数是 FireStoreConfiguration 中用 Bean 实例化的 Firestore 实例，第二个参数是集合的名称。

用户类如下所示:

看，这里的 id 属性是用 DocumentId 注释的，在 firestore 中创建文档时，使用该字段的值作为 Id 会有所帮助。

今天到此为止。我真的想为自定义查询编写一个通用方法。但是考虑到查询的语法及其异构性，它认为每个具体的存储库应该自己处理它们。但是如果你对此有任何想法，或者在 springboot 中集成 firestore 的更好的结构，请随时与我们分享。