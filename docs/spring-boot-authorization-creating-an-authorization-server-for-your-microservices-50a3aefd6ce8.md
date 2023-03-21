# Spring Boot 授权:为您的微服务创建授权服务器

> 原文：<https://medium.com/javarevisited/spring-boot-authorization-creating-an-authorization-server-for-your-microservices-50a3aefd6ce8?source=collection_archive---------0----------------------->

本文详细解释了如何使用集中式授权服务器和 API 网关实现身份验证机制。

# 什么是 JWT 代币，为什么使用它？

> JSON Web Token 是一个提议的互联网标准，用于创建带有可选签名和/或可选加密的数据，其有效负载包含断言一些声明的 JSON。使用私有秘密或公共/私有密钥对令牌进行签名。

它们可以用来实现无状态认证。它们可以跨实例或多个服务共享，并且可以包含对它们进行身份验证所需的详细信息。因此，不需要设置单独的资源来维护会话或将令牌/会话细节存储在单独的数据库/高速缓存中。

## 使用的架构

[![](img/db1f1e0b416dac792a971e3728434841.png)](https://javarevisited.blogspot.com/2018/01/how-to-learn-spring-core-spring-mvc-boot-security-framework.html#axzz7TNldBrH4)

对安全资源的请求的一般流程

使用的一般设计是:

*   将用于创建和验证 JWT 令牌的集中式授权服务器。
*   API 网关将充当应用程序的入口点。这将把请求路由到相应的微服务。
*   网关过滤器将被添加到路由中，用于验证对安全资源的请求中的 JWT 令牌。这将对授权服务器进行 API 调用，以验证令牌并获得用户的用户名和权限。这些详细信息将被转发给标头部分中的下游服务进行验证。
*   Eureka 发现客户端将用于服务发现。

## 认证流程

[![](img/ef1accb1c5ece1ad86974598d619e2b2.png)](https://javarevisited.blogspot.com/2018/01/how-http-basic-authentication-works-in.html#axzz6hhgr3Uqg)

服务的一般流程是:

*   用户 user 可以通过使用用户名和密码调用**/登录** ( **发布**)端点来登录/创建认证令牌。这将返回不记名令牌作为头参数。
*   从响应标头中获取授权令牌。这应该作为对安全资源请求的格式为**载体*访问令牌*** 的 ***授权*** 报头的值传递。
*   对于受保护的资源，将调用自定义的 ***网关过滤器(*AuthenticationPrefilter*)***)。这将对认证服务中的 **/api/v1/validateToken** 端点进行 API 调用，认证服务验证令牌并在成功验证的情况下发送用户名和授予用户的权限作为响应。
*   如果令牌有效，则用户的用户名和[权限](https://javarevisited.blogspot.com/2013/07/role-based-access-control-using-spring-security-ldap-authorities-mapping-mvc.html)在被转发到用户请求的资源之前被附加到头部请求。
*   其他微服务(例如:用户服务)将有一个从 **spring-security** 中的 **OncePerRequestFilter** 扩展而来的授权过滤器，它将使用**usernamepasswordtauthenticationtoken**类创建一个**身份验证**对象(使用来自 header 的 *username* 和 *SimpleGrantedAuthority* 作为输入，密码为空)。
*   如果用户有权访问资源，则允许该请求。否则，向客户端返回 401 未授权/ 403 禁止响应。

# 设置服务:

## 尤里卡服务器

*   使用 pom 文件中的 Spring-cloud-starter-网飞-尤里卡-服务器依赖关系，使用[**Spring initializr**](https://start.spring.io/)**创建一个 Spring Boot 应用程序。spring-cloud-dependencies 也必须添加到依赖项管理下以获得支持。**
*   **现在，通过将[**@ EnableEurekaServer**注释](https://www.java67.com/2018/12/top-5-spring-cloud-annotations-for-java.html)添加到应用程序的主类中，可以将应用程序配置为作为 Eureka 服务器启动。**
*   **以下属性被添加到属性文件中以支持 Eureka 服务器。**

```
spring.application.name=naming-server
server.port=8761

eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
eureka.instance.prefer-ip-address=true
```

*   **现在，尤里卡服务器启动后可以在[**http://localhost:8761/**](http://localhost:8761/)**中访问。它将显示向其注册的服务列表。****

****[![](img/1595a2404a40698ae92e3ced8139623b.png)](https://www.java67.com/2021/01/spring-cloud-interview-questions-with-answers-java.html)****

## ****授权服务****

*   ****创建一个 Spring Boot 应用***spring-boot-starter-security***，***spring-boot-starter-web*，**，*spring-cloud-starter-sleuth*，**，*spring-cloud-starter-config*，**，***spring-cloud-starter-eureka-client*，**********
*   ****spring-boot-starter-security***用于授权和认证目的，而**spring-boot-starter-data-MongoDB**和**spring-boot-starter-data-JPA**用于访问 mongodb 集合中的凭证。创建和验证 jwt 令牌需要依赖关系***io . jsonwebtoken:jjwt***。*
*   *为了使用数据库中的凭证启用身份验证，必须提供来自 [Spring Security](/javarevisited/top-10-courses-to-learn-spring-security-and-oauth2-with-spring-boot-for-java-developers-8f0222d6066d) 的**userdailsservice**类的自定义实现。这将实现***loaduserbysusername()***方法，该方法将用于从[数据库](/javarevisited/top-5-sql-and-database-courses-to-learn-online-48424533ac61)获取用户凭证，并返回 **UserDetails** 的实例(来自 spring security)。*

```
*@Service
public class ApplicationUserDetailsService implements UserDetailsService {

    @Autowired
    private UsersService usersService;

    @Override
    public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {
        return new ApplicationUsers(usersService.getByUsrName(s).orElseThrow(() -> new UsernameNotFoundException("Username Not Found")));
    }
}*
```

*   *创建了一个定制的 **UserDetails** 类的实现来将定制的数据库对象映射到 spring security 所需的格式。*
*   *创建了一个新的配置类，扩展了 [spring security](https://javarevisited.blogspot.com/2017/05/how-to-enable-spring-security-in-java-web-application.html) 中的**WebSecurityConfigurerAdapter**类。*

*   *这里，DaoAuthenticationProvider 的 Bean 是用密码编码器和我们创建的 UserDetailsService 实现的实例创建的。*
*   *这又用于通过配置**AuthenticationManagerBuilder**来使用我们创建的自定义身份验证提供程序，从而覆盖***configure(AuthenticationManagerBuilder auth)***方法。*
*   *现在，为了每次都使用 JWT/承载令牌而不是基于用户名和密码的身份验证，我们必须配置两个过滤器:一个用于生成承载令牌，另一个用于验证它。*
*   *JWT 生成过滤器:这是作为 Spring Security 中的**usernamepasswordtauthenticationfilter**的子类创建的。在这里，我们将覆盖 ***尝试验证()*** 方法 d 来验证凭证，并覆盖 ***成功验证()*** 来创建 JWT 令牌，以防授权成功。*

*   *JWT 验证过滤器:该过滤器是通过扩展由 [spring security](https://www.java67.com/2012/08/spring-interview-questions-answers.html) 提供的 **OncePerRequestFilter** 类创建的，并由配置中的**addFilterAfter()**配置，以便在 JWT 生成过滤器之后调用。*

***附加功能:***

*![](img/468dee9c935f5a77ce757d46d2a7fde6.png)*

*生成 UUID 和存储令牌所涉及的额外过程*

*通常，JWT 令牌的内容一旦创建就不能修改。但是，它可以很容易地被解密，内容可以被任何人读取。*

*因此，我们可以返回一个为认证请求生成的随机 UUID，并将对它们的引用存储在 redis 缓存中，而不是将 JWT 令牌完全返回给用户。*

*因此，对于面向客户端的应用程序，生成的 UUID 将被返回并使用，而 JWT 令牌可以在服务之间共享，以用于授权/认证目的。*

## *API 网关*

*   *使用 spring-cloud-starter-gateway、spring-cloud-starter-config 和 spring-cloud-starter-网飞-尤里卡-客户端依赖项创建一个 Springboot 应用程序,这是使用尤里卡客户端设置 API 网关所需要的。*
*   *云配置服务器和 Eureka 服务器的详细信息被添加到配置文件中。*

```
*debug: true
logging:
  level:
    org.springframework.cloud.gateway: DEBUG
    reactor.netty.http.client: DEBUG
server:
  port: '8765'
spring:
  cloud:
    config:
      profile: dev
    gateway:
      discovery.locator.enabled: true
  config:
    import: optional:configserver:http://clouduser:configserver705!@localhost:8888
  application:
    name: api-gateway
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss
management:
  endpoints:
    web:
      exposure:
        include: '*'
eureka:
  client:
    serviceUrl:
      defaultZone: http://eurekauser:eureka124!@localhost:8761/eureka
  instance:
    prefer-ip-address: 'true'*
```

*   *将 **@EnableFeignClients** 注释添加到 Api Gateway 应用程序的主类中，这样它将连接到已配置的 Eureka 服务器。*
*   *配置网关过滤器，通过调用授权服务器中的 **/validateToken** 端点来验证请求中的承载令牌。这个类是通过扩展 Spring-API Gateway 提供的**AbstractGatewayFilterFactory**类并覆盖其***apply(Config Config)***方法创建的，该方法为 **GatewayFilter** 返回一个对象。*

***配置路线***

*这些路由在一个单独的配置文件中进行配置，上面创建的 **GatewayFilter** 类作为一个过滤器附加到这些路由上。*

## *用户服务(配置 Spring 安全性和资源端点)*

*   *使用 Spring 初始化器创建一个用户服务项目。这将有用户资源，是一个后端服务的例子。*
*   *创建扩展 WebSecurityConfigurerAdapter 类的配置类，覆盖它的***void configure(http security http)
    throws Exception***。这里，自定义过滤器被定义为执行 [Spring Boot](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e) 中的**usernamepasswordtauthenticationfilter**过滤器。*

*   ***JWTVerifierFilter** 过滤器将检查是否有任何用户名和权限数据被添加到请求的 header 参数中(通过 API 网关),并将通过使用**usernamepasswordtauthenticationtoken**类创建一个认证对象。*
*   *然后使用 **SecurityContextHolder** 类将其设置为 Spring Security 的[安全上下文。](https://javarevisited.blogspot.com/2018/02/what-is-securitycontext-and-SecurityContextHolder-Spring-security.html)*

*因此，如果请求资源/端点的用户具有对它们的访问/授权，则该请求将被允许到资源/端点。下面是用户资源中 **GET** 方法的示例块。*

***使用的常见依赖关系:***

```
*<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>*
```

*为了使用 Spring Cloud 库，比如 Config Server 和 Eureka Clients，必须在相应的 POM 文件中的依赖关系管理下添加一个单独的部分。*

```
*<properties>
   <spring-cloud.version>2020.0.3</spring-cloud.version>
</properties>
<dependencyManagement>
   <dependencies>
      <dependency>
         <groupId>org.springframework.cloud</groupId>
         <artifactId>spring-cloud-dependencies</artifactId>
         <version>${spring-cloud.version}</version>
         <type>pom</type>
         <scope>import</scope>
      </dependency>
   </dependencies>
</dependencyManagement>*
```

# *JWT 令牌用法*

## *请求的验证:*

*   *这是在最后公开的登录 API，可用于对用户进行身份验证并生成 JWT 令牌。*

*[![](img/405d01af6af9a8dd587949f9ba31906d.png)](https://javarevisited.blogspot.com/2016/09/top-5-json-library-in-java-JEE.html)*

*   *在内部使用的***validate token***API，用于验证请求中发送的令牌并获取令牌的授权细节。(该 API 稍后将被阻止外部访问)。*

*[![](img/cd87b4af721e0ee0d488c1905ac452d5.png)](https://javarevisited.blogspot.com/2018/02/how-to-parse-json-with-date-field-in-java-jackson-example.html)*

*   *当使用有效的授权令牌通过 API 网关调用时，用户服务中的安全资源。*

*[![](img/69ac4cc41164bfbcebcda724b9b0c498.png)](https://www.java67.com/2019/09/3-ways-to-ignore-null-fields-in-json-java-jackson.html)*

*   *在没有有效授权令牌的情况下，通过 API 网关调用用户服务中的安全资源。*

*[![](img/715c5ba7e1dc74c204e5503f793e7be9.png)](https://javarevisited.blogspot.com/2022/03/3-examples-to-parse-json-in-java-using.html)*

*   *由授权服务生成的样本 JWT 令牌。*

*![](img/6be7a779e58b8ed162eb2a73a020ec4f.png)*

***更新**:你可以在[https://github.com/Vicky-cmd/Authentication-Service.git](https://github.com/Vicky-cmd/Authentication-Service.git)找到完整项目*

*所以这就是目前这个话题的全部内容。评论总是受欢迎的。✌️*