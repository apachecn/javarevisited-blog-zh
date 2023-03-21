# 如何使用脸书和 Spring Boot 实现 OAuth2 社交登录(单点登录)——第 2 部分

> 原文：<https://medium.com/javarevisited/how-to-implement-oauth2-social-login-using-facebook-spring-boot-part-2-8582a8943144?source=collection_archive---------1----------------------->

## 脸书

你好，我是**罗汉·卡达姆**

大家好，希望你们一切都好。今天我们将了解如何使用脸书和 [Spring Boot](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e?source=---------39------------------) 实现 OAuth2 社交登录。让我们开始编码吧。

![](img/08afc365a6db5e50fd46b49c5f1a15f2.png)

**使用脸书和 Spring Boot 的社交登录**

在实现第 2 部分之前。我请求进入第 1 部分— **看看如何使用脸书实现 OAuth2 社交登录**

</@rohankadam965/how-to-implement-oauth2-social-login-using-facebook-part-1-e7995f30774e>  

## **步骤 1:** 使用 Spring 初始化器创建一个 Spring Boot 项目。

<https://start.spring.io/>  

## **步骤 2:** 在您的项目 pom.xml 中添加以下依赖项

```
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-oauth2-client</artifactId>
</dependency>
```

## **第三步:**创建实现 [OAuth2 的配置文件。](/javarevisited/top-10-courses-to-learn-spring-security-and-oauth2-with-spring-boot-for-java-developers-8f0222d6066d?source=---------5-----------------------)

**oauth 2 log in 的安全配置**

## **步骤 4:-** 创建应用程序 Yml，它将包含脸书配置。

**应用 Yml 进行脸书配置。**

## **第五步:-** 创建一个由两个端点组成的[静止控制器](https://javarevisited.blogspot.com/2017/08/difference-between-restcontroller-and-controller-annotations-spring-mvc-rest.html#axzz6grO2U4Lp)。

**带端点的脸书控制器。**

## **注:-**

1.  主体对象包含用户名、电子邮件和配置文件图像，具体取决于范围。
2.  如果出现关于不正确的**重定向 Url** 的**错误**，则添加以下 Url[**https://localhost:8080/oauth 2/callback/Facebook**](https://localhost:8080/oauth2/callback/google)

**本地主机的测试端点:-**

## [**http://localhost:8080/oauth 2/authorize/Facebook？redirect _ uri = http://localhost:8080/oauth 2/callback/Facebook**](http://localhost:8080/oauth2/authorize/google?redirect_uri=http://localhost:8080/oauth2/redirect)

![](img/0d8ba0bb6c76c8909aa5000db0cedfc9.png)

谢谢观众们