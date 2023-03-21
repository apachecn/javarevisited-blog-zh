# Java 中的六边形架构

> 原文：<https://medium.com/javarevisited/hexagonal-architecture-in-java-9031d3570d15?source=collection_archive---------2----------------------->

## Java 中六边形架构的一个实例

![](img/ebc74c3d70de4686b40a17a33aae4c39.png)

# 1.概观

在本教程中，我们将看看 Java 中的**六边形架构。为了进一步说明这一点，我们将创建一个 Spring Boot 应用程序。**

# 2.六角形建筑

六边形体系结构描述了围绕领域逻辑设计软件应用程序的模式。**六边形描述了应用程序的** **核心，由领域对象和应用程序的用例**组成。

六边形的边缘为六边形的外部提供了**入站和出站端口**，如 web 界面、数据库等。所以，在这种[软件架构](/javarevisited/top-5-courses-to-learn-software-architecture-in-2020-best-of-lot-5d34ebc52e9)、**中，组件之间的所有**、**依赖都指向域**对象。

因此，只有使用**端口和适配器**才能实现核心应用程序和外部部分之间的通信。

在下面的章节中，我们可以深入探讨六边形架构的不同层次。

# 3.域对象

**域对象是应用程序的核心部分。**它既可以有状态，也可以有行为。然而，它没有任何外部依赖性。因此其他层中的任何变化都不会影响域对象。

只有当业务需求发生变化时，域对象才会发生变化。因此，这是[软件设计](/javarevisited/25-software-design-interview-questions-to-crack-any-programming-and-technical-interviews-4b8237942db0)的[实体原则](/javarevisited/10-oop-design-principles-you-can-learn-in-2020-f7370cccdd31)中单一责任原则的一个例子。

首先，让我们创建一个域对象`*Product*`，它构成了应用程序的核心。它包含产品相关信息和业务验证:

```
public class Product {

    private Integer productId;
    private String type;
    private String description;

    public Product() {
        super();
    }

    public Product(Integer productId, String type, String description) {
        this.productId = productId;
        this.type = type;
        this.description = description;
    }
    //getters and setters
}
```

# 4.港口

端口是允许入站和出站流量的接口。因此，应用程序的核心部分使用专用端口与外部部分通信。

# 4.1.入境港口

**入站端口将核心应用程序暴露给外部**。它是一个可以被外部组件调用的接口。这些调用入站端口的外部组件被称为**主适配器或输入适配器。**

让我们定义一个接口`*ProductService*`，它是入站端口:

```
public interface ProductService {

    List<Product> getProducts();

    Product getProductById(Integer productId);

    Product addProduct(Product product);

    Product removeProduct(Integer productId);
}
```

# 4.2.出站端口

**出站端口允许核心应用**的外部功能。它是一个接口，使核心应用的用例能够与外部通信，如[数据库](/javarevisited/7-free-courses-to-learn-database-and-sql-for-programmers-and-data-scientist-e7ae19514ed2)访问。因此，出站端口由称为**次级或输出适配器**的外部组件实现。

```
public interface ProductRepository {

    List<Product> getProducts();

    Product getProductById(Integer productId);

    Product addProduct(Product product);

    Product removeProduct(Integer productId);
}
```

# 5.适配器

适配器是六角形结构的外部部分。因此，它们只通过使用入站和出站端口与核心应用程序进行交互。

# 5.1.主要适配器

主适配器也称为输入或驱动适配器。因此，**它们通过使用入站端口**调用核心应用的相应用例来驱动应用。例如，主适配器是 REST APIs 或 web 接口。

让我们定义一个`*ProductController*`类作为我们的主适配器。特别是，它是一个 [REST 控制器](/javarevisited/top-5-books-and-courses-to-learn-restful-web-services-in-java-using-spring-mvc-and-spring-boot-79ec4b351d12)，为创建和访问产品提供端点。随后，它使用入站端口服务与核心应用程序进行交互:

