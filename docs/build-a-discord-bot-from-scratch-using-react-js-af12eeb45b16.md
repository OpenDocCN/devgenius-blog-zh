# 使用 React.js 从头构建一个不和谐机器人

> 原文：<https://blog.devgenius.io/build-a-discord-bot-from-scratch-using-react-js-af12eeb45b16?source=collection_archive---------1----------------------->

*这里有一些关于在 React.js 中构建自定义 Discord bot 项目的注意事项*

![](img/4f9567130d3c16c2a82e3cce11d07736.png)

安妮·斯普拉特在 [Unsplash](https://unsplash.com/s/photos/team?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

从头开始构建一个不和谐机器人是一次有益的经历。这是测试一些代码并快速查看其运行情况的好地方。大多数 Discord 项目都是用 JavaScript 编写的，所以如果您使用 React.js，有很多起点。

如果你决定需要一个自己的机器人，你从哪里开始？首先，在 Discord 上创建一个应用程序。如果你做了一些调查(从来源开始)，有很多关于这个主题的信息。创建应用程序后，您需要连接到 Discord API，创建您的文件，并让您的机器人在线。之后，定制开始。让我们看看你会遇到的一些事情。

# Bot 命令

你几乎可以为机器人创建任何命令。这是你可能碰壁的地方。如果你不知道从哪里开始命名命令，让机器人响应这些命令，那就慢慢来。为什么将命令作为具有键值对的对象包含会有帮助？它允许你的机器人使用你的命令。当用户输入该命令时，应用程序知道在哪里执行该命令。它看起来很关键。

你有没有想过为什么很多 bot 命令前都要加一个感叹号？这是添加到机器人代码中命令的常规前缀。它增加了一层安全性，以确保发出命令的用户是一个“真实的人”。代码看起来是这样的:

```
const prefix = '!';
```

然后，当您在包含您的命令的文件末尾导出时，您将包含该对象:

```
module.exports = { botIntents, prefix, commands };
```

这一点，以及所包含的资源，让您了解机器人的工作点。现在是时候在机器人中集成您正在寻找的任何功能了。如果你计划使用一个需要你联系团队的 API，记住这需要时间。

# Bot 语法

当被问到你的代码是什么意思时，知道你在说什么是很好的。这几行代码会让你很好地理解 JavaScript 中包含`await`的`async`函数、`module.exports`和`export default`的区别，以及 package.json 文件是什么。

**异步功能**

```
// *App.js*const getLastMsgs = async (msg) => {
    const cheers = await msg.channel.messages.fetch({ limit: 10 }); const lastTenMsgs = cheers.map((message) => {
        return message.content;
    })
}
```

在 arrow 函数的开头看到 async 可能需要一秒钟的时间。那会过去的。为什么？很简单。我们告诉`getLastMsgs`函数等待，直到它收到消息。这条信息来自前面的代码行:

```
const reply = await getLastMsgs(msg);
```

你在等待你的职能得到承诺。这给了我们工作的动力。想象收到一封信。在你读这封信之前，你必须花时间停下来打开它。async 函数使我们的应用程序等待，直到有一封信返回给你打开。这个返回的信(或者 JavaScript 中的承诺)必须在函数运行之前被实现或拒绝。

**module.exports 与导出默认值**

```
*// default.js*const botIntents = [
    DIRECT_MESSAGES,
    GUILD_MESSAGES,
    GUILDS,
  ];
    const commands = {
    verify: 'verify',
    lastMsgs: 'last-messages',
  }; const prefix = '!'; module.exports = { botIntents, prefix, commands };
```

上面摘录的最后一行代码使`botIntents`对象、前缀变量和 commands 对象对应用程序的其余部分可用，允许您在其他文件中使用这些对象。这与你可能已经习惯的`export default`不同。

这和`export default`相比如何？

[这是一篇有助于理解这两者的文章](https://www.freecodecamp.org/news/node-module-exports-explained-with-javascript-export-function-examples/)。使用`module.exports`，你必须在需要使用这些导出的文件中找到你写这些导出的文件。使用`export default`，您可以将所需内容导入其他文件。`export default`允许我们在一个文件中导出整个类或函数，这不会像`module.exports`那样限制我们可以使用的函数。当您想要导出多个函数而不是整个类或函数时，最好使用`module.exports`。

**package.json 文件**

```
*// package.json*{
  "dependencies": {
    "discord.js": "^13.3.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.15"
  },
  "scripts": {
    "app": "nodemon app",
    "start": "node app"
  },
  "name": "*bot name*",
  "version": "1.0.0",
  "main": "App.js",
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": ""
}
```

package.json 是节点项目的核心。该文件包含项目运行所需的元数据。它包含项目功能属性的清晰定义，npm 使用这些属性来安装依赖项、运行您拥有的脚本以及标识包的入口点。它必须是实际的 JSON，而不是 JavaScript 对象。你可以在这里找到关于 package.json 文件[的更多信息。](https://docs.npmjs.com/cli/v8/configuring-npm/package-json)

**交互端点 URL**

你会在 [Discord 开发者门户](https://discord.com/developers/applications)看到这个。这里有一些东西，包括添加交互端点 URL 的选项。根据 Discord 的说法，当您添加交互端点 URL 时，“您可以选择配置一个交互端点，通过 HTTP POSTs 而不是通过网关与 bot 用户接收交互”。

如果你想让机器人体验最好的，这可能会让你好奇。什么是交互端点？它帮助你处理斜线命令。这将对**的进一步迭代**有用，但你现在不需要利用它。

# 后续步骤

从这里开始，您的项目非常简单。当然，你必须在旅途中学习，但是接下来的步骤很清楚。如果您能够挂钩到您正在寻找的 API，那么构建您需要的机器人应该是非常简单的。

你知道下一步该做什么吗？花时间阐明步骤。

# 离别问题

**一旦获得 API，机器人的发展方向是什么？**

设置验证文件来处理 API 和代码之间的交互。实现 API 调用，这将允许您访问 bot 需要的命令。用户将能够与机器人和你的 Discord 社区互动。

在此期间，你能做些什么？

为机器人完成时设置通道。渠道会创造体验。你的社区目前可能用户数量有限，所以不会有太多的互动。没有人想这么做，但是你已经建立了规则、FAQ、支持和公告渠道。或者，你可以决定不这样做。跟上项目发展的生物频道是个好主意。一切皆有可能。

吸取了什么教训？

你的将会不同。分享给大家。那里有大量的资源。你分配给一个项目的时间越多，就越容易找到你需要的东西。只是一个解决问题的游戏。更好地解决问题的唯一方法就是解决问题。

你将如何编写一个当用户输入命令时触发的函数！验证？

以一个好的音符结束。这就是你可能需要填补知识空白的地方。把它想象成你在解一个难题。知道你有多少块拼图，但是因为有太多松散的碎片，你找不到把你手中的那块放在哪里？这个决定就是这样的。试着想想复试。

# 将这一切结合在一起

这应该会让你的项目有一个好的开始。这些事情需要时间，有时候最好的办法就是不要编码。灵感会在你最意想不到的时候到来。现在，创建一个机器人的目标已经实现，接下来的步骤已经很清楚了。走开，花点时间**微笑**你创造的一切！

[从零开始建造一个现代的不和谐机器人](https://dev.to/elijahtrillionz/build-a-modern-discord-bot-from-scratch-learn-the-basics-973)

[JavaScript 中的异步函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

[用 JavaScript 导出语句](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export)