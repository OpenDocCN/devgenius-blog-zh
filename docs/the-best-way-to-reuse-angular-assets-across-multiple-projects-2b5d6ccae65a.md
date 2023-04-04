# 跨多个项目重用角度资产的最佳方式

> 原文：<https://blog.devgenius.io/the-best-way-to-reuse-angular-assets-across-multiple-projects-2b5d6ccae65a?source=collection_archive---------9----------------------->

![](img/f2f140e15af095b11f64e6ced3ae97c9.png)

保罗·埃施-洛朗在 [Unsplash](https://unsplash.com/s/photos/npm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

作为一名从事多个项目的开发人员，有时您希望在它们之间使用相同的资产。样式表、图像、字体等。通常保存在 Angular 项目的*资产*文件夹中。如果您计划多次使用它们，那么多次复制它们会很麻烦。

对您来说，更好的方法是为所有可重用资产创建一个 **npm** 包。

出于多种原因，创建一个 npm 包来托管您的资产是非常有益的:

1.  **你所有的项目整合你的资产**很容易。你所要做的就是运行**NPM install<package name>**，所有这些都会集成到你的项目中。
2.  **添加新资产将更新所有项目**。如果你必须添加 CSS 类或改变不同标题的字体类型，改变它将允许所有项目在更新**NPM**时接收这个更新。四
3.  **该包可以作为其他包的依赖项**。假设将来你发布一个包，其中有一组角度元件，可以在你的所有应用中使用。您可以单独创建这个包，并使这个包依赖于您的资产包，这将消除您对您的应用程序缺少样式的担心。

## 如何创建 NPM 包

创建一个 npm 包非常容易！所有的 npm 包实际上都由一个 *package.json* 文件组成，该文件包含所有需要被识别为包的元数据。

若要创建一个文件夹，请创建一个新文件夹，您希望在其中托管您的项目。导航到该文件夹并运行 *npm init* 。这将询问您一系列问题，这些问题将用于填充 *package.json* 文件。

一旦完成，您的 *package.json* 文件将类似于以下内容:

```
{ "name": "<package name>", "version": "1.0.0", "description": "<description>", "main": "index.js", "scripts": { "test": "echo \"Error: no test specified\" && exit 1"
    }, "author": "<author>", "license": "ISC"}
```

## 向 npm 软件包添加文件

接下来，您需要一种将文件添加到包中的方法。这部分也一样简单。

将您所有的资产以结构化的格式放在一个文件夹中。分离样式表文件、字体、图像等。将此文件夹复制到您的包文件夹中。

打开 *package.json* 文件，添加以下字段:

```
"files": [
    "/<folder name>"
]
```

这个值的作用是，当它作为一个依赖项安装时，它会添加所有要包含在包中的文件。当有人安装您的软件包时，将文件夹名称放在这里将包括文件夹内的所有内容。

就是这个！现在，您的包已经设置好，可以安装了。您可以导航到 Angular 项目并通过运行以下命令进行安装:

```
npm install C:/path/to/package/folder
```

## 将样式添加到角度项目

这只是将包安装到您的项目中，但是您的项目实际上是如何识别样式的呢？嗯，你将不得不进口它们！别担心，这也是一个无缝的过程。

有两种方法可以将样式导入到项目中:

1.  **angular.json** :您可以在您的项目 angular.json 文件中添加样式的路径。打开它，找到名为*样式*的字段。默认情况下，你应该已经有了一个值( *styles.scss 或者 styles.css* )。要添加 CSS 文件，只需将它们的路径添加到此列表中。你的路径通常会是 *node_modules/ <包名> / <资产文件夹> / < CSS 文件>* 。
2.  **styles.scss** :或者，您可以将路径添加到您的 *styles.scss* 或 *styles.css* 文件本身。打开后用@import *node_modules/ <包名> / < assets 文件夹> / < CSS 文件>导入即可。*

## 发布您的包

托管软件包的最佳方式是通过在线存储库。发布您的软件包将使它可以通过 npm-registry 获得，这是一个公共存储库，托管了数千个软件包。

也有其他第三方包主机，包括 [GitHub 包](https://docs.github.com/en/packages/using-github-packages-with-your-projects-ecosystem/configuring-npm-for-use-with-github-packages)和 [Artifactory](https://jfrog.com/artifactory/) 。只要软件包可供下载，这两种方法都是有益的。你也可以把你的包托管在一个私有依赖上。

## 外卖食品

该系统非常适合在多个项目中使用相同风格的开发人员，或者希望在多个应用程序中使用其设计系统的承包商。这将有助于使您的开发周期更加容易。

希望这有所帮助！