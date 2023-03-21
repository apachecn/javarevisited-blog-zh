# 在一个 Spring Boot 应用程序中配置多个数据源— Postgres 和 Oracle

> 原文：<https://medium.com/javarevisited/configure-multiple-data-sources-postgres-oracle-in-a-spring-boot-application-72b28a9e6ea9?source=collection_archive---------0----------------------->

最近，我需要将一个 spring boot 应用程序连接到两个不同的数据库。详情如下

1.  Postgresql 13.1，主机服务器 1，端口 5432，数据库— mydb1
2.  Oracle 12c，主机—服务器 2，端口 1521，数据库— mydb2，服务名— mydbservice

步骤-

1.  将以下依赖项添加到 pom.xml

```
<dependency>
   <groupId>com.oracle.database.jdbc</groupId>
   <artifactId>ojdbc8</artifactId>
   <scope>runtime</scope>
  </dependency><dependency>
   <groupId>org.postgresql</groupId>
   <artifactId>postgresql</artifactId>
   <scope>runtime</scope>
  </dependency><dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-configuration-processor</artifactId>
  </dependency>
```

2.修改应用程序.属性文件

```
spring.jpa.show-sql = true
spring.jpa.generate-ddl=false //ddl disabledspring.db1.datasource.username=pguser
spring.db1.datasource.password=pguser
spring.db1.datasource.jdbc-url=jdbc:postgresql://server1:5432/mydb1
spring.db1.datasource.driver-class-name=org.postgresql.Driverspring.db2.datasource.username=orauser
spring.db2.datasource.password=orauser
spring.db2.datasource.jdbc-url=jdbc:oracle:thin:[@server](http://twitter.com/server1)2:1521:mydbservice
spring.db2.datasource.driver-class-name=oracle.jdbc.OracleDriver**## Note : For oracle, we specify database name in entity class
## using annotatiton @Table(schema="mydb2", name="mytable")**
```

