# Spring Boot 和微服务的发展

> 原文：<https://medium.com/javarevisited/evolution-of-spring-boot-and-microservices-4d1109b5a4d3?source=collection_archive---------2----------------------->

![](img/46d872f30aa9cc762f227988b4103cfa.png)

如果你在 5 年或 10 年前开发 web 应用程序，你会欣赏 web 应用程序和微服务领域的进步。微服务架构模式已经颠覆了我们为云创建应用的方式。随着[*【Spring Boot】*](https://spring.io/projects/spring-boot)*的推出，微服务的采用在技术领域的每个领域都出现了飙升。这篇文章给出了一个要点，从我的角度来看，为什么现在技术是正确的选择。*

# 历史

大约十年前，当我开始为一家公司编写代码时，大多数应用程序开发框架都没有创建和部署应用程序的标准方法。托管 web 应用程序的唯一标准方式是将“WAR”(Web 应用程序存档)文件部署到 Tomcat 服务器上，这是一个 servlet 容器。由于开发和生产 tomcat 服务器之间的不正确设置，我们曾经因为配置被生产实例搞得一团糟而彻夜难眠。

# 新时期

[Spring Boot 1.0](https://spring.io/blog/2014/04/01/spring-boot-1-0-ga-released) 由 Pivotal 于 2014 年 4 月 1 日正式发布。这个固执己见的框架是为了解决 Spring Framework 上的 github 问题(该团队花了近 18 个月的时间发布了 Spring 1.0 GA 版本)而创建的— [改进对“无容器”web 应用程序架构的支持](https://github.com/spring-projects/spring-framework/issues/14521)，2012 年 10 月。

微服务已经在这个行业存在了十多年，像网飞这样的公司从 2010 年就开始采用微服务。甚至连 [12factor 应用](http://www.12factorapp.net)的创造者 Adam Wiggins 也在 2011 年开始起草原则([参考文献](https://github.com/heroku/12factor/commit/2b06e7deabb64bb759f9fc6f4d9b6fcc546921bb))——这些是人们在创建微服务时需要遵循的一些基本原则。

由 Pivotal、网飞等公司开发的健康开源生态系统。，为开发人员创造了适应这些技术实践和模式的简单方法。老实说，我是在 2016 年才开始了解 [Spring Boot](http://www.java67.com/2018/06/top-15-spring-boot-interview-questions-answers-java-jee-programmers.html) 的，从那以后，我受到了启发，感到惊讶，焕发了青春；我甚至开始通过 [TechPrimers](https://www.youtube.com/TechPrimers) 学习、编码和分享同样的东西。

# 怎么会？

我仍然记得我在 Spring Boot 的第一次会议，由 [Josh Long](https://medium.com/u/a17df5ec14a4?source=post_page-----4d1109b5a4d3--------------------------------) 主持，他是 Pivotal 的 Spring 开发者拥护者，也是 Spring 框架的贡献者(这是 bugs——他是这样称呼自己的)。他的名言，

> 制造罐子，而不是战争

启发我开始探索 spring boot 创建微服务的方式，从那以后我从未停止探索 spring 生态系统。还有其他开发者拥护者，比如马克·赫克勒、马特·斯汀等等。以及菲尔·韦伯——Spring Boot 的创造者，他将这个框架传播到了科技行业的每一个角落。

除此之外，像 medium.com 这样的平台帮助开发人员社区传播关于微服务架构和用例的有用解释、提示和文档，以推动采用。

# 为什么选择 Spring Boot 和微服务？

随着越来越多的人需要尽可能快地创建应用程序，像 Spring Boot 这样一个固执己见的框架将所有类型的依赖带给你是一个巨大的胜利。更令人兴奋的是 Spring Cloud，它提供了丰富的库来支持网飞开源的微服务架构模式，作为[网飞 OSS](https://netflix.github.io/) 的一部分，来创建跨微服务的弹性、可扩展和有效的通信。据我所知，主要的卖点是开发人员使用像 [Spring Initializer](https://start.spring.io/) 、 [Spring Cloud](https://spring.io/projects/spring-cloud) 和 Spring 自己的基于注释的处理等工具创建微服务的效率。

更不用说网飞通过开源其大部分内部框架所做的贡献，这些框架被用来通过 [Hystrix](https://github.com/Netflix/Hystrix/wiki) 、 [Eureka](https://github.com/Netflix/eureka/wiki) 等模式使微服务生态系统变得更好。这个团队完成的最独特的开创性工程是他们的[混沌工程](https://principlesofchaos.org/)部分[四面军](/netflix-techblog/the-netflix-simian-army-16e57fbab116)。

# 将来的

我很期待看到 Spring Boot 如何反击新框架的崛起，比如来自 Grails 框架开发者的 [Micronaut](https://micronaut.io/) (使用提前编译加快应用程序启动)，Grails 框架从多年来使用 Spring、Spring Boot 和 Grails 构建从单片到微服务的真实世界应用程序的经验中获得灵感。

**进一步学习**
[掌握微服务用 Spring Boot 和 Spring Cloud](https://click.linksynergy.com/fs-bin/click?id=JVFxdTr9V80&subid=0&offerid=323058.1&type=10&tmpid=14538&RD_PARM1=https%3A%2F%2Fwww.udemy.com%2Fmicroservices-with-spring-boot-and-spring-cloud%2F)
[掌握 Hibernate 和 JPA 用 Spring Boot 用 100 步](https://click.linksynergy.com/fs-bin/click?id=JVFxdTr9V80&subid=0&offerid=323058.1&type=10&tmpid=14538&RD_PARM1=https%3A%2F%2Fwww.udemy.com%2Fmicroservices-with-spring-boot-and-spring-cloud%2F)
[Spring Boot 参考指南](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)
[Spring 框架大师班—初学者到专家](https://click.linksynergy.com/fs-bin/click?id=JVFxdTr9V80&subid=0&offerid=323058.1&type=10&tmpid=14538&RD_PARM1=https%3A%2F%2Fwww.udemy.com%2Fspring-tutorial-for-beginners%2F)
[REST 用 Spring 大师班 by Eugen Parsaschiv](http://courses.baeldung.com/p/rest-with-spring-the-master-class?affcode=22136_bkwjs9xa)
[Spring Boot:高效开发、配置和部署](https://pluralsight.pxf.io/c/1193463/424552/7490?u=https%3A%2F%2Fwww.pluralsight.com%2Fcourses%2Fspring-boot-efficient-development-configuration-deployment)
[5 Spring Boot 每个 Java 开发者都应该具备的特性](https://javarevisited.blogspot.com/2018/11/top-5-spring-boot-features-java.html#axzz5YFjHrt5j)

从现在开始，看看未来会如何发展是很有趣的。Spring Boot 还会制造罐子并选择退出战争吗？(你知道我的意思)——我们将拭目以待。