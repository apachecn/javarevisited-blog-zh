# 用 Spring Boot 和科特林构建 REST API

> 原文：<https://medium.com/javarevisited/build-rest-api-with-spring-boot-and-kotlin-e9b622366b68?source=collection_archive---------3----------------------->

## 了解如何使用 Spring Boot 和科特林构建一个简单的微服务应用程序。从 Spring Framework 5.0 开始，引入了对 Kotlin 的专用支持

![](img/008189774fb1c84300b8dbcf29ef9820.png)

# 1.概观

在本文中，我们将使用 Spring Boot 2.x 和 Kotlin 构建一个 REST API。同时，从 Spring Framework 5.0 开始引入了对 Kotlin 的专用支持。我们可以在官方的 [Spring 框架文档](https://github.com/spring-projects/spring-framework/wiki/Spring-Framework-Versions)中读到支持的特性。

特别是，应用程序将通过 REST API 公开数据，并将数据保存在嵌入式 H2 数据库中。

# 2.用例

我们将为员工信息应用程序构建 Restful APIs。**用户可以使用该应用程序**创建、检索、更新和删除员工。雇员有一个 id、用户名、名、姓、电子邮件 id 和出生日期。此外，员工的用户名必须是唯一的。

**我们将使用一个嵌入式 H2 数据库作为我们的数据源，同时使用** [**JPA**](/javarevisited/top-5-hibernate-online-training-courses-for-beginners-and-advance-java-programmers-469460596b2b) **和**[**Hibernate**](/javarevisited/top-5-books-to-learn-hibernate-for-java-developers-b2cb4b16ccd6?source=---------14------------------)**来访问数据库**中的数据。

# 3.设置

我们可以使用 Spring Initializer 工具创建一个 Spring Boot 应用程序。此外，我们可以在[博客文章](https://theanirban.dev/build-rest-api-spring-boot-kotlin/)中了解创建 Spring Boot 项目的其他方法。

**随后，我们将添加**[***Spring Web***](https://javarevisited.blogspot.com/2020/08/top-5-courses-to-learn-spring-mvc-for.html#axzz6qnblZnVj)**[**Spring Data JPA**](https://www.java67.com/2021/01/spring-data-jpa-interview-questions-answers-java.html)**【H2 数据库】**[***Spring Boot***](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e)**dev tools 作为依赖**。因此，现在我们在`build.gradle.kts`中有了所有基本的依赖关系，这将允许我们与 Spring Boot 和科特林一起工作:**

**Gradle 构建文件还包含一些重要的插件:**

*   **`kotlin-spring`编译器插件是在`all-open`插件之上的一个包装器。**`**all-open**`**编译器插件确保**[**Kotlin**](/javarevisited/top-5-courses-to-learn-kotlin-in-2020-dfc3fa7706d8?source=---------16------------------)**类被注释并且它们的成员是** `**open**` **。这使得使用像 Spring AOP 这样的库变得很方便，这些库要求类是开放的，不像 Kotlin 类和它们的成员在默认情况下是 final。******
*   ****`kotlin-jpa`编译器插件是`no-arg`插件之上的一个包装器。**`**no-arg**`**编译器插件为标注有** `**Entity**` **、** `**MappedSuperclass**` **和** `**Embeddable**` **的类生成一个无参数构造函数。这确保了 JPA(Java Persistence API)可以实例化该类，因为它期望为其实体定义一个无参数的构造函数。********

## ****3.1.配置数据库****

****我们必须配置 H2 数据库属性，以便 Spring Boot 可以创建数据源。为了定义这样的属性，我们可以使用外部配置。此外，我们可以使用属性文件、YAML 文件、环境变量和命令行参数来定义这样的外部配置。****

******一般情况下，**[](/javarevisited/10-advanced-spring-boot-courses-for-experienced-java-developers-5e57606816bd?source=collection_home---4------0-----------------------)****提供了** `**application.properties**` **文件来默认定义此类数据库属性**。属性文件使用键值格式。******

******或者，我们可以使用基于 YAML 的配置文件，这些文件使用分层数据**。YAML 文件大大有助于避免重复的前缀，并且与属性文件相比更具可读性:****

****我们也可以使用`application.yml`文件来定义 hibernate 属性:****

*   ****我们将`ddl-auto`属性设置为`update`。它将根据应用程序中领域模型的修改来更新数据库模式。
    如果我们使用没有任何模式管理器的嵌入式数据库，这个缺省值是`create-drop`。
    **如果我们使用 Liquibase 或 Flyway 这样的模式管理器，我们应该考虑使用** `**validate**` **。**它试图根据我们在应用程序中创建的实体来验证数据库模式，如果模式与实体规范不匹配，就会抛出一个错误。****
*   ****`show-sql`属性允许记录 [SQL 语句](/javarevisited/7-free-courses-to-learn-database-and-sql-for-programmers-and-data-scientist-e7ae19514ed2)。****
*   ****属性确保是否在启动时初始化模式。****
*   ****`dialect`属性确保 [Hibernate](https://www.java67.com/2017/02/2-best-books-to-learn-hibernate-for-Java-Developers.html) 为选择的数据库生成更好的 SQL。****

# ****4.领域模型— Kotlin 数据类****

****领域模型是应用程序的核心部分。我们可以使用数据类创建一个领域模型`Employee`。****

******另外，**[](/javarevisited/7-free-courses-to-learn-kotlin-in-2020-327c3872c1e1?source=collection_home---4------2-----------------------)****中的一个** `**data class**` **会自动生成**`**equals/hashCode**`**`**toString**`**和** `**copy**` **函数。**此外，我们不需要像 Java 一样定义 getter 和 setter 方法:********

*   ****`@Entity`注释指定该类是一个实体，并映射到一个数据库表。****
*   ****`@Table`注释指定了用于映射的数据库表的名称。
    **如果我们使用** `**@Table**` **标注而没有表名，那么**[**Spring**](/javarevisited/10-best-spring-framework-books-for-java-developers-360284c37036)**会从类名**中生成表名。****
*   ****`@Id`注释指定了实体类中的主键属性。****
*   ****`@GeneratedValue`注释指定了主键值的生成策略。****
*   ****`@Column`注释指定了持久属性的映射列。如果我们不使用`@Column`注释，那么 [Spring](/javarevisited/top-10-free-courses-to-learn-spring-framework-for-java-developers-639db9348d25) 从属性名中生成列名。****

# ****5.贮藏室ˌ仓库****

****我们将创建一个存储库接口来与数据库进行交互。**此外，** `**EmployeeRepository**` **接口将从** `**JpaRepository**` **接口**扩展而来。这确保了`Employee`实体上的所有 CRUD 方法都可用:****

*   ****`@Repository`注释指定该类是一个存储库，代表我们应用程序中的数据访问层。****

# ****6.服务****

****现在，我们将创建一个服务类来提供业务逻辑。**此外，服务层可以使用 JPA** 与数据访问层进行交互。****

****我们通过`EmployeeService`中的构造函数注入`EmployeeRepository`的一个实例:****

*   ****`@Service`注释指定该类是一个服务。****
*   ****`getAllEmployees()`函数可以获取所有员工的列表。****
*   ****`getEmployeesById()`函数可以根据 id 提取一个员工。我们提供雇员 id 作为函数的参数。****
*   ****`createEmployee()`功能可以创建新员工。我们提供新雇员所需的属性作为函数的参数。****
*   ****`updateEmployeeById()`函数可以根据 id 更新员工的详细信息。我们提供雇员 id 和属性作为函数的参数。****
*   ****`deleteEmployeesById()`函数可以根据 id 删除员工。我们提供雇员 id 作为函数的参数。****

# ****7.控制器****

****我们在调用底层服务的控制器层中实现所有 CRUD 操作的 API。****

****我们定义一个`EmployeeController`作为我们的控制器类。**具体来说，它是一个** [**REST 控制器**](https://javarevisited.blogspot.com/2017/08/difference-between-restcontroller-and-controller-annotations-spring-mvc-rest.html#ixzz6OYNB9oii) **，为创建、操作和删除员工**提供端点。****

****`EmployeeService`类被连接到控制器中以返回值。此外，为了简单起见，我们重用实体作为数据传输对象:****

****`@RestController`注释指定该类是一个控制器，能够处理请求。它结合了`@Controller`和`@ResponseBody`注释。****

******Spring 提供了几个注释来处理不同的传入 HTTP 请求，比如 GET、POST、PUT 和 DELETE** 。这些注释是`@GetMapping`、`@PostMapping`、`@PutMapping`和`@DeleteMapping`。****

# ****8.运行应用程序****

****我们可以在终端中使用以下命令运行应用程序:****

```
**gradle bootRun**
```

****Spring Boot 应用程序将在 [http://localhost:8080 开始运行。](http://localhost:8080.)****

# ****9.探索 API****

## ****9.1.创建员工****

****我们发送以下请求来创建员工:****

****我们收到以下 [JSON](https://javarevisited.blogspot.com/2013/04/convert-json-array-to-string-array-list-java-from.html) 格式的响应:****

## ****9.2.获取所有员工****

****我们发送以下请求来获取所有员工:****

****我们收到以下 JSON 格式的响应:****

## ****9.3.获得员工****

****我们发送以下请求，根据员工 id 获取员工:****

****我们收到以下 JSON 格式的响应:****

## ****9.4.更新员工****

****我们发送以下请求，根据员工 id 更新员工属性:****

****我们收到以下 JSON 格式的响应:****

## ****9.5.删除员工****

****我们发送以下请求，根据员工 id 删除一名员工:****

# ****10.结论****

****在本教程中，我们研究了如何使用科特林和 Spring Boot 创建一个微服务应用并公开 REST API。在这个过程中，我还使用了一些我们在构建这样的应用程序时可以利用的最佳实践。****

****这些例子的代码可以在 [GitHub](https://github.com/anirban99/spring-boot-examples/tree/main/spring-boot-boilerplate) 上找到。****

# ****资源:****

*   ****[Spring 框架文档](https://github.com/spring-projects/spring-framework/wiki/Spring-Framework-Versions)****
*   ****[如何创建 Spring Boot 项目](https://www.expatdev.com/posts/how-to-create-a-spring-boot-project/)****
*   ****[GitHub 库](https://github.com/anirban99/spring-boot-examples/tree/main/spring-boot-boilerplate)****

*****最初发表于* [*阿尼班的科技博客*](https://theanirban.dev)****