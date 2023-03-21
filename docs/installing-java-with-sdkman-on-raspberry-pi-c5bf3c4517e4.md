# 用 SDKMAN 在 Raspberry Pi 上安装 Java

> 原文：<https://medium.com/javarevisited/installing-java-with-sdkman-on-raspberry-pi-c5bf3c4517e4?source=collection_archive---------1----------------------->

如果你用操作系统为一个 Raspberry Pi 创建一个新的 SD 卡，你可以选择“Raspberry Pi OS Full (32 位)”版本，它包含 Java 11。但是许多其他可用的操作系统版本都没有包含 Java。

安装 Java 的方法有很多，但就我个人而言，我发现 [SDKMAN](https://sdkman.io/) 是最容易使用的一种。它可以让你轻松安装一个 JDK，还可以在不同版本之间切换。

> ***SDKMAN！*** *是一个在大多数基于 Unix 的系统上管理多个* ***软件开发工具包*** *并行版本的工具。它提供了一个方便的命令行界面(CLI)和 API，用于安装、切换、删除和列出候选项。*

foojay.io 上有一个关于 SDKMAN 的完整演示视频。

SDKMAN 可以根据操作系统和架构向您展示可用的 JDK 发行版列表。直到最近，这还不能在[树莓 Pi](/javarevisited/my-favorite-courses-to-learn-internet-of-things-iot-in-2020-best-of-lot-8517aa9fc838) 上运行，因为它运行在 ARM 上。

但由于这是一个开源项目，因此可以请求新的功能并帮助实现解决方案。正如你在 GitHub 第 836 期中看到的，这需要在项目的不同部分进行一些返工。但是我们现在有一个工作版本了！！！

# 尝试一下

如果你想直接潜入，给自己拿个树莓 Pi，打开一个终端，安装 ZIP 和 SDKMAN。

```
$ sudo apt install zip
$ curl -s “https://beta.sdkman.io" | bash# Open a new terminal or run this command
$ source “$HOME/.sdkman/bin/sdkman-init.sh”
$ sdk list java===================================================================
Available Java Versions for Linux ARM 32bit Hard Float
===================================================================
Vendor   | Use | Version | Dist   | Status | Identifier
Liberica |     | 17.0.1  | librca |        | 17.0.1-librca
         |     | 11.0.13 | librca |        | 11.0.13-librca
Zulu     |     | 11.0.13 | zulu   |        | 11.0.13-zulu
         |     | 8.0.312 | zulu   |        | 8.0.312-zulu
===================================================================
```

是的，没错，甚至对于 Raspberry Pi 也可以使用不同的发行版！所以让我们试试 Azul Zulu 11。

```
$ sdk install java 11.0.13-zulu
$ java -version
openjdk version “11.0.13” 2021–10–19 LTS
OpenJDK Runtime Environment Zulu11.52+13-CA (build 11.0.13+8-LTS)
OpenJDK Client VM Zulu11.52+13-CA (build 11.0.13+8-LTS, mixed mode)
```

确认！安装了 SDKMAN 的 Azul Zulu 11 在树莓 Pi 上运行良好。

边注:虽然 Liberica 和 Zulu 都被列为“Linux ARM 32bit 硬浮点”，但是**只有 Zulu 在 ARMv6 上工作，比如树莓派 Zero 1** 。目前，BellSoft 没有计划支持这种处理器。

# 关于处理器架构

由于自 2013 年以来有多个 Raspberry Pi 版本(维基百科上的[完整历史](https://en.wikipedia.org/wiki/Raspberry_Pi#Specifications))，因此使用了不同的 ARM 处理器类型。ARMv6 仅支持 32 位，而 ARMv7 是 64 位处理器。

可以使用`uname`命令检查架构，以下是一些输出:

板操作系统`uname -m`树莓 Pi Zero 132-bitar mv 6 树莓 Pi 432-bitar mv 7 lrasp berry Pi 464-bitaarch 64

如果您想更多地了解您的系统，请使用`lscpu`命令，这是 32 位操作系统的 Raspberry Pi 4 的输出:

```
$ lscpu
Architecture: armv7l
Byte Order: Little Endian
CPU(s): 4
On-line CPU(s) list: 0–3
Thread(s) per core: 1
Core(s) per socket: 4
Socket(s): 1
Vendor ID: ARM
Model: 3
Model name: Cortex-A72
Stepping: r0p3
CPU max MHz: 1500.0000
CPU min MHz: 600.0000
BogoMIPS: 108.00
Flags: half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32
```

# 64 位操作系统

经过长时间的测试版测试，2 月 2 日，Raspberry Pi 正式发布了他们操作系统的 64 位版本。你可以[在 Raspberry Pi 博客](https://www.raspberrypi.com/news/raspberry-pi-os-64-bit/)上了解更多关于这个版本的信息。而且因为这是比较常用的架构，所以 SDKMAN 的输出真的很可观…

```
===================================================================
Available Java Versions for Linux ARM 64bit
===================================================================Vendor         | Use | Version      | Dist   | Identifier
AdoptOpenJDK   |     | 8.0.275+1.hs | adpt   | 8.0.275+1.hs-adpt
               |     | 8.0.252.hs   | adpt   | 8.0.252.hs-adpt
Corretto       |     | 17.0.2.8.1   | amzn   | 17.0.2.8.1-amzn
               |     | 17.0.0.35.1  | amzn   | 17.0.0.35.1-amzn… 45 EXTRA LINES HERE !!!Temurin        |     | 17.0.2       | tem    | 17.0.2-tem
               |     | 11.0.14      | tem    | 11.0.14-tem
               |     | 8.0.322      | tem    | 8.0.322-tem
Zulu           |     | 17.0.2       | zulu   | 17.0.2-zulu
               |     | 11.0.14      | zulu   | 11.0.14-zulu
               |     | 8.0.322      | zulu   | 8.0.322-zulu
===================================================================
```

所以使用 Temurin 17 版本就像`sdk install java 17.0.2-tem`一样简单:

```
$ sdk install java 17.0.2-tem
Downloading: java 17.0.2-tem
In progress…
Repackaging Java 17.0.2-tem…
Done repackaging…
Installing: java 17.0.2-tem
Done installing!
Setting java 17.0.2-tem as default.$ java -version
openjdk version “17.0.2” 2022–01–18
OpenJDK Runtime Environment Temurin-17.0.2+8 (build 17.0.2+8)
OpenJDK 64-Bit Server VM Temurin-17.0.2+8 (build 17.0.2+8, mixed mode, sharing)
```

# 结论

[Raspberry Pi 上的 Java](/javarevisited/10-best-places-to-learn-java-online-for-free-ce5e713ab5b2) 一直都是可行的，但是 SDKMAN 让入门过程变得容易多了。

安装第一台 JDK 或者更换版本现在已经变得易如反掌了！