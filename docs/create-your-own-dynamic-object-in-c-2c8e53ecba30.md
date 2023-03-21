# 在 C#中创建自己的动态对象

> 原文：<https://medium.com/javarevisited/create-your-own-dynamic-object-in-c-2c8e53ecba30?source=collection_archive---------1----------------------->

![](img/c46b1bc21139c01ae314e14d07b52338.png)

由 [Gabriel Crismariu](https://unsplash.com/@momentsbygabriel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/dynamic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

如果您使用的是 NoSQL 数据库，您可能希望创建一个类，该类除了包含标准属性之外，还可以包含可扩展属性，以便利用 NoSQL 中的动态模式功能。

您可以依赖 Json Serialiser，尤其是 System.Text.Json。

**什么是系统。Text.Json？**

系统。Json 是微软默认的 Json 串行器。它比[牛顿软件](https://www.newtonsoft.com/)快大约 2 倍。

英寸与. NET 5 中的初始版本相比，微软在性能方面有了显著的提高。网芯 3.1。

**进场**

我们将依赖来自[系统的](https://docs.microsoft.com/en-us/dotnet/api/system.text.json.serialization?view=net-5.0)`[JsonExtensionData](https://docs.microsoft.com/en-us/dotnet/api/system.text.json.serialization.jsonextensiondataattribute?view=net-5.0)`属性。Text.Json.Serialization 命名空间，它将被分配给一个字符串到 object/JsonElement 的字典。该字典将用于保存未显式声明为类属性的附加属性。就像任何其他 Json 属性一样，这个属性需要一个 public 修饰符。

如果您不需要自己手动实例化一个可扩展对象，则不需要在构造函数中实例化 dictionary 对象。换句话说，如果您只使用 Json serialiser 在整个应用程序中构造可扩展对象，那么 dictionary 对象的实例化将在序列化/反序列化操作期间处理。

声明了一个索引器方法，以允许轻松访问存储在字典中的附加属性。

基本可扩展对象类

这个可扩展的对象类将只处理附加的属性，并且不知道该类中声明的其他现有属性。这将消除使用反射来访问现有属性的需要。

**用途**

例如，您希望有一个动态的动物类。

具有可扩展属性的派生类的示例

它将能够接受下面的 JSON 有效负载，并相应地将值解析为各自的属性。

```
// Duck type{
"Name": "Mallory",
"Gender": 1, // 1 is male, 0 is female
"DateOfBirth": "2018-09-19T12:00",
"IsMammal": true,
"BeakColor": "Red" // additional property can be found in dictionary
}// Dog type{
"Name": "Groot",
"Gender": 1, // 1 is male, 0 is female
"DateOfBirth": "2018-09-19T12:00",
"IsMammal": true,
"EyeColor": "Brown" // additional property can be found in dictionary
}
```

**应该用 Object 还是 JsonElement 作为可扩展字典的值的类型？**

对象是基类类型，可以在您的代码库中实例化。这使您能够轻松地向可扩展字典添加更多的键/值对。

JsonElement 是一个只能在系统内实例化的对象。Json 包。因此，如果您需要向字典中添加更多的值，将会更加麻烦。

除此之外，JsonElement 有一个 GetRawText 方法，它为您提供被解析的原始字符串值。

希望这篇文章对你有所帮助。感谢您的阅读，敬请关注更多 C#相关文章！