# 如何制作一个 Chrome 扩展

> 原文：<https://blog.devgenius.io/how-to-make-a-chrome-extension-f37bdfb6edb3?source=collection_archive---------0----------------------->

![](img/2b308240fab0fc6b338db50db48462b8.png)

丹尼尔·罗梅罗在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果你有谷歌 chrome，那么你会知道 chrome 扩展，比如一个应用程序或软件，它有一些算法来执行你的浏览器中的特定任务，最广泛使用的 Chrome 扩展例子包括 LastPass 扩展，它以加密形式在线存储你的密码。下载量很大，因为许多人需要一个应用程序来以安全模式存储密码。如果你想学习如何开发你自己的 chrome 扩展，那么在这篇文章中，我们将学习如何开发一个 chrome 扩展。我们需要的语言是`JavaScript`和一个`manifest.json`文件。

# Manifest.json

这个`manifest.json`文件将告诉 google chrome 关于你的 chrome 扩展的信息，比如扩展的权限和名称，版本和链接代码`JavaScript.`让我们创建一个目录并创建一个扩展名为`json`的清单名称文件，然后将下面的代码放入文件中。

如果你已经看到了上面的代码，我添加了 manfiest _ version，它是 2，我们不能添加 1，因为它在 2014 年后不受支持，所以如果你添加版本 1 扩展将无法工作。接下来我们将在谷歌浏览器中加载这个扩展。

# 加载扩展:

按照步骤在你的谷歌浏览器中加载扩展。

第一步:在你的谷歌浏览器中打开`chrome://extensions/`

第二步:点击右上角的`Developer mode`

第三步:现在点击`Load unpacked extension`

步骤 4:选择扩展的目录

步骤 5:现在你看到你的扩展将出现在`chrome://extensions/`

当你在你的 chrome 扩展中改变一些东西时，只要回到这个页面并重新加载你的扩展，谷歌 chrome 就会更新你的扩展中所做的任何改变。

# 内容。Js:

内容脚本是在网页环境中运行的 JavaScript 文件，简单来说，内容脚本将与您在 google chrome 上访问的每个网页进行交互。内容脚本包含一段 JavaScript 代码，用于在 Chrome 浏览器中执行特定的任务。我在目录中创建了一个`content.js`文件，并在其中添加了下面的代码。

`Alert()` 是 JavaScript 中的内置函数，当你重新加载谷歌浏览器并访问任何网页时，你都会看到一个弹出框，上面写着`Hello from a Coder`。为了注入脚本，我们需要将这个`content.js`文件与`manifest.json`文件连接起来，这样谷歌浏览器就可以读取它了。

在第 7 行，我们用`content_scripts`来定义它，当 google chrome 读取这一行时，他们会知道他们需要根据 content_scripts 中的代码来执行操作。第 9 行显示我们使用了匹配模式并设置它`<all_urls>`这将在你访问的每个 URL 或网页上显示我们的弹出对话框，如果你想添加一些特定的网站，你的对话框应该在上面显示，然后使用第 12 行的`[“https://mail.google.com/*", “http://www.medium.com/*"].`我们告诉谷歌浏览器他们需要注入的内容脚本的名称`js:[“content.js]`。重新加载你的 Chrome 扩展。你现在访问的每一个页面都会弹出一个提示。让我们改为记录页面上的第一个 URL。

# JQuery:

jQuery 不是必需的，但是在 chrome 扩展中，在某些方面，比如登录 URL 等，jQuery 会很有用。首先，从 jQuery CDN 下载一个版本的 jQuery，放在您的扩展的文件夹中，并添加 jQuery 的缩小版本的名称。要加载它，在`content.js`检查下面的代码之前，将其添加到 manifest.json。

现在我们有了 jQuery，让我们用它在 content.js 中记录页面上第一个外部链接的 URL。注意，我们不需要使用 jQuery 来检查文档是否已经加载。默认情况下，Chrome 在 DOM 完成后注入内容脚本。

试试看，你会在你访问的每个网页上看到谷歌控制台的输出。

# 浏览器操作:

如果你已经注意到 chrome extension 有一些图标，当你点击那个图标时，他们会弹出一个消息框，给你一些选项，这些选项是他们的功能，叫做浏览器动作，你的扩展可以监听那个按钮上的点击，然后做一些事情。从 google chrome 下载一个示例图标，将 icon.png 放在带有`content.js`的目录中，并将以下代码放在 content.js 文件中。

第 16 行我添加了一个新的代码名 browser_action，它将告诉 google chrome 我们正在为扩展定义浏览器动作，在下一行我将`default_icon`设置为我想在扩展上显示的图标的名称。如果你重新加载你的 chrome 扩展，你会看到变化。现在，我们希望我们的扩展显示一个消息框，当我们点击它，这可以通过消息传递来完成。

# 消息传递:

内容脚本可以访问当前页面，但受限于它可以访问的 API。例如，它不能监听浏览器动作上的点击。我们需要为我们的扩展添加一个不同类型的脚本，一个后台脚本，它可以访问每个 Chrome API，但不能访问当前页面。创建一个新的文件名`background.js.` ,这样内容脚本将能够从当前页面中提取一个 URL，但需要将该 URL 交给后台脚本来做一些有用的事情。为了进行通信，我们将使用 Google 所谓的消息传递，它允许脚本发送和监听消息。它是内容脚本和后台脚本交互的唯一方式。

现在我们编辑 background.js 文件。

当用户点击浏览器动作时调用第 1 行代码函数，下一行代码函数将发送一个 JSON 负载到活动标签。现在我们将 background.js 代码与 content.js 代码连接起来。

请注意，我们之前的所有代码都被移到了侦听器中，因此它只在收到有效负载时运行。每当您单击浏览器操作图标时，您应该会看到一个 URL 被记录到控制台。如果不起作用，尝试重新加载扩展，然后重新加载页面。

所以在这篇文章中，我们学习了如何创建一个 chrome 扩展，我们还学习了创建 chrome 扩展需要什么，什么是 manifest，什么是内容脚本。如果你是一名 JavaScript 开发人员，那么你应该知道一些功能，你可以编写代码使你的 chrome 扩展更加有用。希望这篇文章对你以后有所帮助，也可以随意分享你的看法。