```
@RestController
@RequestMapping("/api/v1/product")
public class ProductController {

    private ProductService productService;

    @Autowired
    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping
    public ResponseEntity<List<Product>> getProducts() {
        return new ResponseEntity<>(productService.getProducts(), HttpStatus.*OK*);
    }

    @GetMapping("/{productId}")
    public ResponseEntity<Product> getProductById(@PathVariable Integer productId) {
        return new ResponseEntity<>(productService.getProductById(productId), HttpStatus.*OK*);
    }

    @PostMapping
    public ResponseEntity<Product> addProduct(@RequestBody Product product) {
        return new ResponseEntity<>(productService.addProduct(product), HttpStatus.*CREATED*);
    }

    @DeleteMapping("/{productId}")
    public ResponseEntity<Product> removeProduct(@PathVariable Integer productId) {
        return new ResponseEntity<>(productService.removeProduct(productId), HttpStatus.*OK*);
    }
}
```

# 5.2.辅助适配器

次级适配器也称为输出或驱动适配器。**这些是出站端口接口**的实现。核心应用程序的用例使用输出端口调用辅助适配器。例如，辅助适配器是到数据库和外部 API 调用的连接。

让我们定义一个类`*ProductRepositoryImplementation*`作为我们的辅助适配器。特别是，这个类实现了出站端口接口`*ProductRepository*`，并允许核心应用程序访问数据库:

```
@Repository
public class ProductRepositoryImplementation implements ProductRepository {

    private static final Map<Integer, Product> *productMap* = new HashMap<Integer, Product>(0);

    @Override
    public List<Product> getProducts() {
        return new ArrayList<Product>(*productMap*.values());
    }

    @Override
    public Product getProductById(Integer productId) {
        return *productMap*.get(productId);
    }

    @Override
    public Product addProduct(Product product) {
        *productMap*.put(product.getProductId(), product);
        return product;
    }

    @Override
    public Product removeProduct(Integer productId) {
        if(*productMap*.get(productId)!= null){
            Product product = *productMap*.get(productId);
            *productMap*.remove(productId);
            return product;
        } else
            return null;

    }
}
```

**适配器为应用提供了灵活性，而不会影响核心应用逻辑**。如果除了现有客户端之外，新的客户端也可以使用该应用程序，那么我们可以将新的客户端添加到入站端口。此外，如果应用程序需要不同的[数据库](/javarevisited/top-10-free-courses-to-learn-microsoft-sql-server-and-oracle-database-in-2020-6708afcf4ad7)，我们可以添加一个新的辅助适配器来实现相同的出站端口。

# 6.核心应用程序的使用案例

**核心应用的用例是六角形架构**的内部部分。它们是入站端口的特定用例实现。因此，**它包含了所有用例特定的业务规则验证和逻辑**。用例没有类似于域对象的外部依赖。

让我们定义一个提供特定用例实现的类`*ProductServiceImplementation*`:

```
@Service
public class ProductServiceImplementation implements ProductService {
    @Autowired
    private ProductRepository productRepository;

    @Override
    public List<Product> getProducts() {
        return productRepository.getProducts();
    }

    @Override
    public Product getProductById(Integer productId) {
        return productRepository.getProductById(productId);
    }

    @Override
    public Product addProduct(Product product) {
        return productRepository.addProduct(product);
    }

    @Override
    public Product removeProduct(Integer productId) {
        return productRepository.removeProduct(productId);
    }
}
```

# 7.结论

与分层架构相比，六边形架构有几个优点:

*   它**通过分离应用的内部和外部部分来简化架构** **设计**
*   核心业务逻辑与任何外部依赖相隔离，这有助于**实现高度的解耦**
*   **端口允许** **灵活地连接到新网络客户端或数据库形式的新适配器**

六边形架构可能是设计简单 CRUD 应用程序的开销。然而，当我们设计一个领域驱动的应用程序时，这个架构是很有用的。

这些例子的代码可以在 Github 的[中找到。](https://github.com/anirban99/hexagonal-architecture)

*原载于* [*阿尼尔班的科技博客*](https://theanirban.dev/)