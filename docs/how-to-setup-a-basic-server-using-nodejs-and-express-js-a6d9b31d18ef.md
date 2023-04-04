# 如何使用 NodeJS 和 Express.js 设置基本服务器

> 原文：<https://blog.devgenius.io/how-to-setup-a-basic-server-using-nodejs-and-express-js-a6d9b31d18ef?source=collection_archive---------0----------------------->

当使用 nodeJS 和 javascript 构建服务器时，Express 是最可取的框架，今天，我们将了解如何设置一个基本的 express 服务器，我们将尽可能使它简单，没有进一步的讨论，让我们开始吧:

![](img/d6ee7c3a158bb7d004da3fecee5a0132.png)

快递. js

# 1.初始化 NodeJS 项目:

首先，您需要初始化一个简单的 NodeJS 项目，您可以使用下面的 npm 命令简单地完成这项工作:

```
npm init
```

之后，将为您生成几个问题来初始化您的 NodeJS 项目，如果您不想回答这些问题并将其保留为默认值，您可以使用:

```
npm init -y 
```

或者:

```
npm init --yes
```

# 2.安装所需的依赖项:

成功初始化 NodeJS 项目后，您就可以安装所有需要的依赖项来开始设置您的服务器了。第一个也是最重要的依赖项是 express 包，您可以使用 npm install 简单地安装它:

```
npm install express 
```

另外，我强烈建议的一个非常重要的步骤是安装 nodemon 依赖项，什么是 nodemon？很高兴你问了这个问题:

> **nodemon** 是一款帮助开发 node 的工具。当检测到目录中的文件改变时，通过自动重启节点应用程序来启动基于 js 的应用程序。

基本上，使用 nodemon，当您在 nodeJS 项目中保存一个文件时，您的服务器将自动重启，为了安装 nodemon，您需要键入:

```
npm install nodemon --save-dev
```

Nodemon 需要另一个步骤来准备工作，这个步骤是在根目录的 package.json 文件中添加运行 nodemon 的命令，在“脚本”下添加这一行:

```
"dev": "nodemon index.js"
```

现在，无论何时你想运行你的服务器，你只需在终端上键入:

```
npm run dev
```

否则，如果您不想使用 nodemon，只需在终端上键入:

```
npm start
```

每当您在项目中进行更改并希望重启服务器时，您只需进入终端，使用 ctrl+c(对于 windows 和 Linux)或 Command+C(对于 macOS)，然后再次键入“npm start”。

# 3.设置 Express 服务器:

现在，您已经准备好设置您的服务器了，首先，在项目的根目录下创建一个 index.js 文件。

然后，要求快递包装如下:

```
const express = require("express");
```

然后像这样简单地创建 express 的 app 对象:

```
const app = express();
```

如果你想要一种奇特的方法(这是我个人使用的)，你可以简单地忽略上面的两行，用下面的一行代替:

```
const app = require("express")();
```

现在，您需要设置一个您的服务器将在其上运行的端口，如果您不知道端口是什么，您可以查看此视频，它解释了端口在网络世界中的含义: [link](https://youtu.be/7HwtRrnPOVk) ，要为您的服务器选择一个端口，您需要首先在一个单独的变量或常量上定义您的端口号(完全可选的步骤)，然后在 app 对象中将其传递给 listen 方法，listen 方法的第二个参数将是一个回调函数，每当服务器成功运行时都会调用该函数:

```
const port = 5000;
app.listen(
    port,
    () => console.log(`Server Running On Port ${port}...`)
);
```

**建议:**当你要为你的服务器选择端口号时，避免使用一些著名的端口号，如 80、8080 或 20，因为所有这些端口通常被你计算机上的其他服务和应用程序使用。

# 4.路由:

现在，您需要做的就是设置用户在访问您的服务器中的一条路线时将获得或看到什么数据，让我们假设我有一个运行在本地主机上的服务器，当我访问: [http://localhost:5000/](http://localhost:5000/) 或[http://localhost:5000/](http://localhost:5000/)home 时，我会看到一些错误消息，比如:

```
Cannot GET /
```

或者:

```
Cannot GET /home
```

为了设置这些链接中到底会显示什么，我们需要使用 routing，这是一个例子:

```
app.get("/", (req,res) =>{
    return res.send("Hello World!!!");
});
```

首先，我们确定当用户调用我们的服务器时，服务器将响应什么操作，我们在这个例子中使用 GET，但是也可以执行其他操作，比如 GET、POST、PUT、PATCH、DELETE 等等。get 方法得到两个参数，第一个是指定的路由，第二个是回调函数，当用户访问指定的路由时将执行该函数，这里我们使用 res.send 方法，它负责将数据发送回用户。基本上，所有这些代码意味着，当用户访问 [http://localhost:5000/](http://localhost:5000/) 时，将会在浏览器上看到一条消息，上面写着“Hello World ”,同样需要注意的是，我们可以使用 res.send 方法向用户发回 HTML:

```
res.send("<h1>Hello World</h1>");
```

浏览器会把你的信息解释成普通的 HTML。

# 4.服务器静态文件:

使用 res.send 为用户呈现使用 HTML、CSS 和 Javascript 编写的大型复杂网站是不切实际的，如果这是您设置 express web 服务器的主要目标，那么我建议您考虑使用 express.js 提供静态文件。

首先，您需要使用 express.static 指定网站代码的存储位置，然后使用 app.use 函数来完成配置，让我们假设您网站的代码源存储在项目根目录下的 public 文件夹中，以便为您需要键入的用户呈现该网站:

```
app.use(express.static('public'));
```

瞧，您已经使用 express.js 构建了一个 web 服务器，可以部署了。

最终，我希望这篇短文能赢得您的赞赏，并感谢您的关注。