**3。确保连接到 mydb1 或 mydb2 的存储库类应该属于不同的包。**模型、[服务](https://javarevisited.blogspot.com/2017/11/difference-between-component-service.html)和[控制器](https://javarevisited.blogspot.com/2017/08/difference-between-restcontroller-and-controller-annotations-spring-mvc-rest.html#ixzz6OYNB9oii)类可以在同一个包中。

在这里，我的文件夹详细信息如下:

*   应用程序名称— myapp
*   GroupId — com.example
*   ArtifactId- myapp
*   包含 main()方法的基础包— com.example.myapp
*   每个数据库的存储库类的单独包-
    **com . example . myapp . db 1 repo，
    com . example . myapp . db 2 repo**
*   模型类的包— com.example.myapp.domain
*   控制器类包— com.example.myapp.rest
*   服务类包-com . example . myapp . Service
*   配置类包— com.example.myapp.config

4.创建两个配置类，为每个数据库源配置一个配置。
在 config 类中，我们将把域和存储库包配置到特定的数据源。

Db1Config.class

```
// db1 is our primary databasepackage com.example.myapp.config; [@Configuration](http://twitter.com/Configuration)
[@EnableJpaRepositorie](http://twitter.com/EnableJpaRepositorie)s(
entityManagerFactoryRef = "db1EntityMgrFactory", 
transactionManagerRef = "db1TransactionMgr", 
**basePackages = "com.example.myapp.db1repo")** [@EnableTransactionMan](http://twitter.com/EnableTransactionMan)agement
public class Db1Config { [@Bean](http://twitter.com/Bean)(name = "datasource1")
[@ConfigurationPropert](http://twitter.com/ConfigurationPropert)ies(prefix = "spring.db1.datasource")
[@Primary](http://twitter.com/Primary)
public DataSource dataSource() {
  return DataSourceBuilder.create().build();
 }[@Bean](http://twitter.com/Bean)(name = "db1EntityMgrFactory")
[@Primary](http://twitter.com/Primary)
public LocalContainerEntityManagerFactoryBean db1EntityMgrFactory(
   final EntityManagerFactoryBuilder builder,
   [@Qualifier](http://twitter.com/Qualifier)("datasource1") final DataSource dataSource) {
  return builder
    .dataSource(dataSource)
   .packages("com.example.myapp.domain")
    .persistenceUnit("db1")
    .build();
 } [@Bean](http://twitter.com/Bean)(name = "db1TransactionMgr")
[@Primary](http://twitter.com/Primary)
public PlatformTransactionManager db1TransactionMgr(
   [@Qualifier](http://twitter.com/Qualifier)("db1EntityMgrFactory") final EntityManagerFactory entityManagerFactory) {
  return new JpaTransactionManager(entityManagerFactory);
 }}
```

Db2Config.class

```
package com.example.myapp.config; @[Configuration](http://twitter.com/Configuration)
@[EnableJpaRepositorie](http://twitter.com/EnableJpaRepositorie)s(
entityManagerFactoryRef = "db2EntityMgrFactory", transactionManagerRef = "db2TransactionMgr", 
**basePackages = "com.example.myapp.db2repo")** @[EnableTransactionMan](http://twitter.com/EnableTransactionMan)agement
public class EpinetConfig { @[Bean](http://twitter.com/Bean)(name = "datasource2")
@[ConfigurationPropert](http://twitter.com/ConfigurationPropert)ies(prefix = "spring.db2.datasource")
public DataSource dataSource() {
  return DataSourceBuilder.create().build();
 } @[Bean](http://twitter.com/Bean)(name = "db2EntityMgrFactory")
public LocalContainerEntityManagerFactoryBean db2EntityMgrFactory ( 
  final EntityManagerFactoryBuilder builder,
  @[Qualifier](http://twitter.com/Qualifier)("datasource2") final DataSource dataSource) {final Map<String, String> properties = new HashMap<>();
properties.put("hibernate.physical_naming_strategy",
    "org.springframework.boot.orm.jpa.hibernate.SpringPhysicalNamingStrategy");return builder
    .dataSource(dataSource)
    .properties(properties)
    .packages("com.example.domain")
    .persistenceUnit("db2")
    .build();
 }@[Bean](http://twitter.com/Bean)(name = "db2TransactionMgr")
 public PlatformTransactionManager db2TransactionMgr(   @Q[ualifier](http://twitter.com/Qualifier)("db2EntityMgrFactory") final EntityManagerFactory entityManagerFactory) {
  return new JpaTransactionManager(entityManagerFactory);
 }}
```

> 注意:
> 
> 在 Db2Config 类中，我们已经明确指定了**hibernate . physical _ naming _ strategy**的属性值。这样做是因为在 jpa hibernate 查询到 [**oracle** 数据库](/javarevisited/8-free-oracle-database-and-sql-courses-for-beginners-f4e9b25b33c4)的过程中，实体类属性名称不会自动从 camelcase 转换到 snakecase。
> 
> 对于 [postgresql](/javarevisited/7-best-free-postgresql-courses-for-beginners-to-learn-in-2021-3bf369d73794) ，属性名从 camelcase 到 snakecase 的默认转换工作正常，因此没有添加到 Db1Config 类中。如果在实体类中使用@Column(name="first_name ")这样的注释显式指定了列名，则可以忽略这一点。

5.连接到 [Postgresql 数据库的示例域类。](https://javarevisited.blogspot.com/2020/02/top-5-courses-to-learn-postgresql-in.html)

```
package com.example.myapp.domain;@Entity
public class Student {

    @Id
    private Long id;
    private String firstName;
    private String lastName;
    // constructors and getter setter methods
}
```

6.连接到 [Oracle 数据库](https://javarevisited.blogspot.com/2021/05/top-5-oracle-database-and-plsql-online-courses.html)的示例域类

```
package com.example.myapp.domain;@Entity
@Table(schema = "mydb2", name = "teacher")
public class Teacher {

    @Id
    private Long id;
    private String firstName;
    private String lastName;
    // constructors and getter setter methods
}
```

就是这样。

资源:

[](https://examples.javacodegeeks.com/how-to-configure-multiple-data-sources-in-a-spring-boot-application/) [## 如何在 Spring Boot 应用程序中配置多个数据源

### 欢迎，在本教程中，我们将看到如何在单个引导应用程序中配置多个数据源。在这里我们…

examples.javacodegeeks.com](https://examples.javacodegeeks.com/how-to-configure-multiple-data-sources-in-a-spring-boot-application/) [](https://stackoverflow.com/questions/39001044/hibernate-improvednamingstrategy-doesnt-work) [## 休眠改进命名策略不起作用

### 我有一个用 snakecase 命名的列和用 camelcase 命名的列的实体的数据库。

stackoverflow.com](https://stackoverflow.com/questions/39001044/hibernate-improvednamingstrategy-doesnt-work) [](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e) [## 我最喜欢的 2021 年学习 Spring Boot 的课程——最好的

### 大家好，如果你有兴趣学习 Spring Boot 并寻找一些很棒的资源，如书籍，教程…

medium.com](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e)