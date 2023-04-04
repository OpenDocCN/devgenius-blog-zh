# NodeJS 网关—第 1 部分:初始设置

> 原文：<https://blog.devgenius.io/building-a-payment-gateway-using-javascript-nodejs-a98e51f0ea6d?source=collection_archive---------4----------------------->

![](img/3a2ae43c1fff5c72a7b824c55e523fbb.png)

由[费伦茨·阿尔马西](https://unsplash.com/@flowforfrank?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

距离我上一篇关于使用 Javascript 构建支付解决方案的文章已经过去 3 个多月了。

[](https://medium.com/@nkunzecaleb/how-i-build-a-payment-gateway-using-javascript-2e4f3e52b7cd) [## 我如何使用 Javascript 构建支付网关。

### 在我的大学/学院学习之后(大约 5 年前)，我渴望建立一个项目，不仅是为了创收…

medium.com](https://medium.com/@nkunzecaleb/how-i-build-a-payment-gateway-using-javascript-2e4f3e52b7cd) 

是的，对于一个朝九晚五的人来说，这有时会让人不知所措，我很高兴能够最终完成如何使用 Javascript 构建支付网关或解决方案的未来系列的一部分。

在上一篇文章中，我谈到了我为什么选择 Javascript，我在这里不再重复，但是，我已经意识到使用其他框架(不是 JavaScript)或语言也可以给你一个非常好的和健壮的解决方案。因此，我将在未来分享如何使用其他语言(PHP 和 Python)和框架(Laravel 和 Django)来提出类似甚至更好的解决方案。这完全取决于你熟悉哪一个。

好了，现在让我们深入网关(JavaScript — NodeJS)。

# **开始**

这样，我们将使用 TypeScript，我认为这比只使用 JavaScript 更有优势，可以避免可能已经渗透到运行时的错误。你看，基本 JavaScript 是动态的和弱类型的，这可能会给管理不断增长的代码库带来挑战，而 TypeScript 是 JavaScript 的超集，具有强大的静态类型、编译和面向对象的编程特性，这在查看大型代码项目或不断增长的代码库(如支付网关)时很方便(从上一篇博客中，你会看到我们正在考虑移动货币集成、VISA 和 MasterCard、SMS、电子邮件和其他计费和安全功能，这些功能往往会不断变化)

既然我们已经解决了这个问题，让我们从用 TypeScript、Express 和 MongoDB 设置网关开始。

## 第一步

我们将创建我们的文件夹并移入其中:

```
$ mkdir gateway
$ cd gateway
```

然后，我们可以开始初始化我们的 npm 项目:

```
$ npm init
```

您可以使用`-y`标志，通过自动说 yes 来使用默认的`npm init`。然而，对我来说，我喜欢把它个人化😎所以我想出了这样的东西:

## 步骤 2 -添加和配置 TypeScript

初始化我们的项目后，接下来是添加和配置 TypeScript。请记住，这是在我们的网关文件夹中完成的。

首先，我们安装如下的字体:

```
$ npm install --save-dev typescript
```

然后，我们向根目录`gateway`添加一个`tsconfig.json`文件，用于配置 TypeScript 的编译选项。

```
$ nano tsconfig.json
```

然后插入下面的代码:

查看上面的片段，下面是对我们使用的变量的简单解释:

*   `module`:指定模块代码生成方式。节点使用`commonjs`。
*   `esModuleInterop`:在导入过程中启用 CommonJS 和 es 模块之间的互操作性。
*   `target`:指定输出语言级别(我们用的是 ES6)。
*   `moduleResolution`:这有助于编译器理解导入指的是什么。值`node`模拟节点模块解析机制。
*   `sourceMap`:分别创建。允许调试器和其他工具在处理发出的 JavaScript 文件时显示原始 TypeScript 源代码的映射文件。
*   `outDir`:编译后输出`.js`文件的位置。在我们的项目中，我们将使用`dist`文件夹。

在这里阅读更多相关信息 [TypeScript 编译器选项](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

## 步骤 3 -添加和配置 Express 服务器

让我们从安装 express 开始:

```
$ npm install --save express
$ npm install -save-dev @types/express//We use the @types/express package for TypeScript to know about the types of Express classes.
```

然后我们在我们的`gateway`文件夹中创建`src`文件夹来保存扩展名为`.ts`的类型脚本文件:

```
$ mkdir src
```

然后，我们添加一个名为`app.ts`的文件，它将作为我们的主项目应用程序，我们稍后将在`package.json`中添加相应的传输`app.js`文件，以作为主文件运行，而不是作为`index.js`:

```
$ nano src/app.ts
```

添加下面的代码来创建一个极简的 express 服务器:

上面的代码创建了一个运行在端口`3000`上的 express 服务器。让我们继续编译，然后运行我们的服务器来测试是否一切正常。为了进行编译，我们在终端中运行以下命令:

```
$ npx tsc
```

以上使用`tsconfig.json`中的配置选项在`dist`文件夹中生成`.js`文件，我们将使用这些文件运行应用程序，如下所示:

```
$ node dist/app.js
```

为了成功运行，我们将得到以下输出:

```
Express is listening at [http://localhost:3000](http://localhost:3000)
```

您可以在浏览器中打开该链接，并且应该能够在浏览器中看到`Hello World!`。Hurayyyy！！您已经设置并配置了 express 服务器。可以打开`dist/app.js`看看是什么样子。下面是一个片段。

最后，让我们安装和配置 MongoDB。

## 步骤 4 -添加和配置 MongoDB

首先，你需要确保你的机器上已经安装并运行了 MongoDB，你可以根据你的操作系统使用下面的链接来安装、运行和创建一个数据库(在我们的例子中应该是`gateway`)。

*   **在 MacOS 上**
    [https://www . MongoDB . com/docs/manual/tutorial/install-MongoDB-On-OS-x/](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-os-x/)
*   **在 Linux 上**[https://www . MongoDB . com/docs/manual/administration/install-On-Linux/](https://www.mongodb.com/docs/manual/administration/install-on-linux/)
*   **在 Windows 上** [https://www . MongoDB . com/docs/manual/tutorial/install-MongoDB-On-Windows/](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-windows/)

我们可以使用下面的命令将`mongodb`和`mongoose`添加到我们的项目中:

```
$ npm install mongodb mongoose
$ npm install -save-dev [@types/mongodb](http://twitter.com/types/mongodb) @types/mongoose
```

接下来，我们将连接到我们通过更新`app.ts`代码创建的数据库，如下所示:

我们已经导入了 mongoose 模块，并使用它连接到使用 url `mongodb://localhost:27017/gateway`的数据库。在成功的`db`连接上，express 服务器将被启动，您将得到以下输出:

```
Express is listening at [http://localhost:3000](http://localhost:3000)
```

哇！这是一个很长的线程，但我很感谢你花时间来通过它。如果您能为这个系列和其他主题的未来帖子鼓掌，甚至跟随我，那将是非常棒的。接下来，我们将看看数据库操作。

如果你想在这篇文章中使用代码，你可以使用下面的链接:

[](https://github.com/Cank256/payie/) [## GitHub - Cank256/payie:支付解决方案项目

### 支付解决方案项目。在 GitHub 上创建一个帐户，为 Cank256/payie 开发做贡献。

github.com](https://github.com/Cank256/payie/)