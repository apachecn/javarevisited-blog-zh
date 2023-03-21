# 如何在 Ubuntu 20.04 上安装多个版本的 Java 初学者指南

> 原文：<https://medium.com/javarevisited/install-multiple-versions-of-java-7-8-11-on-ubuntu-20-04-beginners-guide-1368a230eafd?source=collection_archive---------1----------------------->

[![](img/664e95da4daed3154f6e7d024adeb040.png)](https://www.java67.com/2018/08/top-10-free-java-courses-for-beginners-experienced-developers.html)

这里我将解释几个简单的步骤，在你最新的 [Ubuntu](https://javarevisited.blogspot.com/2022/03/top-5-free-courses-to-learn-linux-ubuntu.html) 桌面上安装多个版本的 Java。

在这个例子中，我将使用 Oracle JDKs。您可以从以下网站下载您最喜欢的甲骨文 JDK，

  

我个人使用的是 Java 7、8 和 11。在 java 11 中，可以直接从 Oracle 官网下载 Debian 包和，使用以下命令安装。

```
sudo dpkg -i path_to_deb_file
```

但是，对于旧版本的 java，你需要下载 gzip 文件并手动安装。

作为一个例子，我在这里写下 JDK 7 的命令。您可以对首选版本的 tar.gz 文件重复这些命令。

使用[焦油命令](https://javarevisited.blogspot.com/2011/11/tar-command-in-unix-linux-example.html)提取。在这里您可以看到几个提取选项，

*   x —提取归档文件
*   v —显示详细信息
*   z-用于提取使用 gzip 算法压缩的 zip 文件。
*   f-用给定的文件名创建归档文件

```
tar xvzf jdk-7u80-linux-x64.tar.gz
```

将目录的所有者更改为 root，默认情况下所有者应该是您。

```
sudo chown -R root:root jdk1.7.0_80
```

将它移到/usr/lib/jvm 中

```
sudo mv jdk1.7.0_80 /usr/lib/jvm
```

安装 java(配置您的计算机中有哪些可供选择的 Java 版本)

*   1065 是优先级，数字越大，优先级越高

```
sudo update-alternatives — install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_80/bin/java 1065sudo update-alternatives — install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_80/bin/javac 1065sudo update-alternatives — install /usr/bin/jar jar /usr/lib/jvm/jdk1.7.0_80/bin/jar 1065sudo update-alternatives — install /usr/bin/javaws javaws /usr/lib/jvm/jdk1.7.0_80/bin/javaws 1065
```

根据需要切换 java 的默认版本。(您安装的最高优先级版本应在安装时自动设置为默认值)

*   [java](https://javarevisited.blogspot.com/2012/04/difference-between-java-and-javaw.html) — Java 应用启动器
*   Java 编译器
*   [jar](https://javarevisited.blogspot.com/2012/03/how-to-create-and-execute-jar-file-in.html) — Java 归档工具
*   javaws —从浏览器启动 java 应用程序。从 Java 11 开始，不推荐使用 Java Web Start。

```
sudo update-alternatives — config javasudo update-alternatives — config javacsudo update-alternatives — config jarsudo update-alternatives — config javaws
```

与 [$JAVA_HOME](https://www.java67.com/2016/06/how-to-set-javahome-and-path-in-linux.html) 不同，alternatives 允许某些工具使用一个 JAVA 安装，而其他工具使用一个完全不同的安装。

但是[设置$JAVA_HOME](https://javarevisited.blogspot.com/2012/02/how-to-set-javahome-environment-in.html) 的需要完全取决于用例。

编码快乐！