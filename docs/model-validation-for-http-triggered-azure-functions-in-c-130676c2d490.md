# C#中 Http 触发 Azure 函数的模型验证

> 原文：<https://medium.com/javarevisited/model-validation-for-http-triggered-azure-functions-in-c-130676c2d490?source=collection_archive---------0----------------------->

![](img/199e0d8cdddae68e3d761376df7c19ec.png)

本·科尔德在 [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

**Azure 功能简介**

Azure Functions 是一个事件驱动的无服务器解决方案，开发人员可以专注于编写代码片段，而不必为部署和管理其可扩展性的基础架构投入太多精力。

每个函数都有一个触发器，用于执行该函数。以下是使用的一些主要触发器:

1.  超文本传送协议（Hyper Text Transport Protocol 的缩写）
2.  计时器
3.  事件网格
4.  长队
5.  …

你可以从他们的官方网站[这里](https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings?tabs=csharp#supported-bindings)找到整个名单。

今天，我们将关注 Http 触发的 Azure 函数，看看我们如何验证输入，比如查询参数和 JSON 主体。

**ASP 中的模型验证。NET Core Web API**

如果您不熟悉 C#，是的，通过模型验证可以很容易地验证 HTTP 请求中的输入。

我们可以简单地向模型添加注释来指定需求。

你可以在这里找到更多的内置属性。

在对模型进行注释之后，在控制器中，我们可以在继续处理请求之前简单地检查模型的有效性。

下面是一个控制器的例子，摘自微软的[文档](https://docs.microsoft.com/en-us/aspnet/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api)，关于如何在 ASP 中轻松实现验证。NET Web API。

```
using MyApi.Models;
using System.Net;
using System.Net.Http;
using System.Web.Http;namespace MyApi.Controllers
{
    public class ProductsController : ApiController
    {
        public HttpResponseMessage Post(Product product)
        {
            if (ModelState.IsValid)
            {
                // Do something with the product (not shown).return new HttpResponseMessage(HttpStatusCode.OK);
            }
            else
            {
                return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
            }
        }
    }
}
```

**Azure 函数中的模型验证**

能够重用 ASP 可用的模型验证的思想。NET /。NET Core 将有助于使我们的代码更加直观和可读。

然而，由于 Azure 函数中没有对验证的官方支持，所以手动编写 If-Else 语句来检查模型的有效性是一件非常痛苦的事情。

这里有一个解决方案。

首先，我们必须编写我们自己的包装类，它将保存一个关于模型是否有效的布尔值、值本身以及验证结果。

接下来，我们用方法`GetBodyAsync()`和`GetQueryParamAsync()`为`HttpRequest`构造一个扩展类，分别验证 HTTP JSON 主体和查询参数。

下面是一个在 HTTP 触发的 Azure 函数中使用 ModelValidationExtension 的例子。

瞧啊。我们现在可以用一种更直观的方式来验证我们的 HTTP 输入，这种方式很容易管理。

感谢您阅读并关注更多关于 Azure 函数的文章！😁