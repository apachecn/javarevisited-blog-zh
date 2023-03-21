# 我如何处理 Veracode 问题(CWE 117)不正确的日志输出中和| Java | Veracode 修复

> 原文：<https://medium.com/javarevisited/how-i-handle-veracode-issue-cwe-117-improper-output-neutralization-for-logs-java-veracode-2c55df32245a?source=collection_archive---------2----------------------->

![](img/a562458c3f3abab732bdb23ca2794a26.png)

> Veracode scanner 能够发现日志伪造攻击。我建议您在投入生产之前用 Veracode 扫描仪扫描您的应用程序，以确保您的应用程序安全。

## 为什么这是一个问题

将不受信任的数据写入日志文件，使得攻击者能够伪造日志
条目或将恶意内容注入日志文件。损坏的日志文件可用于掩盖攻击者的踪迹，或作为攻击日志查看或处理实用程序的传递机制。

## Veracode 解决方案

Veracode 建议尽可能避免在日志文件中直接嵌入用户输入，或者对用于构建日志条目的不可信数据进行消毒。

这可以通过使用 ESAPI 库中的 encodeForHtml()编码方法来实现。但是，这个库需要一些额外的配置(ESAPI.properties ),如果您只是想对记录的值进行转义，这是多余的。

## 我如何处理这个问题

**我拥有的日志样本**

```
log.info("test user :{}", user);
log.info("test user :{} test name : {}", user, name);
log.info("test user: {} "test name : {} test mail : {} ", user, name, mail);
log.error("Exception occurred for test user :{} test name : {}", user, name, ex); 
```

**我写的解决这个问题的代码**

写完这些[静态方法](https://www.java67.com/2014/10/difference-between-static-and-non-static-method-java-programming.html)后，我们可以直接在 logger 方法中使用它们，如下所示。

```
import utils.VeracodeUtil.*secureArguments;* public class Test{
   public void testMethod(){
     log.info("test user :{}", secureArguments(user));
     log.info("test user :{} test name : {}", secureArguments(user,    name));
     log.info("test user: {} "test name : {} test mail : {} ", secureArguments(user, name, mail));
     log.error("Exception occurred for test user :{} test name : {}", secureArguments(user, name, ex));
  }
}
```

## 参考

  <https://stackoverflow.com/questions/46564555/pass-veracode-cwe-117-improper-output-neutralization-for-logs-only-with-replac>  

[https://stack overflow . com/questions/16225935/security-flaw-vera code-report-crlf-injection/49804999 # 49804999](https://stackoverflow.com/questions/16225935/security-flaw-veracode-report-crlf-injection/49804999#49804999)