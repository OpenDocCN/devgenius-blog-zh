# Node.js 最佳实践—环境和集群

> 原文：<https://blog.devgenius.io/node-js-best-practices-environment-and-clusters-8b23b96bc783?source=collection_archive---------5----------------------->

![](img/ce3428c016f28be78b9c7355b744a239.png)

由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# DevOps 工具

DevOps 工具将使配置我们的环境和服务器设置更加容易。

此外，我们需要一个流程管理器来自动重启我们的 Express 应用程序，以防它崩溃。

我们需要一个反向代理和负载平衡器来将我们的应用程序公开给互联网，缓存请求，并在多个工作进程之间平衡负载。

这让我们在应用程序中保持高性能。

# 用 dotenv 管理 Node.js 中的环境变量

dotenv 库对于让我们从一个`.env`文件中读取变量非常有用

例如，我们可以写:

```
NODE_ENV=production
DEBUG=false
```

在我们的`.env`文件中。

然后我们可以用:

```
require('dotenv').config()
​
const express = require('express')
const app = express()
```

默认情况下，它会从名为`.env`的文件中读取环境变量。

但是，它也可以从多个环境变量文件中读取。

# 确保应用程序通过进程管理器自动重启

如果我们有一个 Express 应用程序，当遇到未处理的错误时，它会崩溃。

因此，重要的是我们要自动重启我们的应用程序，这样它就不会关闭。

我们可以通过在`/lib/systemd/system`中创建一个名为`app.service`的新文件来使用 Systemd:

```
[Unit]
Description=Node.js as a system service.
Documentation=https://example.com
After=network.target
[Service]
Type=simple
User=ubuntu
ExecStart=/usr/bin/node /my-app/server.js
Restart=on-failure
[Install]
WantedBy=multi-user.target
```

`Service`部分有`ExecStart`行，它在启动时运行我们的应用程序。

它还有一条`Restart`线，可以在出现故障时重启。

然后，我们可以重新加载守护程序，用以下命令启动脚本:

```
systemctl daemon-reload
systemctl start fooapp
systemctl enable fooapp
systemctl status fooapp
```

# PM2

PM2 是一个流程经理，让我们管理快速应用程序流程。

我们可以通过运行以下命令来安装它:

```
npm i -g pm2
```

然后我们可以通过运行以下命令来运行我们的应用程序:

```
pm2 start server.js -i max
```

`-i max`是运行我们应用的最大线程数。

这样，它将产生足够的工人来使用所有的 CPU 核心。

# 负载平衡和反向代理

节点集群模块允许我们生成服务于我们的应用程序的工作进程。

我们可以通过编写以下内容来创建集群:

```
const cluster = require('cluster')
const numCPUs = require('os').cpus().length
const app = require('./src/app')
const port = process.env.PORT || 8888
​
const masterProcess = () => Array.from(Array(numCPUs)).map(cluster.fork)
const childProcess = () => app.listen(port)
​
if (cluster.isMaster) {
  masterProcess()
} else {
  childProcess()
}
​
cluster.on('exit', () => cluster.fork())
```

主进程创建集群，子进程在给定的端口上按 listen 来监听请求。

`masterProcess`计算有多少 CPU 内核，并调用`cluser.fork`创建与可用内核数量相等的子进程。

如果因`cluster.fork`而失败，`exit`事件监听器将重启进程。

然后在 Systemd 文件中，我们替换:

```
ExecStart=/usr/bin/node /my-app/server.js
```

使用:

```
ExecStart=/usr/bin/node /my-app/cluster.js
```

然后我们可以使用以下命令重新启动 Systemd:

```
systemctl daemon-reload
systemctl restart fooapp
```

![](img/d9a8eccc3b8c3825f43a7f575fc80032.png)

照片由[迈克尔·泽兹奇](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以用 dotenv 读取环境变量，用`cluster`模块添加集群。