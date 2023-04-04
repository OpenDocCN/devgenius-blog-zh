# 使用 Webpack 从头开始创建 React 应用程序

> 原文：<https://blog.devgenius.io/create-react-app-from-scratch-using-webpack-fa6e41f15289?source=collection_archive---------2----------------------->

在这篇博客中，我们将学习如何使用 webpack 和 babel 等工具从头开始创建 react 应用程序。

首先，我们需要初始化我们的项目，以便我们可以使用 npm 或 yarn 安装包。在当前目录下运行以下命令。

这将使用默认配置创建一个 package.json 文件。

我们将开始安装依赖项。在当前目录下运行以下命令。

上面的命令将在我们的项目中安装 react 和 react-dom，我们将在博客的后面部分使用它们。

我们使用 webpack 来捆绑我们的项目。Webpack 将把我们的代码编译成一个单独的包，可以在 web 上提供之前注入到我们的模板 html 中。运行以下命令安装 webpack 相关依赖项。

所有与 webpack 相关的依赖项都将是开发依赖项，我们不会在生产中使用。一旦构建完成，我们不再需要 webpack。我们需要 webpack-cli 来运行 webpack 命令行界面，需要 webpack-dev-server 来运行本地服务器，需要 html-webpack-plugin 来创建一个 html 模板来服务我们的包。

我们需要来自 babel 的相当多的依赖，babel 用于将我们的现代 javascript 代码转换成浏览器友好的格式。如果不使用 babel，我们就无法在浏览器中运行我们的 ES6 javascript。这也使得我们的代码向后兼容旧版本的浏览器。运行以下命令安装所有与 babel 相关的依赖项。

因为我们正在处理与 babel 相关的依赖项，所以我们将创建一个名为。将下面的代码放在根目录下。

上面的配置将让 babel 知道我们将使用 react 来设置我们的预置。我们完成了巴别塔的设置。

现在，我们将安装一些依赖项来帮助我们处理 css 文件。我们需要安装样式加载器和 css 加载器。这是 webpack 处理 css 和样式文件所需要的。这些也将成为开发依赖。

现在我们已经完成了所有软件包的安装，可以开始了。耶！！！恭喜你！

我们现在将继续设置我们的 webpack 配置。在根目录下创建一个名为 webpack.config.js 的文件。将以下代码添加到该文件中。别担心，我会深入解释的。

如果您注意到我们在这里没有使用常规的 import 语句，而是使用了 commonjs 语法。我们使用 require 关键字而不是 import。我们从节点的路径模块导入路径，从 html-webpack-plugin 导入 HtmlWebpackPlugin，稍后我们将在博客中使用它来创建一个 html 模板以服务于我们的项目。

我们将把所有的配置放在 module.exports 对象中。我们的 webpack 配置中的前两个属性是入口和输出属性。属性条目告诉 webpack 开始捆绑过程时的起始点，输出属性告诉 webpack 将编译后的捆绑包放在用户选择的目录中，并使用应用的名称。我们告诉 webpack 选择 index.js 文件。在输出中，我们要求 webpack 将完成的包放在 dist 文件夹中，在文件名中使用 *contenthash* 是为了在开发阶段提高构建时间的性能，因为我们不需要在未更改的文件上运行构建。即使是几分之一毫秒，对编程也有很大帮助。

第 11 行的属性 devtool 被赋给了 source-map，这是为了使调试更容易。这样我们可以在浏览器中看到我们的源代码。现在，我们正在编写规则，这是一个对象数组。在这里，我们可以指定 webpack 在处理项目中的各种文件时需要遵循的一组规则。我们使用 *test* 属性来查找文件， *exclude* 属性来排除我们不希望 webpack 编译的某些文件或目录。我们使用*使用*关键字来告诉 webpack 使用哪个加载器。我们使用 *babel-loader* 来加载 javascript 文件。为了加载 css 和样式文件，我们使用了*样式加载器*和 *css 加载器*。这就是我们如何维护各种加载器来处理 webpack 中的各种文件类型。如果您有兴趣，请访问:[加载器](https://webpack.js.org/concepts/loaders/)以获取更多加载器来处理您的用例。

我们将在第 27 行添加一个插件，这个插件将在我们的应用程序发布到网络之前生成一个 html。如果我们不想提供模板 html，我们可以将选项留空，但在这种情况下，我们将编写自己的 html 模板，并将在 src/index.html 下创建它。

这只是一个样板 html。这里没什么花哨的:)

在 src 目录下创建 index.js 文件。我们现在将在这个文件上配置 React 和 ReactDOM。将下面的代码放在那里。

在这里，我们使用 root 的 id 来抓取我们添加的 div，并使用 render 方法在 UI 中进行渲染。我们正在从 App.js 文件导入应用程序，我们将很快创建包括 css 文件。将下面的代码分别放在 App.js 和 App.css 中。

我们正在创建简单的 jsx 和 css 来在 UI 中显示一些东西。现在，这已经完成了，剩下的唯一任务是在 package.json 文件中创建一个构建和启动脚本，以便我们可以运行构建和启动命令来运行我们的应用程序。将以下脚本放在 package.json 中的脚本对象下。

我们正在使用 webpack-dev-server 为*启动*脚本打开服务器。我们将模式设置为*开发*，— hot flag 帮助 webpack 在您对应用程序进行任何更改后立即加载应用程序，而— open 让 webpack 在您运行启动脚本时在浏览器中打开标签。对于*构建*脚本，我们只是将模式设置为生产，这将在 dist 目录中创建一个编译好的包，准备在 web 中提供。

现在，当您运行启动脚本时，您应该看到您的 React 应用程序在端口 8080 上运行。恭喜您，您刚刚从零开始构建了 React 应用程序，而不需要 create-react-app 脚本。

设置一次，放在 github。对于每一个其他项目下载的项目改变几个名字，你是好的去。现在，你根本不需要创建-反应-应用程序。

如果你喜欢这个博客，请随时关注我，了解更多类似的内容。

源代码可在此处获得[。如果您喜欢 github 上的内容，请随意开始。](https://github.com/sudiparyal185/react-application-with-webpack)

我在 youtube 上有一个同样主题的深度视频。随便给它一只表。

非常感谢！