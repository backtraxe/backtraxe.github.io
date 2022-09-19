# Java 项目面试


<!--more-->

## JWT

### 什么是 JWT

- JWT（JSON Web Token）是一个开放标准（RFC 7519），它定义了一种紧凑且自包含的方式，以 JSON 对象的形式在各方之间安全地传输信息。
- JWT 是一个数字签名，生成的信息是可以验证并被信任的。
- 使用密钥（使用 HMAC 算法）或使用 RSA 或 ECDSA 的公钥/私钥对 JWT 进行签名。
- JWT是目前最流行的跨域认证解决方案。

### JWT 令牌结构

JWT 令牌以紧凑的形式由三部分组成，这些部分由点（.）分隔，分别是：

- Header
- Payload
- Signature

`xxxx.yyyy.zzzz`

#### Header

Header 通常由两部分组成：令牌的类型（即 JWT）和所使用的签名算法（例如 HMAC SHA256 或 RSA）。

```json
{
    "alg": "HS256",
    "typ": "JWT"
}
```

Header 会被 Base64 编码为 JWT 的第一部分。

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

#### Payload

Payload 是有关实体（通常是用户）和其他数据的声明，它包含三部分：

1. 注册声明。这些是一组预定义的权利要求，不是强制性的，而是建议使用的，以提供一组有用的可互操作的权利要求。其中一些是： iss（JWT的签发者）， exp（expires,到期时间）， sub（主题）， aud（JWT接收者），iat(issued at，签发时间)等。
1. 公开声明。公共的声明可以添加任何的信息，一般添加用户的相关信息或其他业务需要的必要信息.但不建议添加敏感信息，因为该部分在客户端可解密。
1. 私有声明。私有声明是提供者和消费者所共同定义的声明，一般不建议存放敏感信息，因为 Base64 是对称解密的，意味着该部分信息可以归类为明文信息。

```json
{
    "iat": 1593955943,
    "exp": 1593955973,
    "uid": 10,
    "username": "test",
    "scopes": [ "admin", "user" ]
}
```

Payload 会被 Base64 编码为 JWT 的第二部分。

```text
eyJ1c2VybmFtZSI6InRlc3QiLCJpYXQiOjE1OTM5NTU5NDMsInVpZCI6MTAsImV4cCI6MTU5Mzk1NTk3Mywic2NvcGVzIjpbImFkbWluIiwidXNlciJdfQ
```

#### Signature

Signature 部分的生成需要 Base64 编码之后的 Header，Base64 编码之后的 Payload，密钥（secret），Header 需要指定签字的算法。

例如，如果要使用 HMAC SHA256 算法，则将通过以下方式创建签名：

```text
HS256(Base64(Header) + "." + Base64(Payload), secretKey)
```

#### 整体

```text
JWT = Base64(Header) + "." + Base64(Payload) + "." + $Signature
```

输出是三个由点分隔的 Base64-URL 字符串，可以在 HTML 和 HTTP 环境中轻松传递这些字符串，与基于 XML 的标准（例如 SAML）相比，它更紧凑。

JWT 是无状态授权机制，服务器的受保护路由将 Header 中检查有效的 token，如果存在，则将允许用户访问受保护的资源。如果JWT 包含必要的数据，则可以减少查询数据库中某些操作的需求。

### 什么时候使用 JWT

- 授权：一旦用户登录，每个后续请求将包括 JWT，从而允许用户访问该令牌允许的路由，服务和资源。单一登录是当今广泛使用 JWT 的一项功能，因为它的开销很小并且可以在不同的域中轻松使用。这也是 JWT 最常见的方案。
- 信息交换：JWT 是各方之间安全地传输信息的好办法。对 JWT 进行签名，所以您可以确保发件人是他们所说的人。由于签名可以设置有效时长，所以可以验证内容是否遭到篡改。

### 如何使用 JWT

{{< figure src="/imgs/jwt.jpg" title="JWT 认证流程" >}}

### 用户登出，如何设置 token 无效？

使用 redis 数据库，用户登出，从 redis 中删除对应的 token，请求访问时，需要从 redis 库中取出对应的 token，若没有，则表明已经登出。

### 不同设备，一个设备登出，其他设备如何处理？

1. 每一个设备与用户生成唯一的 key，保存在 redis 中，即设备 1 的用户登出，只删除对应的 token，设备 2 的 token 仍然存在
1. 服务器端维护一个版本号，相同用户不同设备登入，版本号加1，这样保持key的唯一性（和上面差不多）


## 参考

1. [JWT详解_baobao555#的博客-CSDN博客_jwt](https://blog.csdn.net/weixin_45070175/article/details/118559272)

