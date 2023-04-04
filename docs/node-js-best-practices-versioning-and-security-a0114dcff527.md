# Node.js 最佳实践—版本控制和安全性

> 原文：<https://blog.devgenius.io/node-js-best-practices-versioning-and-security-a0114dcff527?source=collection_archive---------3----------------------->

![](img/50131c66581f8c137cca7f30fd727367.png)

[Icons8 团队](https://unsplash.com/@icons8?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用语义版本控制

我们应该使用语义版本对我们的应用程序进行版本化。

这是约定俗成的，所以很多人会理解。

版本应该有主要版本、次要版本和错误修复版本，并用点分隔。

所以格式是 major.minor.bugfix

# 保护我们的应用

我们应该保护用户和客户的数据，这样我们就可以保护我们的应用程序免受任何攻击。

我们应该注意的事情包括安全 HTTP 头、暴力攻击等等。

在我们的 HTTP 响应中应该有一些头。

它们包括强制 HTTPS 连接到服务器的严格传输安全性。

X-Frame-Options 提供点击劫持保护。

X-XSS 保护启用最新 web 浏览器内置的跨站点脚本过滤器。

X-Content-Type-Options 阻止浏览器从声明的内容类型中嗅探响应。

内容安全策略可防止各种攻击，如跨站点脚本和其他跨站点注入。

我们可以通过头盔模块启用所有这些功能:

```
const express = require('express');
const helmet = require('helmet');

const app = express();

app.use(helmet());
```

还有 Koa 版本，就是 Koa-头盔模块。

此外，我们可以在 Nginx seer 中使用它来添加标题。

在`nginx.conf`中，我们可以添加:

```
add_header X-Frame-Options SAMEORIGIN;
add_header X-Content-Type-Options nosniff;
add_header X-XSS-Protection "1; mode=block";
add_header Content-Security-Policy "default-src 'self'";
```

# 客户端的敏感数据

我们不应该在源代码中暴露 API 的秘密和凭证，因为任何人都可以读取。

我们可以在代码审查中检查它们。

# 强力保护

我们的应用程序应该添加暴力保护，这样我们就可以防止这些攻击。

为此，我们添加了一个限速库来限制可以发出的请求。

我们可以使用 [ratelimiter](https://www.npmjs.com/package/ratelimiter) 包来限制函数被调用的次数:

```
const limit = new Limiter({ id, db });limit.get((err, limit) => {});
```

我们可以用它来编写 Express 中间件。

例如，我们可以写:

```
const ratelimit = require('koa-ratelimit');
const redis = require('redis');
constkoa = require('koa');
constapp = koa();const emailBasedRatelimit = ratelimit({
  db: redis.createClient(),
  duration: 100000,
  max: 10,
  id(context) {
    return context.body.email;
  }
});const ipBasedRatelimit = ratelimit({
  db: redis.createClient(),
  duration: 100000,
  max: 10,
  id(context) {
    return context.ip;
  }
});app.post('/login', ipBasedRatelimit, emailBasedRatelimit, handleLogin);
```

我们用速率限制中间件用`id`方法检查 ID 和电子邮件。

`duration`设置持续时间。

`db`是热地连接实例。

`max`是请求的最大数量。

# 会话管理

为了保护我们的 cookies，我们设置了几个标志。

其中之一就是`secure`旗。这告诉浏览器只在请求通过 HTTPS 发送时才发送 cookie。

`HttpOnly`用于帮助防止跨站点脚本等攻击，因为它不允许使用 JavaScript 访问 cookies。

域的范围也应该改变。

`domain`与被请求 URL 的服务器的域进行比较。

如果域匹配，那么路径也必须匹配。

一旦`path`被选中，cookie 将随请求一起发送。

`expires`是用来设置持久 cookies 的属性。过期日期过后，它们就会过期。

![](img/7f5c41e22e2b61e8980e7547982392ea.png)

照片由[凯勒·琼斯](https://unsplash.com/@gcalebjones?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们应该使用语义版本，并采取一些措施来保护我们的应用程序。