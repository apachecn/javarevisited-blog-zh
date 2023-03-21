# 如何在一个 Spring boot 应用中加密秘密？

> 原文：<https://medium.com/javarevisited/how-to-encrypt-secrets-in-an-spring-boot-application-57a60c8abaa7?source=collection_archive---------0----------------------->

![](img/19d8dada17aec3f9cef41a2ba24a1e20.png)

更多的时候，我们习惯于在应用程序的属性文件中以纯文本变量的形式存储我们的秘密，但这不是一个好的做法。

# **存储纯文本机密不是好做法的原因？**

如果我们在应用程序中存储纯文本机密，任何能够访问我们的文件系统的人都可以访问受密码保护的资源，这也使得攻击者的工作变得非常容易。

因此，为了保护我们的应用程序并增强其安全性，我们需要使用一些加密方法，或者将我们的秘密存储在保险库或 secret manager 服务中的某个地方，比如在 [AWS](/javarevisited/5-best-aws-courses-for-beginners-and-experienced-developers-to-learn-in-2021-563212409fbd) 中。

# **我们如何加密这些内容？**

这里我们举一个简单的例子，假设您有一个 [spring boot 应用程序](/hackernoon/top-5-spring-boot-and-spring-cloud-books-for-java-developers-75df155dcedc?source=---------23------------------)，并且您已经将您的纯文本数据库凭证存储在 application.yml(yaml 文件)或 application.properties 文件中，如下所示:

> ***application . yml***

```
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/cdr?useSSL=false
    username: root
    password: root123
```

