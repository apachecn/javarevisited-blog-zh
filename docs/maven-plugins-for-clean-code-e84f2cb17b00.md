# 用于干净代码的 Maven 插件

> 原文：<https://medium.com/javarevisited/maven-plugins-for-clean-code-e84f2cb17b00?source=collection_archive---------1----------------------->

# 代码审查:

代码评审是当今几乎每个软件开发公司都在使用的实践。它允许您在 CI / CD 周期的早期阶段检测到错误，并降低以后发生错误的风险。这也是程序员之间交流知识的一个很好的机会——既有关于好的编码实践的知识，也有给定产品的领域知识。

因为代码审查很重要，但是对于审查者来说，发现格式变化或者发现没有被使用的变量是非常困难的。人眼很难捕捉到这些东西，所以为什么不借助一些工具来实现自动化呢？

# **有用的 Maven 插件**

# 1.impsort-maven-plugin

这个插件用于自动排序/验证 Java 源文件中所有导入语句的顺序。除了 sort 之外，它还提供了许多其他有用的功能，这些功能有助于保持代码库的一致性，比如导入分组和删除不使用的导入。

```
<plugin>
    <groupId>net.revelc.code</groupId>
    <artifactId>impsort-maven-plugin</artifactId>
    <version>1.2.0</version>
    <configuration>
        <removeUnused>true</removeUnused>
        <staticGroups>*</staticGroups>
        <groups>java.,javax.,org.,com.</groups>
    </configuration>
    <executions>
        <execution>
            <goals>
                <goal>sort</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

在 [CI/CD](/javarevisited/7-best-courses-to-learn-jenkins-and-ci-cd-for-devops-engineers-and-software-developers-df2de8fe38f3) 中集成插件

```
<profile>
  <id>validate-format</id>
<activation>
    <property>
        <name>{pass property specific to CI/CD}</name>
    </property>
</activation>
  <build>
    <plugins>
      <plugin>
        <groupId>net.revelc.code</groupId>
        <artifactId>impsort-maven-plugin</artifactId>
        <executions>
          <execution>
            <goals>
              <goal>check</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
</profile>
```

所以当这个插件在 [IDE](/javarevisited/7-best-courses-to-learn-intellij-idea-for-beginners-and-experienced-java-programmers-2e9aa9bb0c05?source=---------16------------------) 中使用时，目标是排序，但在 CI/CD 中使用时，目标是检查，因为我们不想在 CI/CD 中排序，而是希望验证它，并在验证失败的情况下使构建失败。

更多信息:[https://code.revelc.net/impsort-maven-plugin/](https://code.revelc.net/impsort-maven-plugin/)

# 2.**格式化程序-maven-插件**

这个插件用于自动格式化/验证一个 maven 项目(包括源代码)。遵循某些规范保持编码风格格式的一致性是防止不必要的冲突和代码差异的一个重要要求。

因此，团队通常保持一种共同的风格，但需要在所有团队成员之间共享。为了避免在 IDE 中手工共享和导入样式表，推荐使用这个[插件](https://javarevisited.blogspot.com/2016/08/top-10-maven-plugins-every-java-developer-know.html#axzz5YVjCcR8u)。

我们公司定义了 java 编码标准，这就是为什么把它添加为依赖项的原因，如果你的公司有添加这些标准的话。

```
<plugin>
    <groupId>net.revelc.code.formatter</groupId>
    <artifactId>formatter-maven-plugin</artifactId>
    <version>2.0.1</version>
    <executions>
      <execution>
        <goals>
          <goal>format</goal>
        </goals>
      </execution>
    </executions>
    <dependencies>
      <dependency>
        <groupId>com.test.standards</groupId>
        <artifactId>java-coding-standards</artifactId>
      </dependency>
    </dependencies>
    <configuration>
      <configFile>eclipse-neon/eclipse-formatter.xml</configFile>
      <lineEnding>KEEP</lineEnding>
      <compilerSource>${java.version}</compilerSource>
      <compilerCompliance>${java.version}</compilerCompliance>
      <compilerTargetPlatform>${java.version}</compilerTargetPlatform>
    </configuration>
  </plugin>
</plugins>
```

将此与 CI/CD 集成

```
<plugin>
  <groupId>net.revelc.code.formatter</groupId>
  <artifactId>formatter-maven-plugin</artifactId>
  <version>2.0.1</version>
  <executions>
    <execution>
      <goals>
        <goal>validate</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

所以当这个插件将在 IDE 中使用时，目标是格式，但当在 CI/CD 中使用时，目标是验证，因为我们不想在 CI/CD 中排序，而是希望验证它。

更多信息:[https://code.revelc.net/formatter-maven-plugin/](https://code.revelc.net/formatter-maven-plugin/)

# 3. **maven pmd 插件**

PMD 是一个源代码分析器，用来发现常见的编程缺陷，比如未使用的变量、[空捕捉块](https://javarevisited.blogspot.com/2013/03/0-exception-handling-best-practices-in-Java-Programming.html) s、不必要的对象创建等等。

```
<plugin>
  <artifactId>maven-pmd-plugin</artifactId>
  <configuration>
    <excludeRoots>
      <excludeRoot>target/generated-sources</excludeRoot>
      <excludeRoot>target/generated-test-sources</excludeRoot>
    </excludeRoots>
    <linkXRef>true</linkXRef>
  </configuration>
</plugin>
```

更多信息:【https://maven.apache.org/plugins/maven-pmd-plugin/ 

# 4.**斑点 Bugs Maven 插件**

SpotBugs 在 Java 程序中寻找 bug。它基于 bug 模式的概念。错误模式是一个经常出错的代码习语。错误模式的出现有多种原因:

*   难懂的语言特征
*   被误解的 API 方法
*   维护期间修改代码时被误解的不变量
*   花园品种错误:打字错误，使用错误的布尔运算符

SpotBugs 使用[静态分析](http://javarevisited.blogspot.sg/2014/02/why-static-code-analysis-is-important.html#axzz4pDTuzL00)来检查 Java 字节码中 bug 模式的出现。

```
<plugin>
  <groupId>com.github.spotbugs</groupId>
  <artifactId>spotbugs-maven-plugin</artifactId>
  <version>4.2.0</version>
  <executions>
    <execution>
      <id>check</id>
      <phase>verify</phase>
      <goals>
        <goal>check</goal>
      </goals>
    </execution>
  </executions>
</plugin>
<plugin>
```

早期的 findBugs 是一个插件，但是现在被弃用了。这个插件对于鼓励开发者使用最佳实践非常有用。

更多信息:[https://spotbugs.github.io/spotbugs-maven-plugin/index.html](https://spotbugs.github.io/spotbugs-maven-plugin/index.html)

# **开发商的行动**

在提交 PR 之前，请在终端中运行 [**mvn 全新安装**](https://javarevisited.blogspot.com/2016/10/difference-between-mvn-install-release-and-deploy-in-Maven.html) ，以便执行所有插件，并且您的 CI/CD 作业不会因为这些问题而失败。

请评论我在这里遗漏的插件/工具。