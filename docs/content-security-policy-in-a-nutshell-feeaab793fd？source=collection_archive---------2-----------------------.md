# 简而言之，内容安全政策

> 原文：<https://medium.com/javarevisited/content-security-policy-in-a-nutshell-feeaab793fd?source=collection_archive---------2----------------------->

[![](img/18a72a4457a3198b8bb3de9e89c99446.png)](https://medium.com/javarevisited/top-10-courses-to-learn-spring-security-and-oauth2-with-spring-boot-for-java-developers-8f0222d6066d)

[卡斯帕·卡米尔·鲁宾](https://unsplash.com/@casparrubin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上 [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 内容安全策略(CSP)是一个安全层，用于缓解诸如跨站点脚本(XSS)和数据注入等攻击

网页可以以不同的方式与不同的网页内容进行交互。很少有人加载和执行内联和外部 JS & CSS 文件，进行 AJAX 调用，将图像/字体等资源加载到 web 应用程序中。以这种方式，如此多的内容与网络应用相关联。

由于上述内容与网页的交互，可能存在诸如跨站脚本(XSS)和代码注入等攻击。为了防止这些攻击，添加了策略安全层。

CSP 可以通过两种方法实现

1.  使用 HTTP 响应头(`**Content-Security-Policy**`)
2.  在 html 文件中使用 meta 元素( )

当使用上述方法之一将 CSP 规则添加到特定网页时，web 浏览器会检查这些策略并据此采取行动。

下面是一些常见的 CSP 示例

1.  所有内容将来自该网站自己的起源

```
Content-Security-Policy: default-src 'self'
```

2.所有内容将来自该网站自己的起源和 example.com 的子域

```
Content-Security-Policy: default-src 'self' *.example.com
```

3.所有内容将来自 example.com 通过 TLS 安全渠道只

```
Content-Security-Policy: default-src https://example.com
```

4.所有内容将来自 example.com，图像可以来自任何地方

```
Content-Security-Policy: default-src [https://example.com;](https://example.com;) img-src *
```

5.允许内联脚本执行(写在

```
Content-Security-Policy: script-src 'unsafe-inline'This method allows all the inline scripts, but hash value of a script can be given to enforce more security.Content-Security-Policy: script-src '<HashAlgorithm>-<Base64 encoded hash of the script>'
```

测试样本策略

[样本 HTML 代码](https://gist.github.com/tnishada/a4fa36d381be945054ec8cee8b999c7d)

在此 [HTML](/javarevisited/top-10-free-courses-to-learn-html-5-css-3-and-web-development-872d62d97a97) 代码中，CSP 被设置为**default-src‘self’，**因此浏览器不会加载任何外部资源或执行任何内联脚本。

在浏览器中打开上面的 HTML 代码。然后，您将在浏览器控制台中看到两条错误消息，说明它无法加载外部 jquery 文件，也无法执行内联脚本。

之后，请用下面的标签替换 meta 标签，并重新加载浏览器。然后你会看到所有的东西都被正确地加载/执行了。

```
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src * 'unsafe-inline' ;">
```

通过这种方式，您可以测试这个内容安全策略。

参考资料:

[https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)

[https://developer . Mozilla . org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy)