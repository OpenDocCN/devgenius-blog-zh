# NodeJS 和关闭 MySQL 连接——一项研究

> 原文：<https://blog.devgenius.io/nodejs-how-to-close-your-mysql-connections-and-why-a7cc7287132b?source=collection_archive---------1----------------------->

![](img/f038d87452b6ac068ab50a863340abcb.png)

约翰·巴克利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了不填满最大连接数，跟踪数据库连接非常重要。如果你不小心，你将很容易达到极限，导致可怕的错误“太多的连接”,当你试图连接到你的 MySQL 服务器，并导致数据库服务器窒息。

这篇文章研究了在使用 npm 包 [mysql2](https://github.com/sidorares/node-mysql2) 时，糟糕的(和良好的)编码会如何影响连接到 MySQL 数据库服务器的线程数量

MySQL 服务器的最大连接数由其变量“max_connection”决定，可以通过查询数据库服务器来读取:

```
SHOW VARIABLES LIKE 'max_connections';
```

要检查连接了多少个线程，我们可以运行以下查询:

```
SHOW STATUS WHERE `variable_name` = 'Threads_connected';
```

如果我们在一个数据库服务器上，我们知道没有其他服务器与之连接，最好是在您自己的笔记本电脑上，我们可能会看到以下结果:

```
Threads_connected 1
```

一个连接是你现在正在查找有多少个连接，所以你永远不会在这里看到 0。

在我的例子中，我安装了一个本地 MySQL 服务器，所以做一些不好的实践例子是安全的。因此，如果你想自己尝试这些不好的做法，请确保你不要在生产服务器上这样做！

## 创建连接

例如，下面的代码将创建 100 个连接，并使它们保持打开状态:

```
const mysql = require(‘mysql2’);for (let i = 0; i < 100; i++) {
  let connection = mysql.createConnection({
    host: ‘localhost’,
    user: ‘root’,
  });
}
```

让我们通过以下方式运行代码:

```
node your-file-name.js
```

现在检查我们的线程数:

```
Threads_connected 101
```

这是因为当您创建一个连接时，它将一直保持打开状态，直到您将其关闭。要关闭一个连接，我们可以使用 *connection.end()* ，所以让我们重新启动我们的 MySQL 服务器，以确保我们重置了所有连接，然后尝试下面的代码:

```
const mysql = require(‘mysql2’);for (let i = 0; i < 100; i++) {
  let connection = mysql.createConnection({
    host: ‘localhost’,
    user: ‘root’,
  });
  connection.end();
}
```

现在检查我们的线程数:

```
Threads_connected 1
```

这意味着我们所有的 100 个连接都被关闭了，这当然要好得多。但是，如果您打算在一个请求中多次查询您的数据库，设置一个**池**是最佳实践。

![](img/2d1053b10e16ba219ee5d88d5eacf947.png)

[科里·比约克](https://unsplash.com/@corybjork?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 使用池

那我们应该如何使用游泳池呢？让我们从如何不使用游泳池开始。如果我们重启我们的 MySQL 服务器，然后运行下面的代码:

```
const mysql = require(‘mysql2’);for (let i = 0; i < 100; i++) {
  let pool = mysql.createPool({
    host: ‘localhost’,
    user: ‘root’,
    connectionLimit: 10,
  });
}
```

如您所见，我们仍在循环一百次，但我们现在不是创建一个连接，而是创建一个池。池选项引入了一个新参数， *connectionLimit* 。默认情况下，它的值是 10，这意味着该池创建的连接不会超过这个限制。

现在检查我们的线程数:

```
Threads_connected 1
```

所以这比 *createConnection()* 要好，对吗？是的，只要我们不使用我们的游泳池，那有什么好处呢？不好。

要使用我们的池，我推荐通过[承诺包装器](https://github.com/sidorares/node-mysql2#using-promise-wrapper)来使用它(我实在看不出为什么不使用它)。

因此，让我们应用 promise 包装器，然后使用池进行查询，如下所示:

```
const mysql = require(‘mysql2’);for (let i = 0; i < 100; i++) {
  let pool = mysql
    .createPool({
      host: ‘localhost’,
      user: ‘root’,
      connectionLimit: 10,
    })
    .promise();
  pool.query(‘SELECT 1’)
    .then(res => {
  });
}
```

现在检查我们的线程数:

```
Threads_connected 101
```

换句话说，就像我们使用 *createConnection()* 时一样糟糕。但是(您可能已经明白了)池的整个概念是创建一次，然后多次重用。因此，让我们将池的创建移到循环之外:

```
const mysql = require(‘mysql2’);let pool = mysql
  .createPool({
    host: ‘localhost’,
    user: ‘root’,
    connectionLimit: 10,
  })
  .promise();for (let i = 0; i < 100; i++) {
  pool.query(‘SELECT 1’)
    .then(res => {
  });
}
```

如果我们重新启动我们的 MySQL 服务器，然后运行上面的代码，检查我们的连接线程数，现在的结果是:

```
Threads_connected 11
```

这意味着，根据我们的选项参数 *connectionLimit* ，我们的池使用了所有可用的连接，但是它们仍然保持开放，并且在我们的最大连接数中占据了插槽。

## 结束池

幸运的是，对于这个问题也有一个解决方案，它被称为 *pool.end()* ，所以让我们通过这样做来尝试一下:

```
const mysql = require(‘mysql2’);let pool = mysql
  .createPool({
    host: ‘localhost’,
    user: ‘root’,
    connectionLimit: 10,
  })
  .promise();for (let i = 0; i < 100; i++) {
  pool.query(‘SELECT 1’)
    .then(res => {
  });
}
pool.end();
```

如果我们运行这段代码，我们将得到一个错误，如*“池已关闭”*。(正如您可能会想到的)这是因为我们在异步查询完成之前关闭了池。

是时候引入 *async/await* 功能了(仅在 Node.js 版本 8 或更新版本中可用)。要使用 *await* ，我们需要遵循一些规则，正如 NodeJS 所说:

> await 只在异步函数和顶级模块体中有效。

因此，我将把我的代码包装在一个自调用的异步函数中，就像这样:

```
const mysql = require(‘mysql2’);(async () => {
  let pool = mysql
    .createPool({
      host: ‘localhost’,
      user: ‘root’,
      connectionLimit: 10,
    })
    .promise();
  for (let i = 0; i < 100; i++) {
    let rows = await pool.query(‘SELECT 1’);
  }
  pool.end();
})();
```

如果我们现在重新启动我们的 MySQL 服务器并运行上面的代码，检查我们的线程数，结果如下:

```
Threads_connected 1
```

## 达到我们的目标？

我们终于做到了。我们查询数据库 a 100 次，没有在连接的线程中留下任何痕迹。这不是很好吗？好吧，让我们继续学习。

如果您在本例中删除 *pool.end()* ，然后运行它，那么检查我们的线程数将会显示:

```
Threads_connected 2
```

这是因为我们从来没有并行运行查询，由于 *await* 正在等待查询在下一个查询之前完成，所以即使根据 *connectionLimit* 的值它有多达 10 个可用的连接，池也不会使用一个以上的连接。

如果您再次添加 *pool.end()* 并重新运行您的代码，而不重启 MySQL 服务器，那么检查我们的线程数将会显示:

```
Threads_connected 1
```

这是因为池使用了仍然保持打开的旧连接，然后在完成后关闭它。

那么，如果我们从来不使用一个以上的同时连接，为什么还要使用一个池呢？

那么，如果我们不只是手动调用 node.js 文件，或者设置一个调用它的 cron 作业，而是创建一个应该能够同时处理请求的 REST API 呢？

## 当使用 http 服务器时

让我们通过在 Node.js 中设置一个最小的 http 服务器来研究这个问题，如下所示:

```
const mysql = require(‘mysql2’);
const http = require(‘http’);
const pool = mysql
 .createPool({
  host: ‘localhost’,
  user: ‘root’,
  connectionLimit: 10,
 })
  .promise();
const server = http.createServer((req, res) => {
  if (req.method != ‘GET’ || req.url != ‘/’) {
    // exclude all other request (to favicon.ico etc)
    res.end();
    return;
  }
  // console.log(req.method, req.url);
  (async () => {
    for (let i = 0; i < 100; i++) {
      let rows = await dbPool.execute('SELECT 1');
    }
    pool.end();
  })();
  res.end();
});
server.listen(3000);
```

让我们浏览一下代码，这样我们就知道这里发生了什么。在顶部，我们需要来自节点本地库的 http 对象。

然后我们创建我们的池，这样只要我们的服务器在运行，它就可以在所有请求中使用。

然后我们创建一个 http 服务器。我们在服务器“内部”做的第一件事是过滤掉不需要的请求——因为我们希望能够在普通浏览器中测试我们的代码，当我们加载页面时，浏览器可能会执行额外的请求(例如检查 favicon.ico 等),我们希望确保我们的代码在这种情况下只执行一次。

那么您可能会认出我们之前的代码(需要使用 *res.end()* 部分来告诉 Node.js 结束请求)。

最后，我们确保监听端口 3000。

为了启动我们的服务器，我们像以前一样，在命令行中运行:

```
node your-file-name.js
```

这将启动我们的服务器，并让它对请求开放。

如果我们重启 MySQL 服务器，然后打开浏览器，进入 [http://localhost:3000，](http://localhost:3000,)，然后检查我们的连接，我们会看到:

```
Threads_connected 1
```

所以我们没有留下任何联系，这很好。至少看起来是这样。但是，如果我们再次重新加载我们的网页，我们得到错误“池是关闭的”。这是因为我们在第一个请求结束时关闭了我们的池。一个网页当然会管理多个请求，所以在这种情况下，我们不应该关闭我们的池，因为我们创建了它，所以只要它在运行，它就应该对整个应用程序/服务器可用。

因此，让我们删除 pool.end()，这将留给我们最后的代码:

```
const mysql = require(‘mysql2’);
const http = require(‘http’);
const pool = mysql
 .createPool({
  host: ‘localhost’,
  user: ‘root’,
  connectionLimit: 10,
 })
  .promise();
const server = http.createServer((req, res) => {
  if (req.method != ‘GET’ || req.url != ‘/’) {
    // exclude all other request (to favicon.ico etc)
    res.end();
    return;
  }
  // console.log(req.method, req.url);
  (async () => {
    for (let i = 0; i < 100; i++) {
      let rows = await dbPool.execute('SELECT 1');
    }
  })();
  res.end();
});
server.listen(3000);
```

如果我们现在:

1.  关闭我们的节点服务器(在命令行按下 *CTRL + C*
2.  通过 *node your-file-name.js* 再次启动我们的节点服务器

然后，我们可以在浏览器中多次重新加载我们的网页，在我们的示例中，活动连接永远不会超过 2 个。如果您同时执行查询(而不是 async/await ),您将永远不会使用超过 10 个连接(就像我们的 *connectionLimit* 中那样)打开 MySQL 服务器。

这项研究到此为止。请在下面的评论中提出意见和批评，这样我们都可以改进。