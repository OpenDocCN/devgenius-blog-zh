# 从头开始创建一个电子应用程序

> 原文：<https://blog.devgenius.io/create-an-electron-app-from-scratch-3b7e5b63d00f?source=collection_archive---------11----------------------->

如果你试图创建一个基于多操作系统的应用程序，如果你对使用 JavaScript 有信心，你可以使用“电子”。在这里，我将让您了解创建一个样板文件的基本步骤。

仅供参考——electronic 使用网页作为它的 GUI，所以你也可以把它看作一个最小的 Chromium 浏览器，由 JavaScript 控制。

1.  打开您最喜欢的代码编辑器工具。这里我用的是微软 VS 代码。
2.  为您的存储库创建一个空文件夹。
3.  在终端上，您应该从— **npm init** 开始。写下关于你的项目的信息，当它询问“这样可以吗?”时输入“是”。(是)”

> 我假设您的系统上安装了 npm 和 node。如果没有，您应该在您的系统上安装这些软件包。

4.完成第 3 步后，您应该能够在文件夹中看到“package.json”文件。

5.安装“electron ”,这样您创建的就不仅仅是一个 node.js 应用程序了。要安装 electron，请运行以下命令。希望你知道 save-dev 是什么意思。:)

```
npm install — save-dev electron
```

6.好了，现在您必须在存储库中看到一个文件夹“node_modules”和“package-lock.json”。太好了！

7.我们赶紧把这个 node.js app 变成电子 app 吧。为此，打开 package.json 文件，在 scripts 标记中，您需要添加如下一行

```
“start”: “electron .”
```

不确定，如果你像我一样设置你的 package.json 文件，你应该能够在编辑之前看到类似这样的东西。现在忽略以下内容，只在下面添加您的行，或者您可以删除这个“测试”命令，用“开始”替代:“电子”

```
“scripts”: {
“test”: “echo \”Error: no test specified\” && exit 1"
},
```

*别忘了用那个点(。).检查一下！！*

8.在根文件夹上创建一个新文件“main.js”。这是应用程序的入口点。

是时候检查你的 package.json 了。你能看到文件中的 json 代码吗

```
“main” : “index.js”
```

如果看到以上内容，可以创建一个名为“index.js”的文件。或者，如果你已经用“main.js”创建了一个文件，那么就让应用程序知道“main.js”是你进入应用程序的入口。大概是这样的—

```
“main” : “main.js”
```

完美！现在，让我们写一些代码。但是这里有一些你需要了解的东西— *电子应用程序是用 JavaScript 开发的，使用了 Node.js 开发中相同的原则和方法。Electron 中的所有 API 和特性都可以通过****Electron****模块访问，该模块可以像任何其他 Node.js 模块一样需要:*

```
const electron = require(‘electron’)
```

*电子模块在名称空间中公开特性。例如，通过****electron . app****管理应用的生命周期，可以使用* ***electron 创建窗口。BrowserWindow*** *类。一个简单的****main . js****文件可能会等待应用程序准备就绪并打开一个窗口:*

看起来你跳过了上面的讲座。好吧，让我们给你的书呆子编码吧！！！

我通常会使用一个“Hello World”类型的 HTML 页面来查看应用程序是否真的工作。

9.让我们为根文件夹中的视图创建一个名为“**index.html**的 HTML 页面。

```
<!DOCTYPE html>
 <html>
 <head>
 <meta charset=”UTF-8">
 <title>Hello World!</title>
 </head>
 <body>
 <h1>Hello World!</h1>
 </body>
 </html>
```

全选，Shift+Alt+F，神奇！！

10.打开您的“main.js”文件并复制粘贴以下内容。你想详细了解每件事吗？我稍后会解释它，或者你可以在电子文档或几个艺术家的长 YouTube 视频中读到这些。(编码员就是艺术家，我是这么认为的。)

```
const {app, BrowserWindow} = require(‘electron’)
 const path = require(‘path’)
 const url = require(‘url’)

 function createWindow () {
 // Create the browser window.
 win = new BrowserWindow({width: 800, height: 600})

 // and load the index.html of the app.
 win.loadURL(url.format({
 pathname: path.join(__dirname, ‘index.html’),
 protocol: ‘file:’,
 slashes: true
 }))
 }

 app.on(‘ready’, createWindow)
```

全选，Shift+Alt+F，神奇！！

重述—

*   你有一个 node_modules 文件夹，package.json，package-lock.json，main.js/index.js, inde.html 文件。
*   您已经编辑了 package.json 脚本标签，使用了“start”:“electron”再次检查电子后的点。
*   就是这样。现在，让我们运行应用程序。

11.要运行您的应用程序，请在终端上点击以下命令

```
npm start
```

上面的“开始”指的是你的 package.json — script — start 命令。

答对了。！有一个你的应用程序，随时可以使用。

我将使用 Bootstrap 并更改视图文件或其他 js 控制器的文件夹结构..如果我不觉得懒，我也会为此写一篇文章。

干杯！！