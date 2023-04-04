# 管理 NodeJS 中的多个环境文件

> 原文：<https://blog.devgenius.io/managing-multiple-environment-file-in-nodejs-4d3be879c2f9?source=collection_archive---------1----------------------->

![](img/f831ac8c8e645a9665b2ffb97fd87477.png)

Pic 信用代码动作

## 问题

在编写 NodeJS 应用程序时，我发现很难管理不同的环境变量。对于阅读环境变量是使用 dotenv 作为其余的开发人员。我的方法是只创建一个。env 文件并写下其中的所有变量文件。所以问题就在这里，我在一个文件中提到了所有的东西，这真的很难管理。

```
NODE_ENV=development
PORT=3000# Database
DB_HOST=
DB_USER=
DB_PASS=
DB_NAME=
```

然后，我开始根据环境创建不同的文件，例如对于开发，我的文件名将是. env.development，对于其他环境也是如此，我开始将. env. <env>文件复制到。基于环境的 env 文件。假设我正在开发环境中工作，那么我将把. env.development 文件复制到。env 并更改变量值，然后开始工作。环境的其他部分也是如此。但是这里仍然需要人工干预。假设有人忘记更改/复制变量值，那么您可能会得到不同的内容/应用程序可能会中断。</env>

## 问题的解决方案

我发现了一个非常好的包， [env-cmd](https://www.npmjs.com/package/env-cmd) 。这是一个简单的节点程序，用于使用 env 文件中的环境执行命令。

## 如何安装 [env-cmd](https://www.npmjs.com/package/env-cmd) ？

您可以运行以下命令在项目中安装 env-cmd。在这里，我将 env-cmd 作为 devDependencies 安装

```
npm install env-cmd --save-dev
```

要使用这个包，您必须做的是，为每个环境创建环境文件。比如对于开发环境，你可以创建 *.env.development* 或者对于生产环境，你可以创建 *.env.production* 等等。

这里我为开发创建了一个 env 文件。文件名 **.env.development**

```
NODE_ENV=development
PORT=3000
DB_HOST=
DB_USER=
DB_PASS=
DB_NAME=
```

## 如何在 package.json 命令中添加 env-cmd？

现在，您可以在 package.json 中创建的命令中传递 env 文件名，如下所示

```
{
  "name": "env-tuts",
  "version": "1.0.1-RC1",
  "description": "Env Tuts",
  "private": true,
  "scripts": {
    "development": "npx env-cmd -f .env.development node index.js",
    "production": "npx env-cmd -f .env.production node index.js",
    "uat": "npx env-cmd -f .env.uat node index.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "env-cmd": "^10.1.0"
  },
  "license": "Apache v2.0"
}
```

在脚本部分，您可以看到我添加了 npx env-cmd <env-file-name>node index.js。这个命令将帮助我们根据环境获取环境变量。</env-file-name>

> **请注意**，您可以根据需要用任何其他命令替换 node index.js。比如，如果你想用 nodemon 运行你的应用程序，只需用 nodemon index.js 替换 node index.js。

## 测试应用程序

现在，一旦你完成了上面提到的步骤，你就可以试着运行你的应用程序了。在这里，我给出了运行开发模式的应用程序的例子

```
npm run development
```

## 结论

在这篇博客中，我们看到了如何根据环境在应用程序中集成不同的 env 文件。如果你知道任何更好的方法，请随意评论。如果你喜欢这篇文章，请分享。感谢阅读。快乐编码。

你喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCvEB7wXUEXGFE9lCx0USR3Q) **！**

同样的博客也将在[ajaykrp.me/](https://ajaykrp.me/)推出。