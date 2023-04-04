# 扩展角度 CLI

> 原文：<https://blog.devgenius.io/extending-angular-cli-f8b1abeb612a?source=collection_archive---------8----------------------->

在 enterprise Angular 中，工具通常不仅仅是 Angular CLI。许多公司运行代码生成器、模拟服务器和定制的构建管道。

通常是`package.json`脚本将这个工具添加到 Angular CLI 之上。但是`package.json`脚本遗漏了 Angular CLI 必须提供的许多东西。

*   他们对可观察的建筑商感到尴尬，比如对`ng serve`。
*   他们看不懂`BuilderOptions`。
*   他们无法对棱角分明的 CLI 建设者的`BuilderOutput`做出反应。
*   它们不是 Angular CLI `--help`文档的一部分。

解决方案很简单，停止依赖`package.json`脚本，转而扩展 Angular CLI。这听起来工作量很大，但是在`@angular-devkit/build-angular`中添加了一些新的东西，实际上非常简单。

![](img/262b2b9e16bf55bae0cacb8227c66604.png)

# Angular CLI 的 node.js API

谷歌一下，你只会得到少得可怜的 19 次点击。对于一个极其有用的 Angular API 来说，这实在是太低了。

从名字就能猜到，`executeDevServerBuilder`只是`ng serve`的 node.js API。事实上，大多数 Angular CLI 命令都有一个 API。

*   `executeBrowserBuilder`是`ng build`
*   `executeDevServerBuilder`是`ng serve`
*   `executeNgPackagrBuilder`是图书馆里的`ng build`
*   `executeExtractI18nBuilder`是`ng i18n`
*   `executeKarmaBuilder`是`ng test`
*   `executeProtractorBuilder`是`ng e2e`

这为什么有用？这些 API 使得 Angular CLI 易于扩展！你所要做的就是创建一个 [Angular CLI 构建器](https://angular.io/guide/cli-builder)，然后交给一个本地构建器 API。

在这里，我们只是打印`Hello world!`，但可能性是无穷的。

*   你可以在`ng serve`上启动一个模拟服务器。
*   您可以在运行`ng build`之前运行代码生成器。

# 向现有构建器添加新选项

扩展任何 CLI 的很大一部分都是为了添加新的 CLI 选项。如果`ng serve`要启动一个模拟服务器，用户可能想要一个`--no-mock-server`选项。

上例中`extendedServeBuilder`的参数包含了`executeDevServerBuilder`的所有选项。但是这些选项从何而来？它们是每个 Angular CLI 生成器声明的 JsonSchema 的一部分。这意味着如果你想给`ng serve`添加新的选项，那么你必须将一个模式合并到 Angular CLI 模式中。

`postinstall`钩子是进行这种合并的完美地方。这将确保生成的模式与 Angular CLI 的本地版本相匹配。

# 使用扩展的构建器库

实现了扩展构建器并列出其选项后，是时候重新路由命令了。Angular CLI 使用`angular.json`来查找在什么命令上调用什么构建器。要让`ng serve`调用新的扩展构建器，只需更新`angular.json`中的 serve `builder`属性。

```
{ "builder": "@angular-devkit/build-angular:dev-server" }
```

成为

```
{ "builder": "your-builder-library:your-builder-name" }
```

# 把所有的放在一起

*   创建一个使用 node.js API 和任何扩展的新构建器函数。
*   在`postinstall`钩子中生成模式。
*   用`builder.json`创建一个构建器库，导出构建器实现及其模式。
*   更新 angular.json 以使用新的构建器库。

# 将所有内容放在一起(惰性版本)

我创建了一个库，它包装了所有本机 Angular CLI 生成器，并将它们的模式与您的模式合并。你要做的就是实现钩子。

[https://hooks.albinberglund.com](https://hooks.albinberglund.com)
[https://github . com/blid blid/Berg Lund/tree/main/projects/angular-CLI-hooks](https://github.com/blidblid/berglund/tree/main/projects/angular-cli-hooks)
[https://www.npmjs.com/package/@berglund/angular-cli-hooks](https://www.npmjs.com/package/@berglund/angular-cli-hooks)

*最初发布于 https://*albinberglund.com/extending-angular-cli