# Node.js 最佳实践—安全性

> 原文：<https://blog.devgenius.io/node-js-best-practices-security-e5df4ba59d20?source=collection_archive---------13----------------------->

![](img/52b79c287ecc2b14534fb5a0af23ec29.png)

Todd Quackenbush 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用中间件限制并发请求

攻击者很容易进行 DOS 攻击。

他们只需要用请求淹没我们的应用程序。

为了避免这种情况，我们可以使用负载平衡、防火墙、反向代理和速率限制器包来限制一次可以完成的请求。

有[灵活限速](https://www.npmjs.com/package/rate-limiter-flexible)和[快速限速](https://www.npmjs.com/package/express-rate-limit)包来帮助我们。

我们可以通过以下方式限制请求:

```
const http = require('http');
const redis = require('redis');
const { RateLimiterRedis } = require('rate-limiter-flexible');const redisClient = redis.createClient({
 enable_offline_queue: false,
});const rateLimiter = new RateLimiterRedis({
 storeClient: redisClient,
 points: 50,
 duration: 1,
 blockDuration: 5
});http.createServer(async (req, res) => {
  try {
    const rateLimiterRes = await rateLimiter.consume(req.socket.remoteAddress); res.writeHead(200);
    res.end();
  } catch {
    res.writeHead(429);
    res.end('Too Many Requests');
  }
})
 .listen(3000);
```

用`rate-limiterr-flexible`。

它将检查请求的数量，如果有太多的请求来自一个源，它将返回一个 429 状态代码。

有了快速限速器，我们可以写:

```
const RateLimit = require('express-rate-limit');
app.enable('trust proxy'); 

const apiLimiter = new RateLimit({
  windowMs: 10*60*1000, 
  max: 100,
});

app.use('/profile/', apiLimiter);
```

我们有`app.enable(‘trust proxy’);`来确保客户端 IP 被设置为`req.ip`的值。

我们使用 express-rate-limit 将请求窗口限制为每 10 分钟 100 个。

然后我们将中间件应用于`profile`路线。

# 从配置文件中提取机密或使用包来加密它们

不要在配置文件或源代码中存储秘密。

我们可以使用秘密管理系统来存储密钥。

它们也可以被管理和加密。

我们还应该检查预提交钩子，这样我们就不会意外地提交秘密。

例如，我们可以用`cryptr` 库解密:

```
const Cryptr = require('cryptr');
const cryptr = new Cryptr(process.env.SECRET);

const accessToken = cryptr.decrypt('...');
```

# 使用 ORM/ODM 库防止查询注入漏洞

我们应该防止 SQL 或 NoSQL 注入和其他恶意攻击，使用一个数据库库来逃避我们用于查询的数据。

此外，我们应该验证用户输入，这样恶意代码就不会被注入其中。

大多数 ORM 或 ODM 都有这个基本特性。

# 使用 SSL/TLS 加密客户端-服务器连接

SSL/TLS 几乎无处不在，因为它在我们的通信通道中提供了基本的安全性。

这可以防止中间人攻击。

因此攻击者无法通过接入通信信道来监视用户。

# 安全地比较秘密值和散列值

秘密值和散列不应该与纯文本进行比较。

我们可以使用`crypto.timingSafeEqual(a, b)`来比较哈希值。

它有两个对象，如果数据不匹配，它会一直比较。

默认的相等比较方法只在字符不匹配后返回，这允许计时攻击，因为我们可以检查比较的持续时间，以获得关于两个事物有多少匹配的线索。

![](img/aa5b3668f3ae13eab48cb1c2bb7059af.png)

斯雷科·斯克罗比奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

应该考虑一些基本的安全问题，包括存储机密、使用 SSL 和防止 DOS 攻击。