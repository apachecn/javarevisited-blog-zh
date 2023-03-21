# 高性能日志记录— Java

> 原文：<https://medium.com/javarevisited/high-performance-logging-java-59ba374b2166?source=collection_archive---------0----------------------->

![](img/55668a340609f8abf51bc0d74aa7e5db.png)

当架构和设计开发时，日志记录是软件中的一个基本原则。因为日志记录会直接影响应用程序的性能。如果我们不在应用程序中进行有意义和可理解的日志记录，生产应用程序中的故障排除将是一场噩梦。我们需要知道什么需要记录，什么不应该记录。否则，我们将面临许多问题，如安全问题、容量问题等。

## SLF4J(Java 的简单日志门面)

SLF4J 为 Log4j、Log4j2、Logback 等日志框架提供了日志 API。这有助于减少各种日志框架和应用程序之间的紧密耦合。这也将提高代码的可维护性。最好的方法是[在代码](https://javarevisited.blogspot.com/2013/08/why-use-sl4j-over-log4j-for-logging-in.html#axzz6JyHI5fGb)中使用 SLF4J，并在运行时使用日志框架。

软件行业中最流行的日志框架是 Log4j 和 Log4j2。

## 为什么 Log4j2 比 Log4j 好？

*   主要是 Log4j2 支持异步记录器。
*   对 Log4j2 配置文件进行配置修改时，无需重启服务器。
*   无垃圾运行时。
*   开发人员可以定义自定义日志级别。
*   并发性提高，没有任何死锁。
*   从 java 8 开始，lambda 函数支持日志记录。

## 带有 Log4j2 的历史记录记录器

Chronicle logger 是一个为 Java 语言开发的速度极快的记录器，使用 Chronicle Queue 作为持久性引擎。它支持 Log4j2 日志框架的使用。使用这个历史日志记录器的实现，我们可以最小化应用程序的日志开销，并提高应用程序的性能。在这种方法中，日志记录部分从操作系统层开始在 [JVM 应用程序](/javarevisited/7-best-courses-to-learn-jvm-garbage-collection-and-performance-tuning-for-experienced-java-331705180686?source=---------8------------------)中负责。

*使用这种依赖关系—*

```
<**dependency**>
    <**groupId**>net.openhft</**groupId**>
    <**artifactId**>chronicle-logger-log4j-2</**artifactId**>
    <**version**>4.21ea40</**version**>
</**dependency**>
```

*日志记录配置文件示例—*

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration packages="net.openhft.chronicle.logger,net.openhft.chronicle.logger.log4j2">
    <appenders>
        <Chronicle name="CHRONICLE">
            <path>/var/log/</path>
            <chronicleCfg>
                <blockSize>128</blockSize>
                <bufferCapacity>256</bufferCapacity>
     <rollCycle>HOURLY</rollCycle>
            </chronicleCfg>
        </Chronicle>
    </appenders>
    <loggers>
        <logger name="chronicle" level="info" additivity="false">
            <appender-ref ref="CHRONICLE"/>
        </logger>
    </loggers>
</configuration>
```

## Log4j2 内存映射文件附加器

这是高性能日志记录的另一种方式，它将日志记录的责任交给了操作系统。使用内存映射文件的主要好处是应用程序性能。在执行日志记录时，JVM 没有 I/O 开销。因为 appender 不是将日志记录到磁盘内存映射文件，而是将日志事件写入本地内存。

*样本日志记录配置文件—*

```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="info" name="SampleApplication" packages="com.sidath.app">
  <Appenders>
    <MemoryMappedFile name="AppLog" fileName="/var/log/application.log">
      <PatternLayout>
        <Pattern>%d %p %c{1.} [%t] %m%n</Pattern>
      </PatternLayout>
    </MemoryMappedFile>
  </Appenders>
  <Loggers>
    <Root level="Info">
      <AppenderRef ref="AppLog"/>
    </Root>
  </Loggers>
</Configuration>
```

## 夏天似的

日志是软件开发的一个重要部分。这会直接影响应用程序的性能，因为日志记录是一项高度 I/O 密集型的操作。如果您真的关心应用程序的性能，那么最好使用上面提到的方法。

***参考文献***

OpenHFT/Chronicle-Logger:一个亚微秒 java logger，支持标准日志 API，如 Slf【github.com】Log4J(T4)

[Log4j 2 附加器—阿帕奇 Log4j 2](https://logging.apache.org/log4j/log4j-2.1/manual/appenders.html)