现在，要加密用户名和密码字符串，我们可以在这里使用 [**Jasypt(Java 简化加密)库**](http://www.jasypt.org/) 。

# **jas ypt 如何工作？**

Jasypt 提供了两种加密方式:单向加密(没有私钥)和双向加密(有私钥)

它使用默认的[加密算法](https://javarevisited.blogspot.com/2019/04/top-20-searching-and-sorting-algorithms-interview-questions.html):**pbewithmd 5 和 DES** ，但是它也提供了使用更强加密选项的选项，例如:**pbewithmd 5 和 TripleDES** 。

有一个[免费的 Jasypt 在线加密和解密工具](https://www.devglan.com/online-tools/jasypt-online-encryption-decryption) ，它提供了单向和双向(简单)加密和解密的选项。

# **如何在 Spring boot 应用中使用 Jasypt？**

如果您有`@SpringBootApplication`或`@EnableAutoConfiguration`自动配置注释，那么您可以简单地将下面的 starter 依赖项和插件添加到项目的 pom.xml 或 build.gradle 中，加密属性将在整个应用程序中启用:

> *美芬:*

```
<dependency>
        <groupId>com.github.ulisesbocchio</groupId>
        <artifactId>jasypt-spring-boot-starter</artifactId>
        <version>3.0.4</version>
</dependency><plugin>
   <groupId>com.github.ulisesbocchio</groupId>
   <artifactId>jasypt-maven-plugin</artifactId>
   <version>3.0.4</version>
</plugin>
```

> 格雷尔:

```
implementation group: ‘com.github.ulisesbocchio’, name: ‘jasypt-spring-boot-starter’, version: ‘3.0.4’implementation group: ‘com.github.ulisesbocchio’, name: ‘jasypt-maven-plugin’, version: ‘3.0.4’
```

如果您不使用`[@SpringBootApplication](https://javarevisited.blogspot.com/2018/05/the-springbootapplication-annotation-example-java-spring-boot.html#axzz6qnblZnVj)`或`[@EnableAutoConfiguration](https://www.java67.com/2018/05/difference-between-springbootapplication-vs-EnableAutoConfiguration-annotations-Spring-Boot.html)`自动配置注释，那么不添加上面的依赖项，而是将下面的依赖项添加到您的项目中:

> [*美芬*](/javarevisited/6-best-maven-courses-for-beginners-in-2020-23ea3cba89) *:*

```
<dependency>
        <groupId>com.github.ulisesbocchio</groupId>
        <artifactId>jasypt-spring-boot</artifactId>
        <version>3.0.4</version>
</dependency>
```

> [](/javarevisited/5-best-gradle-courses-and-books-to-learn-in-2021-93f49ce8ff8e)**:**

```
*implementation group: ‘com.github.ulisesbocchio’, name: ‘jasypt-spring-boot’, version: ‘3.0.4’*
```

*然后将`@EnableEncryptableProperties`添加到您的配置类中:*

```
*@Configuration
@EnableEncryptableProperties
public class MyApplication {
    ...
}*
```

*现在，一旦您添加了这些依赖项，只需更新您的项目，这样这些依赖项就可以在您的 [maven/gradle](https://javarevisited.blogspot.com/2020/06/maven-vs-gradle-beginners-introduction.html#axzz6dHZ7oEpK) 存储库中使用了。*

*要用 Jasypt 加密凭证，只需转到 application.yml 并添加以下属性:*

```
*jasypt:
  encryptor:
    algorithm: PBEWithMD5AndTripleDES
    iv-generator-classname: org.jasypt.iv.NoIvGenerator*
```

*除此之外，您还需要编写类似于**DEC(*secret _ string*)**的秘密字符串，以便 Jasypt 了解哪些字符串需要加密:*

```
*spring:
  datasource:
    url: jdbc:mysql://localhost:3306/cdr?useSSL=false
    username: DEC(root)
    password: DEC(root123)*
```

*然后进入终端，在 pom.xml 所在的项目根文件夹中运行下面的命令:*

****注意:*** 如果您有 application.yml，那么您需要在命令中添加文件路径参数，如下所示，因为 Jasypt 默认在类路径中查找 application.properties 文件*

```
*mvn jasypt:encrypt -Djasypt.plugin.path=”file:src/main/resources/application.yml” -Djasypt.encryptor.password=”secretkey”*
```

*如果您正在使用 application.properties，则可以使用以下命令:*

```
*mvn jasypt:encrypt -Djasypt.encryptor.password=”secretkey”*
```

***一旦你运行这个命令，进入你的 application.properties 或者 application.yml 文件，你会看到你的秘密已经被加密了… *瞧****

***现在，你可以看到这些字符串变成了这样:***

```
***spring:
  datasource:
    username: **ENC(zI0hID6ReXaLOMNaOPMWQg==)*****
```

***但是，如果您现在尝试运行这个应用程序，因为您的秘密已经被加密，您的应用程序将由于数据库连接错误而无法运行。***

## *****现在..想想我们如何告诉应用程序以解密的形式使用这些秘密？*****

***我们想到的一个解决方案是将私钥(用于加密我们的秘密)存储在加密秘密的某个地方，但这也不是一个好方法。***

***作为替代，我们可以将它存储在系统或环境变量中，或者我们需要以加密形式存储它。***

***更好的方法是在系统/环境变量中的某个地方配置私钥，为此我们再次需要在 application.yml 中存储一个变量，我们将把它配置为我们的[环境变量](https://javarevisited.blogspot.com/2012/08/how-to-get-environment-variables-in.html):***

```
***jasypt:
  encryptor:
    algorithm: PBEWithMD5AndTripleDES
    iv-generator-classname: org.jasypt.iv.NoIvGenerator
    password: ${JASYPT_ENCRYPTOR_PASSWORD}***
```

***要将这个 JASYPT_ENCRYPTOR_PASSWORD 存储为环境变量，请转到终端并运行命令***

```
***vi ~/.bash_profile***
```

***并在那里添加属性***

```
***export JASYPT_ENCRYPTOR_PASSWORD = secretkey***
```

***保存文件并退出，然后使用***

```
***source ~/.bash_profile***
```

***如果您使用的是 IDE，您需要重新启动它以从环境/系统变量中获取更新的值。***

***现在你都准备好了。如果您要运行您的应用程序，它将开始没有任何错误。***

*****注意:**这篇文章关注的是整个 spring boot 应用程序中的加密，但是如果你只需要为某些类定制它，你可以参考这里的[jaspyt Github 库](https://github.com/ulisesbocchio/jasypt-spring-boot)。***

***你可能喜欢的其他 Java 和 Spring Boot 文章***

***[](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e) [## 我最喜欢的 2021 年学习 Spring Boot 的课程——最好的

### 大家好，如果你有兴趣学习 Spring Boot 并寻找一些很棒的资源，如书籍，教程…

medium.com](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e) [](/javarevisited/10-best-online-courses-to-learn-spring-framework-in-2020-f7f73599c2fd) [## 2021 年学习 Spring 框架的 10 个最佳在线课程

### 学习 Spring 框架、Spring Boot、Spring 安全和 RESTful Web 服务的最佳在线课程

medium.com](/javarevisited/10-best-online-courses-to-learn-spring-framework-in-2020-f7f73599c2fd) [](/javarevisited/10-best-java-microservices-courses-with-spring-boot-and-spring-cloud-6d04556bdfed) [## 2021 年 Spring Boot 和 Spring Cloud 的 10 个最佳 Java 微服务课程

### 我最喜欢的 2021 年用 Spring Boot 和 Spring Cloud 为初学者学习 Java 微服务的在线课程来自…

medium.com](/javarevisited/10-best-java-microservices-courses-with-spring-boot-and-spring-cloud-6d04556bdfed)***