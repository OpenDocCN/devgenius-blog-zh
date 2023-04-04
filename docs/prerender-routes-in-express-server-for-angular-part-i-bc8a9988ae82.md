# 用于 Angular 的 Express server 中的 Prerender 路由—第一部分

> 原文：<https://blog.devgenius.io/prerender-routes-in-express-server-for-angular-part-i-bc8a9988ae82?source=collection_archive---------8----------------------->

## 角度预渲染

有许多理由需要 prerender，也有许多理由不需要。每个应用程序都找到自己的用例来预渲染，而静态站点生成则完全是预渲染，像 stackoverflow 这样的大型网站避免了这一点。Angular 最近添加了用于预渲染的通用生成器，但在这个系列中，我想研究其他方法。

在进行我们自己的预渲染时，我想检查两种方法。

1.  好的 ol' Express 服务器
2.  Angular prerender builder 的分拆

在服务器上也有使用 Angular `RenderModule`的，但这是多余的。

今天我们将构建我们的 **Express server** 来预呈现页面，下周我们将讨论创建一个[单构建多语言 Angular app](https://garage.sekrab.com/posts/alternative-way-to-localize-in-angular) 的不同方面，这是我们之前提到的。

## 关于 StackBlitz 项目的几点注记

> 我创建了一个简单的 [StackBlitz 项目](https://stackblitz.com/edit/angular-express-prerender)来构建一个 SSR 应用程序，但不幸的是它在客户端创建`index.html`时失败了。如果你运行`npm run build:ssr`，它可能会卡在`index.html`上。取消该步骤，继续，并自己修补索引。我确实在 StackBlitz 中修补了这个文件，但是这意味着为根生成一个预先呈现的索引文件不会产生正确的结果。管他呢，斯塔克布里兹！
> 
> 这个项目展示了一个简单的 Express 服务器，我们[在之前](https://garage.sekrab.com/posts/loading-external-configurations-in-angular-universal)隔离服务器的时候谈到过。
> 
> 提供的节点版本不支持`fetch`，这就是为什么我们使用`node-fetch`库，而不是`commonjs`，所以解决方案(根据文档)是这样导入的:

```
const fetch = (...args) => import('node-fetch')
	.then(({ default: fetch }) => fetch(...args));
```

# 运行本地 Express 服务器

最简单直接的方法是建立一个本地 Express 服务器，并在 Node 中使用一个简单的`fetch`。`fetch`从节点版本 17 开始可用，在那之前，你可以使用`node-fetch`库。

当前设置如下:

*   `src`文件夹中有角度模块，包括`app.server.module`
*   Building 在`host/client`下创建客户端文件，在`host/server/main.js`下创建 SSR
*   `host/server.js`有一个独立的 Express 服务器，在本地端口上运行 Angular。
*   `host/server/routes.js`有进口角`ngExpressEngine`出口角`app.server.module`的航线
*   我们的新获取文件在`host/prerender/fetch.js`下

因此，首先让我们创建预渲染获取模块:

```
// host/prerender/fetch.js file
async function renderToHtml(route, port) {
	// run url in localhost
  // do something with returned text
	// return
}
// export some function here:
module.exports = async (port) => {
  // generate /client/static/route/index.html
  // my static routes, example routes
  const routes = ['', 'projects', 'projects/1']; for (const route of routes) {
    await renderToHtml(route, port);
  }
};
```

如果设置了环境变量，我们调整我们的服务器来做一些事情，如下所示:

```
// host/server.js
// ...
// just when you start listening:const port = process.env.PORT || 1212;// assign a server to be able to close later
// turn function to async to allow an await statement
const server = app.listen(port, async function (err) {
  console.log('started to listen to port: ' + port);
  if (err) {
    console.log(err);
    return;
  }
  // if process.env.PRERENDER, then run this and close
    if (process.env.PRERENDER) {
      const prerender = require('./prerender/fetch');
			// await fetch before you close here
			// pass the port to reuse it
			await prerender(port);
			server.close();
    }
});
```

为了在 prerender 模式下运行，我们在**主机**文件夹的根目录下创建一个快速`npm`脚本。

`"prerender": "SET PRERENDER=true && node server"`

或者在 Windows 之外的其他地方(比如 StackBlitz，在根包中找到脚本，首先是`cd host`)

`"prerender": "PRERENDER=true node server”`

功能`renderToHtml`，应该执行以下操作:

*   在 SSR 环境中提取路线
*   将输出字符串保存到一个`index.html`文件中
*   将文件放在与路径匹配的路径中
*   把它保存在一个容易找到的地方，不仅仅是为了快速，也是为了火焰，网络和浪涌。所以目的地应该在`client`文件夹里面(云主机的公共文件夹)。

为此，我们将使用节点的`fs/promises`包，这允许我们在完成后关闭端口。我现在选择将它们放在`/client/static`文件夹中。在 Express 中，这很容易管理。在其他主机中，比如 Netlify，将它们放在 root `/client`上会更容易。

```
// host/prerender/fetch.js
const fs = require('fs/promises');// this should be part of a config passed down from server listener
// client/static for Express, or simply client for cloud hosts
const prerenderOut = './client/static/';async function renderToHtml(route, port, outFolder) {
	// fetch it
  const response = await fetch(`http://localhost:${port}/${route}`);
  if (response.ok) {
    const text = await response.text();
		// the output folder is ./client/static/{route}, relative to root server file
    const d = outFolder + route;
		// mkdir recursive, creates the folder structure
    await fs.mkdir(d, {recursive: true});
		// create index.html, and write text to it.
    await fs.writeFile(d + '/index.html', text);
		// loggin success
    console.log('ok', route, text.length);
  } else {
		// log errores
    console.log('not ok', route, response.status);
  }}module.exports = async (port) => {
	// generate /client/static/route/index.html
  // my static routes, example routes (you could run an API call to get all paths)
  const routes = ['', 'projects', 'projects/1']; for (const route of routes) {
    await renderToHtml(route, port, prerenderOut);
  }
}
```

需要改进的一点是，在我们开始创建静态文件夹之前删除它，以确保我们每次构建它时都有一个新的副本。在云主机中，情况会有所不同。

```
// prerender/fetch.js
module.exports = async (port) => {
	// ...
  // remove static folder first
  await fs.rm(prerenderOut, {recursive: true, force: true});
	// ...
}
```

运行脚本，观察文件的创建过程。注意:Angular universal packages 的酷之处在于，它还会为每个路径创建内嵌的关键 CSS。

# 快速路线

为了在 Express 中提供这些静态文件，创建了一个新的静态适配器，将`/client/static`的内容暴露给根，因此在`routes.js`文件中(包含到 Express 中服务器 Angular SSR 的必要路径):

```
// host/server/routes.js
// this should be part of a config file passed down from server listener
const rootPath = path.normalize(__dirname + '/../');module.exports = function (app) {
	// expose static folder
  app.use('/', express.static(rootPath + 'client/static'));
	// ... other routes
}
```

为了测试这一点，首先我们进入主机文件夹，并运行`node server`。然后浏览到`localhost:1212`，为了区分静态文件和角度服务文件，我们执行以下操作:

*   将静态文件的标题改为我们能够识别的名称，比如“静态…”
*   在浏览中禁用 JavaScript

现在，如果我们看到标题改变了，那么静态页面就被提供了。当然，JavaScript 在启用时会水化，Angular 客户端会取而代之。

# 单一多语言版本

在**扭曲角度定位**的罪恶中进一步沉沦，让我们在相同的构建中为不同的语言创建静态文件，为此，休息一下，下周再回来。😴

感谢你阅读这篇简短的介绍，如果你开始看到替身，请告诉我。

# 资源

*   [角度—预渲染](https://angular.io/guide/prerendering)
*   [StackBlitz 项目](https://stackblitz.com/edit/angular-express-prerender)
*   [Express API](https://expressjs.com/en/4x/api.html)
*   [NodeJs 文件系统](https://nodejs.org/api/fs.html)
*   [MDN 获取](https://developer.mozilla.org/en-US/docs/Web/API/fetch)
*   [节点取库](https://www.npmjs.com/package/node-fetch)

# 相关职位

*   [角度定位的替代方式](https://garage.sekrab.com/posts/alternative-way-to-localize-in-angular)
*   [在角度通用中加载外部配置](https://garage.sekrab.com/posts/loading-external-configurations-in-angular-universal)

[](https://garage.sekrab.com/posts/prerender-routes-in-express-server-for-angular-part-i) [## Angular - Part I，Angular - Sekrab 车库的 Express 服务器中的预呈现程序路线

### 在进行我们自己的预渲染时，我想检查两种方法。好的 ol' Express 服务器附带的 Angular…

garage.sekrab.com](https://garage.sekrab.com/posts/prerender-routes-in-express-server-for-angular-part-i)