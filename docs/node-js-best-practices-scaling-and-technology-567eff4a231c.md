# Node.js 最佳实践—扩展和技术

> 原文：<https://blog.devgenius.io/node-js-best-practices-scaling-and-technology-567eff4a231c?source=collection_archive---------3----------------------->

![](img/2959dd3a56ad81dc42bc265c9c1993cc.png)

照片由 [i 云脉](https://unsplash.com/@yunmai?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写节点应用程序时应该遵循的一些最佳实践。

# 使用 npm 脚本

我们可以使用 NPM 脚本来放置构建、测试和启动应用程序的脚本。

这样，我们可以在一个命令中输入更少的内容来完成所有这些任务。

我们把我们的脚本放在`package.json`中是为了让我们的生活更容易。

所以我们可以把:

```
"scripts": {
  "preinstall": "node preinstall.js",
  "postintall": "node postintall.js",
  "build": "webpack",
  "postbuild": "node index.js",
  "postversion": "npm publish"
}
```

这样我们就可以用`npm run preinstall`、`npm run postinstall`等来运行它们。

如果我们需要运行多个命令，我们可以使用`&&`。

Webpack、Nodemon 等命令行工具。应该作为本地开发依赖项运行，以避免冲突。

# 使用环境变量

我们应该使我们的应用程序可配置，以便我们可以在任何环境中运行它们。

我们可以在不同的地方设置它们，包括启动应用程序的命令:

```
NODE_ENV=production MONGO_URL=mongo://localhost:27017 nodemon index.js
```

或者在 Nodemon 自己的配置中，也就是`nodemon.json`:

```
{
  "env": {
    "NODE_ENV": "production",
    "MONGO_URL": "mongo://localhost:27017/accounts"
  }
}
```

请记住，不要为我们的应用程序检查任何秘密。

# 事件循环

如果我们需要执行长时间运行的任务，那么我们需要在事件循环中对它们进行排队。

有各种方法可以做到这一点。

`setImmediate`和`setTimeout`都运行在下一个事件循环周期。

`nextTick`工作在同一个周期。

# 使用类继承

使用`extends`关键字，类语法使继承变得更容易。

它确保我们调用父构造函数，并让我们继承东西而不用直接使用原型。

尽管类语法是原型的语法糖，但它使我们的生活更容易。

# 恰当地命名事物

我们应该恰当地给事物命名，这样我们就不必向人们解释它们的含义。

所以与其写:

```
const foo = require('morgan')
// ...
app.use(foo('dev'))
```

我们写道:

```
const logger = require('morgan')
// ...
app.use(logger('dev'))
```

# 使用 JavaScript？

我们可以使用像 TypeScript 这样的 JavaScript 扩展来使我们的生活更加轻松。

他们经常让我们用各种 JavaScript 做不到的方式限制数据类型。

TypeScript 还提供了其他方便的特性，如接口和类型别名，以限制我们的对象的结构。

它也有类型守卫来推断类型。

TypeScript 编译器最早可以编译到 ES3 的 JavaScript 版本。

它应该满足任何人的需求。

# 快速中间件

Express 中间件让我们可以将 Express 应用模块化。

许多 Express 附加组件作为中间件提供。

它们包括用于解析请求体的 body-parser 等等。

路线也是中间件。

他们让我们轻松地构建我们的 Express 应用程序。

因此，我们应该知道如何创造和使用它们。

# 按比例增加

我们依赖于节点中的异步代码，这样我们运行的代码不会阻碍其他部分的运行。

它只有一个线程，所以在这段代码完成之前，我们不能运行其他任何东西。

要使用处理器的多个内核，我们必须创建一个集群。

PM2 是一个简单的进程管理器，让我们可以轻松地创建集群。

我们运行:

```
npm i -g pm2
```

来安装它。

然后我们运行:

```
pm2 start server.js -i 4
```

用 4 个实例运行`server.js`,每个实例在其自己的内核上运行。

还有针对 Dockerized 应用程序的`pm2-docker`。

我们可以把:

```
# ...

RUN npm install pm2 -g

CMD ["pm2-docker", "app.js"]
```

来运行它。

![](img/408196fe0ddfee0d8a53db26b2a584e5.png)

由[尼克·卡沃尼斯](https://unsplash.com/@nickkarvounis?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以与流程管理器一起扩展，一些方便的技巧会帮助我们更容易地开发。