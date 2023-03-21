# JWT 的斯普林波特安全公司

> 原文：<https://medium.com/javarevisited/springboot-security-with-jwt-fca1446790ba?source=collection_archive---------0----------------------->

![](img/2e48f611faa1bf27db55de54e7238a8b.png)

在本文中，我将展示如何使用 JWT 令牌——JSON Web 令牌来实现项目 spring 安全性。

**如何使用 JWT:** 用户名+密码+ JSON map+ Base64 + key +有效期

**工作原理:**当用户第一次使用用户名+密码登录时，系统将交换回访问令牌，这个令牌代表一个 JSON 映射，包含所有用户信息，如用户配置文件、角色和权限，用 Base64 编码，用私钥签名。收到令牌后，用户可以一直使用它，直到它自己过期。

**好处:**JWT 令牌是无状态的。这意味着服务器可以将令牌解密为用户信息状态，并且不需要使用该令牌从数据库中进行额外的查找。这对于减少服务器上的负载是一个巨大的好处。这种方法在全世界被广泛使用。

# 技术

你应该具备基础知识，熟悉 java8 和 spring-boot。

```
*# Technologies* **-** Spring boot 2 
- Spring Secruity
**-** JWT jjwt
**-** Mysql
**-** Database JPA
```

# 项目结构

```
springboot2-jwt
|-- README.md
|-- pom.xml
|-- src
|   `-- main
|       |-- java
|       |   `-- com
|       |       `-- sma
|       |           `-- **security**
|       |               |-- Application.java
|       |               |-- **config**
|       |               |   |-- JwtAuthenticationEntryPoint.java
|       |               |   |-- JwtRequestFilter.java
|       |               |   |-- JwtTokenService.java
|       |               |   `-- WebSecurityConfig.java
|       |               |-- **controller**
|       |               |   |-- JwtTokenController.java
|       |               |   `-- TestController.java
|       |               |-- **dao**
|       |               |   `-- UserRepository.java
|       |               |-- **exception**
|       |               |   `-- GlobalExceptionHandler.java
|       |               |-- **json**
|       |               |   |-- JwtResponse.java
|       |               |   `-- UserDTO.java
|       |               |-- **model**
|       |               |   `-- UserEntity.java
|       |               `-- **service**
|       |                   `-- JwtUserDetailsService.java
|       `-- **resources**
|           `-- application.yml
```

# POM 文件依赖关系

```
spring-boot-starter-parent (version 2.2.2.RELEASE)
spring-boot-starter-web
spring-boot-starter-security
spring-boot-starter-data-jpa
jjwt (0.9.1)
mysql
lomboksee more details : [pom.xml](https://github.com/sophea/springboot2-jwt/blob/master/pom.xml)
```

<https://javarevisited.blogspot.com/2019/03/top-5-course-to-learn-apache-maven-for.html>  

# 安全配置

这个配置定义了 spring 安全性、密码编码器和 JWT 令牌认证

配置

# JWT 滤波器

此过滤器用于验证用户发送的 access_token 是否有效。

JwtRequestFilter.java

# JwtTokenService

此类服务用于生成令牌，从令牌中提取用户信息，以及验证令牌和到期时间。

JwtTokenService.java

# jwtuserdetaileservice

该类用于查询用户名/在数据库中保存新用户。

jwtuserdetaileservice

# **休息控制器**

假设我们使用 JTW 令牌为登录和注册新用户创建 REST-API

这个 JwtTokenController.java 类有两个 API

*   登录 API 的 POST/auth/token #
*   注册新用户的帖子/注册号码

# **仓库—道**

```
package com.sma.security.dao;

import com.sma.security.model.UserEntity;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<UserEntity, Long> {

    UserEntity findByUsername(String username);

}
```

# 域用户实体

```
package com.sma.security.model;

import com.fasterxml.jackson.annotation.JsonIgnore;
import lombok.Data;
import javax.persistence.*;

@Entity
@Table(name = "user")
@Data
public class UserEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.*IDENTITY*)
    private long id;
    @Column
    private String username;
    @Column
    @JsonIgnore
    private String password;

}
```

# 数据库 Mysql

![](img/2b463b0ab00eddcbc1ca24da3b1a8bf3.png)

数据库 mysql —用户表

# 资源属性(application.yml)

```
**jwt**:
    **secret**: jwtsecretkey
**spring**:
    **datasource**:
        **password**: password
        **platform**: mysql
        **url**: jdbc:mysql://localhost/jwt?createDatabaseIfNotExist=true&autoReconnect=true&useSSL=false
        **username**: root
    **jpa**:
        **hibernate**:
            **ddl-auto**: update
```

# REST APIs 测试

*   注册新用户

```
curl -X POST \
  http://localhost:8080/register \
  -H 'content-type: application/json' \
  -d '{"username":"test","password":"password"}'
```

*   登录 API 以交换令牌

```
curl -X POST \
  http://localhost:8080/auth/token \
  -H 'content-type: application/json' \
  -d '{ "username":"test", "password":"password"}'response back {
    "token": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0IiwiUk9MRVMiOlsiUk9MRV9VU0VSIl0sImV4cCI6MTU3ODE3ODI5NSwiaWF0IjoxNTc4MTYwMjk1fQ.gDMuDfp1_1kv729HrOWCskTm4rNCm7SSoniqDHZuRD5H1uUtFzbiktN9NVaTNaTvp14UqGavQygKhO3pTFOQJg"
}
```

*   无登录 API curl[http://localhost:8080/test](http://localhost:8080/test)
*   登录 API curl[http://localhost:8080/secure](http://locathost:8080/secure)

```
curl -X GET [http://localhost:8080/secure](http://localhost:8080/secure) -H 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0IiwiUk9MRVMiOlsiUk9MRV9VU0VSIl0sImV4cCI....'
```

# 结论

希望这篇文章能帮到你，扩大你的知识面。

如果这篇文章是有帮助的，请支持按下**按钮**并帮助其他读者到达这个故事。

这篇文章的完整源代码可以在 GitHub 上找到[。](https://github.com/sophea/springboot2-jwt)

</javarevisited/10-advanced-spring-boot-courses-for-experienced-java-developers-5e57606816bd>  </javarevisited/top-5-courses-to-learn-microservices-in-java-and-spring-framework-e9fed1ba804d> 