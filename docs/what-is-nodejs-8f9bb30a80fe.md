# 什么是 NODEJS？如何在不安装 NODEJS 运行时和 NPM 的情况下运行 NODEJS 项目？

> 原文：<https://blog.devgenius.io/what-is-nodejs-8f9bb30a80fe?source=collection_archive---------13----------------------->

![](img/e389d91f25c8d1ae83869c30cf2a2f39.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Pietro Jeng](https://unsplash.com/@pietrozj?utm_source=medium&utm_medium=referral) 拍摄

# (1)什么是 NODEJS？

Node.js (NODEJS)是一个开源的、跨平台的 JavaScript 运行时环境(参考 [Node.js](https://nodejs.org/en/) )，它以能够运行客户端和服务器应用程序而闻名(参考[微软——什么是 NODEJS](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-overview) )。JavaScript(包括 NODEJS)连续第七年成为最常用的编程语言(参考 [StackOverFlow](https://insights.stackoverflow.com/survey/2019#:~:text=javascript%20is%20the%20most) )。

## NODEJS 安装

NODEJS 和节点包管理器(NPM)可以通过三种方法安装。

* [直接安装](https://nodejs.org/dist/v18.12.1/)。例如 [node-v18.12.1-x86.msi](https://nodejs.org/dist/v18.12.1/node-v18.12.1-x86.msi)

* [通过软件包管理器](https://nodejs.org/en/download/package-manager/)。例如 [choco 安装节点 js](https://community.chocolatey.org/packages/nodejs)

*直通码头集装箱

# (2)不安装 NODEJS 运行时和 NPM，如何运行 NODEJS 项目？

可以在不安装 NODEJS 运行时和 NPM 的情况下运行 NODEJS 项目，即通过 Docker 容器方法。

*以下步骤专门针对 Windows 和 PowerShell 环境。但是，稍加修改，这些步骤也适用于 Linux 和 Mac OS。*

**(2.1)从 Docker Hub 中拉出 NODEJS 镜像。**

例如，使用 Bitnami Docker 图像(参考[https://hub.docker.com/r/bitnami/node/](https://hub.docker.com/r/bitnami/node/))。该映像包含预安装的 NODEJS 和 NPM 软件。

```
docker pull bitnami/node:18.12.1
```

**(2.2)运行临时容器准备项目。**

假设我们想要创建一个运行 web 服务器的 NODEJS 项目(使用 [Express Node Package](https://www.npmjs.com/package/express) )。

首先，创建一个项目工作目录(PWD)。比如创建`prjnode`。下面是创建`prjnode`目录然后移入该目录的终端命令。

```
mkdir prjnode

cd prjnode
```

接下来，运行 docker 命令将上面的 PWD 绑定到 docker 容器，然后安装 [Express 节点包](https://www.npmjs.com/package/express)。

```
docker run -it --rm --name cnode -v ${PWD}:/app bitnami/node:18.12.1 npm install express
```

参数的含义:

```
-it = run the command in interactive mode.

--rm = remove the container after the command execution is completed.

--name cnode = give the container a name 'cnode'.

-v = bind the PWD directory in the host machine to 'app' directory in the container.

npm install express = use NPM to install Express web server
```

或者，首先创建 package.json 文件，然后按照下面的步骤运行 npm install 命令(参见[NODEJS—Dockerizing NODEJS App](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/))

```
notepad package.json 
```

在记事本中，粘贴以下代码。

```
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last <first.last@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}
```

在容器中运行 npm install 命令。

```
docker run -it --rm --name cnode -v ${PWD}:/app bitnami/node:18.12.1 npm install
```

**(2.3)编辑 NODEJS 脚本**

在 powershell 终端中，键入命令打开记事本程序。

```
notepad myexpress.js
```

输入以下代码(参考 [Bitnami Docker 节点示例](https://hub.docker.com/r/bitnami/node#:~:text=example%20of%20an%20express.js))。这些只是输出字符串“Hello World！”的基本代码通过网址。

```
var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});

var server = app.listen(3000, '0.0.0.0', function () {

  var host = server.address().address;
  var port = server.address().port;

  console.log('Example app listening at http://%s:%s', host, port);
});
```

**(2.4)运行容器中的脚本**

运行 Docker 命令，通过临时容器执行脚本。

```
docker run -it --rm --name cnode -p 8081:3000 -v ${pwd}:/app bitnami/node node myexpress.js
```

参数的含义:

```
-p 8081:3000 = bind the container's internal port 3000 to the host machine's port 8081

node myexpress.js = use a node command to parse myexpress.js
```

使用 web 浏览器浏览 URL [http://localhost:8081。字符串“你好，世界！”应该出现在浏览器窗口中。](http://localhost:8081.)

要退出 Docker 命令，请按[CTRL]+P+Q 键。

要停止容器，键入`docker stop cnode`。