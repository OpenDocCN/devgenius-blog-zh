# API 授权

> 原文：<https://blog.devgenius.io/api-authorization-with-jwt-53c349a19361?source=collection_archive---------7----------------------->

在本文中，我们将讨论 JWT (Json Web Token)是什么，它有什么用，以及如何将它添加到 REST API 中。

## 定义

首先，我将回顾一些定义。

**认证**:“验证用户或进程身份的过程或动作”(你就是你所说的那个人)

**授权:**“给予某人做某事或拥有某物许可的过程”(假设你是这个人，你被允许访问这个东西吗)

## 例子

假设我们有一个银行网站，在那里我们显示机密的用户信息。为了向用户显示这些信息，我们必须。

1.  使用密码和用户名对他们进行验证(例如，这个人是 Sally)
2.  授权他们查看他们请求的页面上的数据(即 Sally 不能查看 John 的银行对账单)

## 这是什么？

Json Web 令牌是一种新的授权方法，它利用加密散列在客户机和服务器之间安全地传输用户元数据。

对于普通人来说，令牌本身只是一堆毫无意义的字符，但是如果您有正确的密钥，就可以提取用户元数据。例如，`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiU2FsbHkifQ.u7gUUVZWpG_ur96XDyNEuIHFeyFZlYNE22we9cFAMpk`是对有效载荷进行编码的 JWT

```
{
  "user": "Sally"
}
```

用秘钥“秘密”🤫。你可以在这个网站上玩杂凑游戏[https://jwt.io/](https://jwt.io/)

![](img/0bc087d09f4b0bba73dcf3475062a17c.png)

如果你知道这个秘密，你也可以把 JWT 解码成有效载荷。

## **怎么有用？**

现在我们知道令牌只是一个加密字符串，那么它有什么帮助呢？我们可以用它给用户一张通行证去访问他们被允许访问的东西。

![](img/c2c9673b9fcc68bb53600232eb97efbc.png)

**步骤**

1.  客户端发送用户名和密码
2.  服务器对用户进行身份验证，并使用关于用户的一些元数据，通过密钥创建 JWT
3.  JWT 通过 HTTP 头在/登录响应中发送回客户端，客户端将 JWT 存储在本地存储中
4.  在对服务器的后续请求中，客户端在请求头中提供这个令牌
5.  服务器使用密钥解密 JWT，找出试图访问端点的人
6.  服务器向知道他们是谁的客户端发送响应

由于这是一个加密散列，人们不能伪造他们是谁，也就是说，为了创建`{“user”:”John”}`的 JWT，我需要知道密钥。我也不能在不知道密钥的情况下拿走一个 JWT 并找出它属于谁，因为也需要它来解密 JWT。

使用它，我们可以为一组付费用户创建高级端点，使用用户特定数据个性化 api 结果，阻止用户访问其他用户的数据，即使他们知道要访问哪些用户数据的 URL。

## 将 JWT 添加到 API 中

现在让我们看一个例子，利用 JWT 授权创建一个 express 服务器。

```
const express = require('express')
const jwt = require('jsonwebtoken');
const app = express()
app.use(express.json());
const port = 3000

app.post('/authenticate', (req, res) => {
    //AUTHENTICATE HERE

    //create jwt using user info
    console.info(req.body)
    const {username} = req.body;
    const token = jwt.sign({username: username}, "SECRET", { expiresIn: '30s' });
    console.info(token);

    //send token
    res.setHeader("x-auth-token", token)
    res.sendStatus(200)
})

app.get("/guarded", (req, res) => {
    if (req.header("x-auth-token") == null) {
        // USER DIDN'T SUPPLY TOKEN
        return res.status(400).send("NO AUTH TOKEN SUPPLIED");
    }
    try {
        const decoded = jwt.verify(req.header("x-auth-token"), 'SECRET');
        console.info(decoded);
        //could use the user to personalize some db query or guard a page for only some users
        res.send(decoded["username"]);
    } catch (e) {
        // USER MESSED WITH TOKEN
        return res.status(400).send("INVALID AUTH TOKEN PROVIDED")
    }
})

app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
})
```

这是完整的代码，以获得一个认证路线和一个受保护的(只有授权的个人)路线。

**/认证**

```
app.post('/authenticate', (req, res) => {
    //AUTHENTICATE HERE

    //create jwt using user info
    console.info(req.body)
    const {username} = req.body;
    const token = jwt.sign({username: username}, "SECRET", { expiresIn: '30s' });
    console.info(token);

    //send token
    res.setHeader("x-auth-token", token)
    res.sendStatus(200)
})
```

首先，您将验证用户并取回他们的元数据，这些元数据将在有效载荷中使用(我只是将提供的主体作为有效载荷)。然后，我们用我们的“秘密”密钥对令牌进行签名，并设置一个 30 秒的过期时间(因此这个令牌将在 30 秒后失效)。然后我们在一个头中将它发送给用户。

![](img/1504e533dffbbe18378f5c3334b08e93.png)

**/守护**

```
app.get("/guarded", (req, res) => {
    if (req.header("x-auth-token") == null) {
        // USER DIDN'T SUPPLY TOKEN
        return res.status(400).send("NO AUTH TOKEN SUPPLIED");
    }
    try {
        const decoded = jwt.verify(req.header("x-auth-token"), 'SECRET');
        console.info(decoded);
        //could use the user to personalize some db query or guard a page for only some users
        res.send(decoded["username"]);
    } catch (e) {
        // USER MESSED WITH TOKEN or TOKEN EXPIRED
        return res.status(400).send("INVALID AUTH TOKEN PROVIDED")
    }
})
```

受保护的路由将首先检查请求者是否在其报头中提供了身份验证令牌，如果没有，我们将发回“未提供身份验证令牌”。然后，我们尝试验证和解码令牌。如果验证失败，它会抛出一个错误，因此我们会捕获它并返回“提供了无效的 AUTH TOKEN”。最后，如果用户提供了有效的 auth 令牌，我们只需将他们的用户名(通过解码有效负载获得)发送给他们。

![](img/6aa182a08b7ce70c9cd97b8fd0e4d457.png)

现在您已经准备好向您的 API 添加授权了！