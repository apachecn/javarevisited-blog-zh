# Javax 验证和组

> 原文：<https://medium.com/javarevisited/javax-validation-and-groups-acd9a1fb1091?source=collection_archive---------0----------------------->

![](img/8e5311d5d9d414631ebf2678470c1856.png)

米歇尔·勒恩斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

简而言之，Javax 验证使用两组主要注释

*   确保验证发生的注释，如***@有效*** 或*@有效*
*   注释，表示要对值强制执行的约束类型，如 ***@NotNull*** ， ***@Pattern***

如果我们在项目中不使用 Spring 框架，hibernate validator 将是 JSR 验证规范最常见的实现。

## @Valid 和@Validated 是如何工作的？

当一个参数用 ***@Valid*** 注释时，Java 会知道这个对象需要被验证，类内部声明的所有约束都会被求值。

让我们使用一个具有很少约束的简单模型，如 ***@NotEmpty，@Min*** 定义的:

```
package com.mimr.model;import com.fasterxml.jackson.annotation.JsonCreator;
import com.fasterxml.jackson.annotation.JsonProperty;import javax.validation.constraints.*;public class Word { @NotEmpty
    private String id;
    private Language language;
    @NotEmpty
    @Size(min = 1)
    private String originalWord;
    private String translatedWord;
    @Email
    private String addedBy; @JsonCreator
    public Word(@JsonProperty("id") String id,
                @JsonProperty("language") Language language,
                @JsonProperty("originalWord") String originalWord,
                @JsonProperty("translatedWord") String translatedWord,
                @JsonProperty("addedBy") String addedBy) {
        this.id = id;
        this.language = language;
        this.originalWord = originalWord;
        this.translatedWord = translatedWord;
        this.addedBy = addedBy;
    }// Getters
}
```

为了在反序列化发生时启用验证，例如当一个端点正在接收一个带有请求体的请求时，我们将用 ***@Valid*** 注释对请求体进行注释

```
package com.mimr.resource;import com.mimr.model.Word;import javax.validation.Valid;
import javax.ws.rs.Consumes;
import javax.ws.rs.POST;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;@Path("/api/word/")
@Consumes(MediaType.*APPLICATION_JSON*)
@Produces(MediaType.*APPLICATION_JSON*)
public class WordResource { @POST
    public Response createWord( @Valid Word word){
        return Response.*ok*().build();
    }
```

当我们向端点发出下面的请求时，我们将收到一个错误，因为请求中的“originalWord”为空。

```
POST /api/word/ HTTP/1.1
Host: localhost:8080
Content-Type: application/json{
  "id": "id1",
  "language": "DEUTSCH",
  "originalWord": "",
  "translatedWord": "Hello",
  "addedBy": "[xyz@gmail.com](mailto:mrlreni@gmail.com)"
}
```

收到以下响应:

```
{"errors":["originalWord must not be empty","originalWord size must be between 1 and 2147483647"]}
```

## 验证组:

当我们将同一个模型用于多个操作(如创建或更新)时，我们可能希望根据需要对同一个模型进行不同的验证。

例如，如果我们想使用 Word.java 来创建和更新一个 word 对象，我们可能不想在创建它时将 id 字段作为强制字段。

这里的解决方案是创建一个验证组，并使用它只对更新操作进行验证。

1.  ***创建验证组:***

验证组只不过是一个简单的[标记接口](https://javarevisited.blogspot.com/2012/01/what-is-marker-interfaces-in-java-and.html)。

```
package com.mimr.validation;public interface Update {
}
```

2. ***通过用验证组*** 标记约束来更新模型

```
public class Word { @NotEmpty(groups = {Update.class})
    private String id;
    private Language language;

//
..
//
```

3. ***使用@Validated 注释有选择地强制验证***

```
public class WordResource { @POST
    @Path("create")
    public Response createWord( @Valid Word word){
        return Response.*ok*().build();
    } @POST
    @Path("update")
    public Response updateWord( @Validated(Update.class) Word word){
        return Response.*ok*().build();
    }
```

现在，对 id 的验证只针对更新操作。