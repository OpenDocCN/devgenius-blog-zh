# 在 Node.js 中使用 JSON Web 令牌

> 原文：<https://blog.devgenius.io/working-with-json-web-tokens-in-node-js-49d3bb83459c?source=collection_archive---------6----------------------->

🚀 [**打造分层微服务**](https://learnbackend.dev/books/build-layered-microservices) 这本书出来了！现在就在 [learnbackend.dev](https://learnbackend.dev/books/build-layered-microservices) 购买你自己的副本。

![](img/2182ad04bef8ff43013a9ec8b0c358c3.png)

JSON Web 令牌**通常缩写为 JWT(读作“jot”)是一个紧凑的、自包含的和数字签名的令牌，它使用 JSON 格式在各方之间传输信息。**

除了它的加密方法之外，使它受欢迎的是 JSON 格式的使用，例如，它比 XML 更紧凑，这使得它更容易在 web 上操作，并且通常更容易使用，因为 JSON 解析器在大多数编程语言中非常常见。

在本文中，您将了解如何使用`jsonwebtoken`包对 Node.js 中的**签名**和**解码** JSON web 令牌。

# JWT 的解剖

一个 **JWT** 由三部分组成:头部**头部**、**有效载荷**和**签名**，各部分之间用一个点字符隔开。

```
header.payload.signature
```

## JWT 头球

头部是一个由两个字段组成的`Base64Url`编码对象:令牌的类型**和签名算法**。

```
{
  "typ": "JWT",
  "alg": "HS256"
}
```

## JWT 有效载荷

有效载荷也是一个`Base64Url`编码的对象，包含关于一个实体(通常是一个用户)的信息片段，以及称为**声明**的附加数据，可以有三种类型:

*   **注册声明**是非强制性的预定义声明，可以指定这些声明来确保与第三方或外部应用程序的互操作性，例如令牌的颁发者或其过期时间。
*   **公开声明**通常是通用的非敏感信息，如用户全名或电子邮件地址。
*   **私有声明**通常是应用特定的敏感信息，如内部用户 ID。

```
{
  "exp": 1638298179,            // registered
  "name": "John Doe",           // public
  "email": "johndoe@mail.net",  // public
  "internal_id": "admin1234"    // private
}
```

## JWT 签名

最后，通过使用报头中指定的算法、秘密值以及编码报头和有效载荷的点连接值的组合来创建**签名**。

```
HmacSHA256(
  secret,
  base64url(header) + '.' + base64url(payload)
)
```

# jsonwebtoken 包

在 Node.js 中操作 jwt 最流行的包是`jsonwebtoken`，可以使用下面的命令安装:

```
$ npm install --save jsonwebtoken
```

## 签署新的 JWT

为了生成(即签名)一个新的 JSON web 令牌，我们可以使用顶级的`sign()`方法，它有三个参数:

*   一个**有效载荷**，它是我们想要发送给客户端的数据(例如用户 ID)。
*   一个**秘密**或一个**私有密钥**，它或者是一个字符串(例如“myJWTSecret”)或者是一个 PEM 文件，可以使用诸如`ssh-keygen`之类的工具生成。
*   一个非强制的**选项**对象，它包含额外的或元数据，如算法、到期日期或令牌的颁发者(参见声明)。

```
const jwt = require('jsonwebtoken');

const payload = {
  name: "John"
};
const secret = "mysecret";
const token = jwt.sign(payload, secret);

console.log(token);
```

运行上述脚本，将产生以下输出:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiSm9obiIsImlhdCI6MTY1OTAwODQxMH0.w-z_Ht5B4PLQFUQJhymbERqudUboCVtmM24t2y-UjsE
```

## 定义截止日期

通过互联网等网络传输的每一条信息都有可能被窃取。因此，通常建议为 JWT 定义一个到期日期，这样，被盗的令牌不会授予攻击者对受保护资源的无限访问权。

第一种方法是在`payload`对象中定义一个`exp`声明，它包含从纪元(1970 年 1 月 1 日)开始到到期日的秒数。

```
const jwt = require('jsonwebtoken');

const payload = {
  name: "John",
  exp: Math.floor(Date.now() / 1000) + (60 * 60)  // 1h
};
const secret = "mysecret";
const token = jwt.sign(payload, secret);
```

第二种方法是在`options`对象上添加一个`expiresIn`字段，其中包含描述时间跨度的字符串。

```
const jwt = require('jsonwebtoken');

const payload = {
  name: "John"
};
const secret = "mysecret";
const options = {
  expiresIn: "1h"
};
const token = jwt.sign(payload, secret, options);
```

## 解码 JWT

要解码一个 JSON web 令牌，我们可以使用模块导出的顶级`verify()`方法，它有三个参数:

*   一个**令牌**，它是要解码的 JSON web 令牌。
*   一个**秘密**或一个**公开密钥**，它或者是一个字符串(例如“myJWTSecret”)或者是一个 PEM 文件，可以使用诸如`ssh-keygen`之类的工具生成。
*   一个非强制性的**选项**对象执行额外的令牌验证。

```
const jwt = require('jsonwebtoken');

const token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiSm9obiIsImlhdCI6MTY1OTAwODQxMH0.w-z_Ht5B4PLQFUQJhymbERqudUboCVtmM24t2y-UjsE";
const secret = "mysecret";
const payload = jwt.decode(token, secret);

console.log(payload);
```

运行上述脚本，将产生以下输出:

```
{ name: 'John', iat: 1659008410 }
```

# 下一步是什么？

👉你喜欢这种内容？查看 https://learnbackend.dev 上的书籍[**Build Layered micro services**](https://learnbackend.dev/books/build-layered-microservices)了解如何使用 Express framework 构建生产就绪的分层认证微服务，从第一行代码到最后一行文档，该服务在开发实践和软件架构方面符合行业标准。