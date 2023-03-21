# 创建您自己的 maven 打包

> 原文：<https://medium.com/javarevisited/create-your-own-maven-packaging-2d69ad832720?source=collection_archive---------1----------------------->

*使用 maven，很容易制作 jar 文件、war 文件、ear 文件……等等。Maven 支持非常多的内置包。此外，它可以在 maven 中定义用户定义的打包。让我们看看这篇文章中关于 maven 打包的所有内容。*

![](img/b56ce786f2ee970533cfbe0a8b23a652.png)

由[杰西·拉米雷斯](https://unsplash.com/@jesseramirezla?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Maven 支持构建打包，如`jar, ear, war` …等。在 maven 中，构建什么由打包属性决定。它定义在`<packaging></packaging>`标签之间…