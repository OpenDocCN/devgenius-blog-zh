# 给你的自述增加一些魔力

> 原文：<https://blog.devgenius.io/add-some-magic-to-your-readme-d7f9a4d8622c?source=collection_archive---------15----------------------->

使用降价魔术的快速教程

![](img/c211ba57ee497e670fd4512a26b8ccdb.png)

神奇 wohoo！

你的项目的自述文件是否显得枯燥无味？今天我想介绍一下将名为 [Markdown Magic](https://github.com/DavidWells/markdown-magic) 的 NPM 软件包添加到您的项目中的步骤。但首先什么是降价魔术？

**降价魔法:**

*   用本地或远程代码文件保持 markdown 文件最新。
*   用自定义函数转换降价内容。
*   使用任何模板引擎呈现降价内容。
*   自动生成目录和更多！

当我把我所有的 [Codewars](https://www.codewars.com/) 解决方案编译成一个 Github 库的时候，我发现了 Markdown 的魔力。为了使我的知识库更有吸引力和更有用，我决定最好加强自述文件。如果我能把所有的解决方案都添加到一个组织良好的自述文件中，那就太好了。我的第一个想法是将每个代码片段复制并粘贴到各自的位置，但意识到如果我重构代码，这将是非常低效的。我将不得不返回并手动更新每个解决方案。

在花了时间思考处理这个问题的好方法后，我把我的问题发布在了丹佛发展频道上。很快，一位更资深的开发人员帮我找到了 Markdown Magic。我很快就相信这是解决我问题的完美而神奇的方法！因此，像我的其他教程一样，让我带你通过设置 Markdown 魔术的步骤。

## 设置:降价魔术

**1。用 NPM 初始化你的项目。这涉及到在项目文件的终端中运行以下命令:**

```
npm install markdown-magic --save-dev
```

**2。**现在您已经有了 package.json 文件，将以下代码添加到 scripts 对象中:

```
"docs": "md-magic --path '**/*.md' --ignore 'node_modules'"
```

这将允许您从终端运行 Markdown Magic 来检查 Markdown 内容中的任何内容是否需要根据代码文件中的更改进行更新。

**3。**现在你需要在 README 中添加一些标记，告诉 Markdown Magic 在哪里插入你的代码片段。它还会告诉 Markdown Magic 要监视哪个代码文件的变化:

```
<!-- AUTO-GENERATED-CONTENT:START (CODE:src=./relative/path/to/code.js) -->
This content will be dynamically replaced with code from the file
<!-- AUTO-GENERATED-CONTENT:END -->
```

这里需要指出的一点是，您需要将 src 更改为您想要监视更改的代码文件的适当路径。

除了检查本地代码文件，您还可以检查远程代码文件。这难道不神奇吗？！使用以下代码跟踪远程代码文件:

```
<!-- AUTO-GENERATED-CONTENT:START (REMOTE:url=http://url-to-raw-md-file.md) -->
This content will be dynamically replace from the remote url
<!-- AUTO-GENERATED-CONTENT:END -->
```

第三步的最后一件事是，你可以使用降价魔术生成目录。它将解析 markdown 内容，寻找标题。为此，请使用以下代码:

```
<!-- AUTO-GENERATED-CONTENT:START (TOC:collapse=true&collapseText=Click to Expand dude!) -->
toc will be generated here
<!-- AUTO-GENERATED-CONTENT:END -->
```

请注意，TOC 有一些可选的参数，这些参数使它可以与文本“单击以展开 dude！”一起折叠。如果删除，目录将是一个静态列表。请参阅 Markdown Magic docs 了解更多定制和功能！

暂时就这样吧！如果你想看我的成品，请查看我的 [Codewars 库](https://github.com/mrshappy0/codewars)。