# 什么是依赖注入，如何在 Java 和 Spring 中使用它

> 原文：<https://medium.com/javarevisited/what-is-dependency-injection-and-how-to-use-it-in-java-with-spring-516a605d671e?source=collection_archive---------1----------------------->

![](img/b8772d37d51183f1e17ea844fcab738b.png)

嗨，今天我将尝试用简单的方式描述两件事:依赖注入和控制反转。

[依赖注入](https://javarevisited.blogspot.com/2015/06/difference-between-dependency-injection.html)是一种设计模式，用于帮助改善代码耦合，使其更具可读性，同时也改善项目中类之间的责任分解。

这也是练习控制反转的一种方式。

但是什么是[控制反转](https://javarevisited.blogspot.com/2012/12/inversion-of-control-dependency-injection-design-pattern-spring-example-tutorial.html#axzz6u4HTHz4Z)？

基本上，这是一种在类内部操纵对象的方法。看看这个:

```
import br.com.clinicalosacco.app.adapters.controllers.request.SecretariaRequest;
import br.com.clinicalosacco.app.adapters.jpa.SecretariaJpa;
import br.com.clinicalosacco.app.domain.usecases.SecretariaService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/secretaries")
public class SecretaryController {

    private SecretaryService service = new SecretaryJpa();

    @GetMapping
    public ResponseEntity<?> list(){
        return ResponseEntity.*ok*(service.listar());
    }}
```

你是否注意到你必须实例化一个`SecretaryService`？

SecretaryController 类知道如何创建 SecretaryController 类的对象，如果这个[构造函数](https://javarevisited.blogspot.com/2012/12/what-is-constructor-in-java-example-chainning-overloading.html)被修改，SecretaryController 类将直接受到影响。那不公平，她只是想用一些服务来做她的运营。

为此，我们可以修改`SecretaryController` 类来注入 SecretaryService 类，并让应用程序决定何时创建它。
这在 [Spring](/javarevisited/10-best-online-courses-to-learn-spring-framework-in-2020-f7f73599c2fd) 中并不复杂，这里我们将使用构造函数依赖注入，如下所示:

```
import br.com.clinicalosacco.app.adapters.controllers.request.SecretariaRequest;
import br.com.clinicalosacco.app.adapters.jpa.SecretariaJpa;
import br.com.clinicalosacco.app.domain.usecases.SecretariaService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/secretaries")
public class SecretaryController {

    private SecretaryService service; public SecretaryController(SecretaryService service){
        this.service = service;
    } @GetMapping
    public ResponseEntity<?> list(){
        return ResponseEntity.*ok*(service.listar());
    }}
```

或者你可以使用一个 [lombok 注释](https://javarevisited.blogspot.com/2021/08/how-to-use-lombok-library-in-java.html):

```
import br.com.clinicalosacco.app.adapters.controllers.request.SecretariaRequest;
import br.com.clinicalosacco.app.adapters.jpa.SecretariaJpa;
import br.com.clinicalosacco.app.domain.usecases.SecretariaService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/secretaries")
@RequiredArgsConstructor
public class SecretaryController {

    private final SecretaryService service; @GetMapping
    public ResponseEntity<?> list(){
        return ResponseEntity.*ok*(service.listar());
    }}
```

恭喜，这是一个如何使用依赖注入应用控制反转的例子。