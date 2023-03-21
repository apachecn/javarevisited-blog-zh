# 配置还是自动配置？

> 原文：<https://medium.com/javarevisited/configuration-or-autoconfiguration-ddb75a7b1548?source=collection_archive---------2----------------------->

了解 Spring boot 中配置和自动配置的区别！

![](img/73084fd2f3ab34d3d0a01ed0ad15d302.png)

照片由[格雷格·拉科齐](https://unsplash.com/@grakozy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/magic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

春靴是关于魔法的。您将依赖项添加到类路径中，所有的东西就可以组合在一起工作了。它为你提供了一张画布。开箱即用的配置是固执己见的，并且会做出默认的配置选择，同时，如果您想要做出自己的配置选择，这些观点和配置将是第一个让出您的方式的。

这种神奇很大程度上是因为[自动配置](https://docs.spring.io/spring-boot/docs/1.4.1.RELEASE/reference/html/using-boot-auto-configuration.html)。除了以下两个不同之处，自动配置类与常规配置类非常相似:

*   自动配置类最后运行(意味着在所有常规的非自动配置类之后)，而配置类的运行顺序是不确定的(除非我们使用像`@Ordered`这样的排序注释)
*   要声明一个类为`AutoConfiguration` ，它们需要在`spring.factories`文件中指定。一个例子可以在[这里](https://github.com/spring-projects/spring-boot/blob/v2.1.8.RELEASE/spring-boot-project/spring-boot-autoconfigure/src/main/resources/META-INF/spring.factories#L20)找到。

到目前为止，我相信您正在考虑什么时候应该选择自动配置类还是配置类。很棒的问题！就个人而言，我喜欢在以下情况下使用自动配置:

*   bean 的创建依赖于其他 bean 的存在。换句话说，如果你正在考虑使用`@ConditionalOnBean`或者`@ConditionalOnMissingBean`注释。
*   您的配置依赖于其他自动配置。对于自动配置类，您可以使用`@AutoConfigureAfter`或`@AutoConfigureBefore`注释来控制 beans 的创建顺序。

让我们看一些例子(并做一些日志记录来理解 beans 创建的顺序):

## 仅使用`@Configuration`的 Beans

让我们举一些非常非常简单的例子。考虑两个实用程序类:`StringUtils`和`ObjectUtils`:

```
class StringUtils { fun isBlank(value: String): Boolean { // Implementation here } // Other utility methods will go here}class ObjectUtils { fun isNull(value: Object?): Boolean { // Implementation here } // Other utility methods will go here}
```

接下来，我们将创建一个`StringUtilsConfiguration`类:

```
@Configuration
class StringUtilsConfiguration { @Bean
    fun stringUtils(): StringUtils {
        logger.info("Running StringUtilsConfiguration") return StringUtils()
    } companion object { private val logger = Logger.getLogger(this.*javaClass*.*name*)
    }}
```

还有一个`ObjectUtilsConfiguration`类:

```
@Configuration
class ObjectUtilsConfiguration { @Bean
    fun objectUtils(): ObjectUtils {
        logger.info("Running ObjectUtilsConfiguration") ObjectUtils()
    } companion object { private val logger = Logger.getLogger(this.*javaClass*.*name*)
    }}
```

如果我们要运行这个 [**spring boot 应用程序**](https://javarevisited.blogspot.com/2018/05/the-springbootapplication-annotation-example-java-spring-boot.html) ，您应该会看到以下日志语句:

```
Running ObjectUtilsConfiguration
Running StringUtilsConfiguration
```

为了控制执行的顺序，我们可以使用`@Order`注释。但是当只有配置类时，这很容易。如果我们有许多配置类，或者如果我们需要根据其他 bean 是否存在来有条件地创建或不创建 bean，那么排序就变得很困难。最后，如果我们想要提供默认值(例如:提供默认值)——输入自动配置。

## 自动配置简介

如前所述，自动配置类是使 Spring Boot 如此有效的一个重要组成部分。我们可以在 Spring boot 的源代码中找到几个自动配置类。一些例子包括:`FlywayAutoConfiguration`、GsonAutoConfiguration、MailSenderAutoConfiguration 等。

在很多这样的类中，你可能会看到某种形式的`@Conditionalxxx`注释。所以当这些条件满足时，这些类就会被执行。

让我们从创建自动配置开始，为了简单起见，让我们将`StringUtilsConfiguration`类改为自动配置。我们需要做的唯一一件事是通过在`src/main/resource/META-INF`目录下创建一个`spring.factories`文件来将`StringUtilsConfiguration`注册为自动配置:

```
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.autoconfiguration.sample.ObjectUtilsConfiguration
```

现在，如果我们运行这个应用程序，我们将会看到`ObjectUtilsConfiguration`类总是最后运行。

当一个[配置类](https://www.java67.com/2018/05/difference-between-springbootapplication-vs-EnableAutoConfiguration-annotations-Spring-Boot.html)或者甚至仅仅是一个 bean 的创建依赖于其他 bean 的存在或不存在时，这一点确实很突出。`@ConditionalOnBean`是要使用的注释，但是从注释的文档来看:

> `[@Conditional](https://docs.spring.io/spring-framework/docs/5.3.2/javadoc-api/org/springframework/context/annotation/Conditional.html?is-external=true)`仅当满足所有指定要求的 beans 已经包含在`[BeanFactory](https://javarevisited.blogspot.com/2012/11/difference-between-beanfactory-vs-applicationcontext-spring-framework.html#axzz6CgkxY2DJ)`中时，才匹配。要使条件匹配，必须满足所有的要求，但是不一定要由同一个 bean 来满足。

如上所述，`@ConditionalOnBean`只查看 BeanFactory 的当前状态，而不查看 bean 是否会出现在应用程序中。因此，如果创建某个 bean 的类的配置依赖于另一个配置，那么一个选择是让您的配置成为自动配置，以确保它在所有其他非自动配置类之后运行。如果您的配置依赖于另一个自动配置，使用`@AutoConfigureAfter`或`@AutoConfigureBefore`会有所帮助。

# 结论

创建我们自己的类似于 Spring boot 的神奇代码来让事情“自动地”工作是非常非常容易的。但是最终会有一个点，知道和理解这两个选项之间的区别变得极其重要，以使这种魔力发挥作用。就 [**Spring boot**](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e) 而言，一个很好理解的话题就是自动配置！我希望我的博客帖子至少触及了表面:)