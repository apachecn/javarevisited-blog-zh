# Spring Boot —黑盒测试

> 原文：<https://medium.com/javarevisited/spring-boot-black-box-testing-3a6954591ae?source=collection_archive---------3----------------------->

在这篇文章中，我将向你展示

1.  白盒测试和黑测试有什么区别？
2.  后者有什么好处？
3.  如何在您的 Spring Boot 应用程序中实现它？
4.  如何配置 OpenAPI 生成器来简化代码和减少重复？

您可以在[这个库](https://github.com/SimonHarmonicMinor/spring-boot-black-box-testing-example)中找到代码示例。

![](img/1c2c4d5daac3e18daacb288d564b497e.png)

# 领域

我们正在开发一个餐厅自动化系统。有两个域类。`Fridge`和`Product`。一个冰箱可以有许多产品，而一个产品属于一个冰箱。请看下面的类声明。

```
@Entity
@Table(name = "fridge")
public class Fridge {
    @Id
    @GeneratedValue(strategy = IDENTITY)
    private Long id;

    private String name;

    @OneToMany(fetch = LAZY, mappedBy = "fridge")
    private List<Product> products = new ArrayList<>();
}

@Entity
@Table(name = "product")
public class Product {
    @Id
    @GeneratedValue(strategy = IDENTITY)
    private Long id;

    @Enumerated(STRING)
    private Type type;

    private int quantity;

    @ManyToOne(fetch = LAZY)
    @JoinColumn(name = "fridge_id")
    private Fridge fridge;

    public enum Type {
        POTATO, ONION, CARROT
    }
}
```

> *我正在使用*[*Spring Data JPA*](https://spring.io/projects/spring-data-jpa)*作为持久性框架。因此，那些类都是*[*Hibernate*](https://hibernate.org/)*实体。*

# 白盒测试

这种类型的测试假设我们知道一些实现细节，并可能与它们交互。系统中有 4 个 REST API 端点:

1.  创造一个新的`Fridge`。
2.  添加新的`Product`。
3.  改变`Product`数量。
4.  从`Fridge`上拆下`Product`。

假设我们想测试改变`Product`量的那个。请看下面的测试示例。

```
@SpringBootTest(webEnvironment = RANDOM_PORT)
class ProductControllerWhiteBoxTest extends IntegrationSuite {
    @Autowired
    private FridgeRepository fridgeRepository;
    @Autowired
    private ProductRepository productRepository;
    @Autowired
    private TestRestTemplate rest;

    @BeforeEach
    void beforeEach() {
        productRepository.deleteAllInBatch();
        fridgeRepository.deleteAllInBatch();
    }

    @Test
    void shouldUpdateProductQuantity() {
        final var fridge = fridgeRepository.save(Fridge.newFridge("someFridge"));
        final var productId = productRepository.save(Product.newProduct(POTATO, 10, fridge)).getId();

        assertDoesNotThrow(() -> rest.put("/api/product/{productId}?newQuantity={newQuantity}", null, Map.of(
            "productId", productId,
            "newQuantity", 20
        )));

        final var product = productRepository.findById(productId).orElseThrow();
        assertEquals(20, product.getQuantity());
    }
}
```

让我们一步一步地检查这段代码。
我们注入存储库来操作数据库中的行。然后`TestRestTemplate`开始发挥作用。这个 bean 用于发送 HTTP 请求。然后您可以看到`@BeforeEach`回调删除了数据库中的所有行。因此，每个测试都是确定性的。最后，这是测试本身:

1.  我们创建一个新的`Fridge`。
2.  然后我们创建一个数量为`10`的新`Product`，它属于新创建的`Fridge`。
3.  之后，我们调用 REST 端点将`Product`数量从`10`增加到`20`。
4.  最终，我们从数据库中选择相同的`Product`，并检查数量是否增加。

测试工作正常。无论如何，有些细微差别应该被考虑进去:

1.  尽管测试验证了整个系统的行为(也称为功能测试),但是在实现细节(即数据库)上存在耦合。
2.  测试验证的不是实际的用例。如果有人想与我们的服务交互，他们将不能直接在数据库中插入和更新行。

事实上，如果我们想从用户的角度测试系统，我们只能使用服务公开的公共 API。

# 黑盒测试

这种类型的测试意味着系统实现细节上的松散耦合。所以只能依赖公共 API(即 REST API)。

查看之前的白盒测试示例。怎么才能重构成黑盒那种？看下面的`@BeforeEach`实现。

```
@BeforeEach
void beforeEach() {
    productRepository.deleteAllInBatch();
    fridgeRepository.deleteAllInBatch();
}
```

黑盒测试不应该直接与持久性提供者交互。这意味着应该有一个单独的 REST 端点来清除所有数据。请看下面的代码片段。

```
@RestController
@RequiredArgsConstructor
@Profile("qa")
public class QAController {
    private final FridgeRepository fridgeRepository;
    private final ProductRepository productRepository;

    @DeleteMapping("/api/clearData")
    @Transactional
    public void clearData() {
        productRepository.deleteAllInBatch();
        fridgeRepository.deleteAllInBatch();
    }
}
```

现在我们有了一个特殊的控制器，它封装了所有的清算数据逻辑。如果测试只依赖于这个端点，那么随着应用程序的增长，我们可以安全地进行更改和重构方法。我们的黑盒测试不会失败。`@Profile(“qa”)`注释至关重要。我们不想公开一个可以删除生产甚至开发环境中所有用户数据的端点。因此，如果`qa` [配置文件](https://www.baeldung.com/spring-profiles)是活动的，我们就注册这个端点。我们只会在测试中使用它。

> *`*qa*`*缩写代表* [*质保*](https://www.techtarget.com/searchsoftwarequality/definition/quality-assurance) *。**

*现在我们应该重构测试方法本身。再看看下面它的实现。*

```
*@Test
void shouldUpdateProductQuantity() {
    final var fridge = fridgeRepository.save(Fridge.newFridge("someFridge"));
    final var productId = productRepository.save(Product.newProduct(POTATO, 10, fridge)).getId();

    assertDoesNotThrow(() -> rest.put("/api/product/{productId}?newQuantity={newQuantity}", null, Map.of(
        "productId", productId,
        "newQuantity", 20
    )));

    final var product = productRepository.findById(productId).orElseThrow();
    assertEquals(20, product.getQuantity());
}*
```

*有 3 个操作应该替换为直接 REST API 调用。这些是:*

1.  *创造新的`Fridge`。*
2.  *创建新的`Product`。*
3.  *检查`Product`数量是否增加。*

*看看下面的整个黑盒测试例子。*

```
*@SpringBootTest(webEnvironment = RANDOM_PORT)
@ActiveProfiles("qa")
class ProductControllerBlackBoxTest extends IntegrationSuite {
    @Autowired
    private TestRestTemplate rest;

    @BeforeEach
    void beforeEach() {
        rest.delete("/api/qa/clearData");
    }

    @Test
    void shouldUpdateProductQuantity() {
        // create new Fridge
        final var fridgeResponse =
            rest.postForEntity("/api/fridge?name={fridgeName}", null, FridgeResponse.class, Map.of(
                "fridgeName", "someFridge"
            ));
        assertTrue(fridgeResponse.getStatusCode().is2xxSuccessful(), "Error during creating new Fridge: " + fridgeResponse.getStatusCode());
        // create new Product
        final var productResponse = rest.postForEntity(
            "/api/product/fridge/{fridgeId}",
            new ProductCreateRequest(
                POTATO,
                10
            ),
            ProductResponse.class,
            Map.of(
                "fridgeId", fridgeResponse.getBody().id()
            )
        );
        assertTrue(productResponse.getStatusCode().is2xxSuccessful(), "Error during creating new Product: " + productResponse.getStatusCode());

        // call the API that should be tested
        assertDoesNotThrow(
            () -> rest.put("/api/product/{productId}?newQuantity={newQuantity}",
                null,
                Map.of(
                    "productId", productResponse.getBody().id(),
                    "newQuantity", 20
                ))
        );

        // get the updated Product by id
        final var updatedProductResponse = rest.getForEntity(
            "/api/product/{productId}",
            ProductResponse.class,
            Map.of(
                "productId", productResponse.getBody().id()
            )
        );
        assertTrue(
            updatedProductResponse.getStatusCode().is2xxSuccessful(),
            "Error during retrieving Product by id: " + updatedProductResponse.getStatusCode()
        );
        // check that the quantity has been changed
        assertEquals(20, updatedProductResponse.getBody().quantity());
    }
}*
```

*与白盒测试相比，黑盒测试的优势在于:*

1.  *该测试检查用户检索预期结果的路径。因此，验证行为变得更加健壮。*
2.  *黑盒测试对重构非常稳定。只要 API 契约保持不变，测试就不会中断。*
3.  *如果您不小心破坏了向后兼容性(例如，向现有的 REST 端点添加一个新的强制参数)，黑盒测试将会失败，您将在产品被部署到任何环境之前确定问题的方式。*

*但是，您可能已经注意到了代码中的一个小问题。这个测试相当麻烦。很难阅读和维护。如果我不加入解释性的注释，你可能会花太多时间去弄清楚发生了什么。此外，不同的场景可能会调用相同的端点，这会导致代码重复。*

*幸运的是有解决方案。*

# *OpenAPI 和代码生成*

*Spring Boot 带来了出色的 open API T4 支持。您所要做的就是添加两个依赖项。看看下面的 [Gradle](https://gradle.org/) 配置。*

```
*implementation 'org.springframework.boot:spring-boot-starter-actuator'
implementation 'org.springdoc:springdoc-openapi-ui:1.6.12'*
```

*添加完这些依赖项后，`GET /v3/api-docs`端点就可以使用 OpenAPI 规范了。*

> **[*spring doc*](https://springdoc.org/)*库附带了大量注释来精确地调优 REST API 规范。无论如何，这超出了本文的范围。***

**如果我们有 OpenAPI 规范，这意味着我们可以生成 Java 类来以类型安全的方式调用端点。更令人兴奋的是，我们可以在我们的黑盒测试中应用那些生成的类！**

**首先，让我们定义即将到来的 OpenAPI Java 客户端的需求:**

1.  **生成的类应该放入`.gitignore`中。否则，如果您的项目中有 [Checkstyle](https://checkstyle.sourceforge.io/) 、 [PMD](https://pmd.github.io/) 或[sonar cube](https://www.sonarqube.org/)，那么生成的类可能会违反一些规则。此外，如果你不把它们放到`.gitignore`中，那么每个拉请求可能会变得很大，因为事实上，即使是最轻微的修改也会导致生成的类发生很多变化。**
2.  **每个 pull 请求构建都应该保证生成的类总是最新的，符合实际的 OpenAPI 规范。**

**我们如何在构建阶段获得 OpenAPI 规范本身？最简单的方法是编写一个单独的测试，创建 Spring 上下文的 web 部分，调用`/v3/api-docs`端点，并将检索到的规范放入`build`文件夹(如果你是 Maven 用户，那么它将是`target`文件夹)。看看下面的代码示例。**

```
**@SpringBootTest(
    webEnvironment = RANDOM_PORT
)
@AutoConfigureTestDatabase
@ActiveProfiles("qa")
public class OpenAPITest {
    @Autowired
    private TestRestTemplate rest;

    @Test
    @SneakyThrows
    void generateOpenApiSpec() {
        final var response = rest.getForEntity("/v3/api-docs", String.class);
        assertTrue(response.getStatusCode().is2xxSuccessful(), "Unexpected status code: " + response.getStatusCode());
        // the specification will be written to 'build/classes/test/open-api.json'
        Files.writeString(
            Path.of(getClass().getResource("/").getPath(), "open-api.json"),
            response.getBody()
        );
    }
}**
```

> ***`*@AutoConfigureTestDatabase*`*配置内存数据库(如* [*H2*](https://www.h2database.com/) *)，如果类路径中有内存数据库的话。由于数据库提供者不影响结果 OpenAPI 规范，我们可以通过不使用*[*test containers*](https://www.testcontainers.org/)*来使测试运行得更快一些。****

**现在我们有了结果规范。我们如何基于它生成 Java 类呢？我们有另一个 Gradle 插件。看看下面的`build.gradle`配置。**

```
**plugins {
    ...
    id "org.openapi.generator" version "6.2.0"
}

...

openApiGenerate {
    inputSpec = "$buildDir/classes/java/test/open-api.json".toString()
    outputDir = "$rootDir/open-api-java-client".toString()
    apiPackage = "com.example.demo.generated"
    invokerPackage = "com.example.demo.generated"
    modelPackage = "com.example.demo.generated"
    configOptions = [
            dateLibrary    : "java8",
            openApiNullable: "false",
    ]
    generatorName = 'java'
    groupId = "com.example.demo"
    globalProperties = [
            modelDocs: "false"
    ]
    additionalProperties = [
            hideGenerationTimestamp: true
    ]
}**
```

> **在这篇文章中，我将向你展示如何配置相应的 [*Gradle 插件*](https://github.com/OpenAPITools/openapi-generator/tree/master/modules/openapi-generator-gradle-plugin) *。不管怎样，还有* [*Maven 插件*](https://github.com/OpenAPITools/openapi-generator/tree/master/modules/openapi-generator-maven-plugin) *也是一样，方法不会有太大不同。***

**关于 OpenAPI 生成器插件有一个重要的细节。它创建整个 Gradle/Maven/SBT 项目(包含`build.gradle`、`pom.xml`和`build.sbt`文件)，而不仅仅是 Java 类。因此，我们将`outputDir`属性设置为`$rootDir/open-api-java-client`。因此，生成的 Java 类进入 Gradle 子项目。**

**我们还应该将`open-api-java-client`目录标记为`settings.gradle`中的子项目。请看下面的代码片段。**

```
**rootProject.name = 'demo'
include 'open-api-java-client'**
```

**要生成 OpenAPI Java 客户端，您只需运行这些 Gradle 命令:**

```
**gradle test --tests "com.example.demo.controller.OpenAPITest.generateOpenApiSpec"
gradle openApiGenerate**
```

# **应用 Java 客户端**

**现在让我们尝试一下我们全新的 Java 客户端。为了方便起见，我们将创建一个单独的`@TestComponent`。请看下面的代码片段。**

```
**@TestComponent
public class TestRestController {
    @Autowired
    private Environment environment;

    public FridgeControllerApi fridgeController() {
        return new FridgeControllerApi(newApiClient());
    }

    public ProductControllerApi productController() {
        return new ProductControllerApi(newApiClient());
    }

    public QaControllerApi qaController() {
        return new QaControllerApi(newApiClient());
    }

    private ApiClient newApiClient() {
        final var apiClient = new ApiClient();
        apiClient.setBasePath("http://localhost:" + environment.getProperty("local.server.port", Integer.class));
        return apiClient;
    }
}**
```

**最后，我们可以重构我们的黑盒测试。看看下面的最终版本。**

```
**@SpringBootTest(webEnvironment = RANDOM_PORT)
@ActiveProfiles("qa")
@Import(TestRestControllers.class)
class ProductControllerBlackBoxGeneratedClientTest extends IntegrationSuite {
    @Autowired
    private TestRestControllers rest;

    @BeforeEach
    @SneakyThrows
    void beforeEach() {
        rest.qaController().clearData();
    }

    @Test
    void shouldUpdateProductQuantity() {
        final var fridgeResponse = assertDoesNotThrow(
            () -> rest.fridgeController()
                .createNewFridge("someFridge")
        );
        final var productResponse = assertDoesNotThrow(
            () -> rest.productController()
                .createNewProduct(
                    fridgeResponse.getId(),
                    new ProductCreateRequest()
                        .quantity(10)
                        .type(POTATO)
                )
        );

        assertDoesNotThrow(
            () -> rest.productController()
                .updateProductQuantity(productResponse.getId(), 20)
        );

        final var updatedProduct = assertDoesNotThrow(
            () -> rest.productController()
                .getProductById(productResponse.getId())
        );
        assertEquals(20, updatedProduct.getQuantity());
    }
}**
```

**如您所见，该测试更具声明性。此外，API 契约变成静态类型，参数验证在编译时进行！**

# **`.gitignore`警告和单独的测试源**

**我说过我们应该把生成的类放到`.gitignore`中。然而，如果您将`open-api-java-client/src`目录标记为 Git 未索引的，那么您会突然意识到您的测试不能在 CI 环境中编译。原因是生成 OpenAPI 规范(即`open-api.json`文件)的过程也是一个单独的测试。即使你告诉 Gradle 直接运行一个单独的测试，它也会编译`src/test`目录中的所有内容。最终，测试不会成功编译。**

**谢天谢地，这个问题很容易解决。Gradle 提供了[源集](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.SourceSet.html)。它是一个逻辑组，将代码分成可以独立编译的独立模块。**

**首先，让我们添加 [gradle-testsets](https://github.com/unbroken-dome/gradle-testsets-plugin) 插件，并定义一个包含`OpenAPITest`文件的独立测试源。它生成了`open-api.json`规范。看看下面的代码示例。**

```
**plugins {
    ...
    id "org.unbroken-dome.test-sets" version "4.0.0"
}

...

testSets {
    openApiGenerator
}

tasks.withType(Test) {
    group = 'verification'
    useJUnitPlatform()
    testLogging {
        showExceptions true
        showStandardStreams = false
        showCauses true
        showStackTraces true
        exceptionFormat "full"
        events("skipped", "failed", "passed")
    }
}

openApiGenerator.outputs.upToDateWhen { false }

tasks.named('openApiGenerate') {
    dependsOn 'openApiGenerator'
}**
```

**`testSets`块声明了一个名为`openApiGenerator`的新源集。这意味着 Gradle 将`src/openApiGenerator`目录视为另一个测试源。**

**`tasks.withType(Test)`声明也很重要。我们需要告诉 Gradle 每一个`Test`类型的任务(也就是`test`任务本身和`openApiGenerator`任务)都应该使用 JUnit 运行。**

**为了方便起见，我选择了`upToDateWhen`选项。这意味着生成`open-api.json`文件的测试将总是按需运行，而不会被缓存。**

**最后一块定义了在生成 OpenAPI Java 客户端之前，我们应该提前更新规范。**

**现在我们只需要将`OpenAPITest`移动到`src/openApiGenerator`目录中，并对`build.gradle`中的`openApiGenerate`任务做一点小小的改变。请看下面的代码片段。**

```
**openApiGenerate {
    // 'test' directory should be replaced with 'openApiGenerator'
    inputSpec = "$buildDir/classes/java/openApiGenerator/open-api.json".toString()
    ....
}**
```

**最后，您可以用这两个命令构建整个项目。**

# **结论**

**黑盒测试是应用程序开发过程的重要部分。尝试一下，你会注意到测试场景变得更有代表性。此外，黑盒测试也是 API 的很好的文档。你甚至可以应用 [Spring REST 文档](https://spring.io/guides/gs/testing-restdocs/)并生成一份对 API 用户和 QA 工程师都有用的精美手册。**

**如果您有任何问题或建议，请在下面留下您的评论。感谢阅读！**

# **资源**

1.  **[带有代码示例的存储库](https://github.com/SimonHarmonicMinor/spring-boot-black-box-testing-example)**
2.  **[春季数据 JPA](https://spring.io/projects/spring-data-jpa)**
3.  **[冬眠](https://hibernate.org/)**
4.  **[弹簧轮廓](https://www.baeldung.com/spring-profiles)**
5.  **[质量保证](https://www.techtarget.com/searchsoftwarequality/definition/quality-assurance)**
6.  **[OpenAPI](https://swagger.io/specification/)**
7.  **[格雷尔](https://gradle.org/)**
8.  **SpringDoc**
9.  **[检查样式](https://checkstyle.sourceforge.io/)**
10.  **[PMD](https://pmd.github.io/)**
11.  **[SonarQube](https://www.sonarqube.org/)**
12.  **[H2 数据库](https://www.h2database.com/)**
13.  **[测试容器](https://www.testcontainers.org/)**
14.  **[OpenAPI 生成器 Gradle 插件](https://github.com/OpenAPITools/openapi-generator/tree/master/modules/openapi-generator-gradle-plugin)**
15.  **[OpenAPI 生成器 Maven 插件](https://github.com/OpenAPITools/openapi-generator/tree/master/modules/openapi-generator-maven-plugin)**
16.  **[梯度源集合](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.SourceSet.html)**
17.  **[Gradle testsets 插件](https://github.com/unbroken-dome/gradle-testsets-plugin)**
18.  **[春假文件](https://spring.io/guides/gs/testing-restdocs/)**