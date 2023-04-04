# Node.js 最佳实践—恶意命令

> 原文：<https://blog.devgenius.io/node-js-best-practices-malicious-commands-74eb63b98884?source=collection_archive---------4----------------------->

![](img/6b20543eea43caf0b6981c88b1872624.png)

扎卡里·斯皮尔斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 设置 Cookies

我们可以用 Express 设置 cookies。

我们可以使用 cookie 会话来发送 cookie。

例如，我们可以写:

```
const cookieSession = require('cookie-session');
const express = require('express');

const app = express();

app.use(cookieSession({
  name: 'session',
  keys: [
    process.env.COOKIE_KEY1,
    process.env.COOKIE_KEY2
  ]
}));

app.use((req, res, next) => {
  const n = req.session.views || 0;
  req.session.views = n++;
  res.end(n);
});

app.listen(3000);
```

我们使用 cookie 会话包来设置 cookie。

# CSRF

跨站点请求伪造是一种攻击，其中用户在他们登录的应用程序中执行不想要的操作。

这些攻击的目标是状态改变请求，因为它们看不到伪造请求的响应。

为了保护我们免受 CSRF 的攻击，我们可以使用`csrf`模块。

而在 Express 中，我们可以使用`csurf`模块。

例如，我们可以写:

```
const cookieParser = require('cookie-parser');
const csrf = require('csurf');
const bodyParser = require('body-parser');
const express = require('express');

const csrfProtection = csrf({ cookie: true });
const parseForm = bodyParser.urlencoded({ extended: false });

const app = express();

app.use(cookieParser());

app.get('/form', csrfProtection, (req, res) => {
  res.render('send', { csrfToken: req.csrfToken() });
});

app.post('/process', parseForm, csrfProtection, (req, res) => {
  res.send('submitted');
});
```

然后我们可以在模板中添加表单:

```
<form action="/process" method="POST">
  <input type="hidden" name="_csrf" value="{{csrfToken}}">

  Name: <input type="text" name="name">
  <button type="submit">Submit</button>
</form>
```

我们在表单中添加了一个带有`csrfToken`的隐藏输入，这样我们只能在出现 CSRF 令牌时提交表单。

# 数据有效性

我们应该验证我们的数据，以防止跨站点脚本。

当攻击者将恶意代码转化为带有特制链接的 HTML 时，就会出现跨站点脚本。

当应用程序存储未正确过滤的用户输入时，会出现存储的跨站点脚本。

它在应用程序中运行。

为了防止这种类型的攻击，我们应该总是过滤和杀毒用户输入。

# SQL 注入

另一种要防范的攻击是 SQL 注入。

我们在代码中动态运行 SQL 语句，这样我们就可以读取数据并进行恶意操作。

为了防止这些攻击，我们应该使用参数化查询或预处理语句。

像 node-postgres 模块这样的一些模块将允许我们创建如下的参数化查询:

```
const q = 'SELECT name FROM books WHERE id = $1';
client.query(q, ['1'], (err, result) => {});
```

[sqlmap](http://sqlmap.org/) 让我们在应用程序中自动检测和探索 SQL 注入缺陷。

我们可以用它来测试 SQL 注入漏洞。

# 命令注入

另一个可能出现的安全缺陷是命令注入。

我们可以将命令放在查询字符串中来运行 shell 命令。

为了防止这些攻击者，我们应该始终净化用户输入。

我们可以编写这样的代码:

```
https://example.com/downloads?file=%3Bcat%20/etc/passwd
```

另外，`child_process.exec`运行`/bin/sh`，所以它是 bash 解释器而不是程序启动器。

攻击者可以用反勾号或`$()`注入新命令。

我们可以用`child_process.execFile`克服这一点。

![](img/66197999ac51ad3ff988732d78ed12a5.png)

由 [Roi Dimor](https://unsplash.com/@roi_dimor?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以设置 cookies 并运行各种安全命令。

此外，我们需要防止恶意输入。