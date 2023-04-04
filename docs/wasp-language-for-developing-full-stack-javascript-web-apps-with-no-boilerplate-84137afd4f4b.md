# wasp——开发全栈 Javascript web 应用程序的语言，没有样板文件

> 原文：<https://blog.devgenius.io/wasp-language-for-developing-full-stack-javascript-web-apps-with-no-boilerplate-84137afd4f4b?source=collection_archive---------0----------------------->

![](img/2740d46e76802b638cdd1dd1c7ce207b.png)

认识一下 [Wasp](https://wasp-lang.dev/) :一种用更少代码开发全栈 web 应用的新编程语言。

Wasp 是一种简单的声明性语言，它使开发 web 应用程序变得容易，同时还允许您使用 React、Node.js 和 Prisma 等最新技术。

在这篇文章中，我们将与你分享为什么我们相信 Wasp 会成为 web 开发的一件大事，它是如何工作的，我们现在在哪里，以及未来的计划是什么！此外，**我们给我们的阿尔法测试员**一吨很酷的礼物。如果你想加入——一直读到最后。

# 为什么是 Wasp？

您知道如何使用 React，知道如何使用 HTML/CSS/…，知道如何在后端编写业务逻辑(例如，在 Node 中)，但是当您想要构建一个实际的 web 应用程序并部署它供其他人使用时，您会淹没在所有的细节和额外工作中—响应 UI、正确的错误处理、安全性、构建、部署、身份验证、管理客户端上的服务器状态、管理数据库、不同的环境等等。

![](img/43df790cf774bc81e9bf95c91a1a061c.png)

Jose Aguinaga 在他的博客[“2016 年学习 JavaScript 的感觉如何”](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f)中以一种有趣的方式描述了 web 应用开发意想不到的复杂性，这在 6 年后仍然有意义。

我们正在构建 Wasp，因为即使我们都是经验丰富的开发人员，并且已经使用各种技术开发了多个复杂的 web 应用程序(JQuery-> Backbone-> Angular-> React，own scripts/makefile-> Grunt-> Gulp-> web pack，PHP -> Java -> Node.js 等等)，我们仍然觉得构建 web 应用程序比它应该的要困难，因为在这个过程中涉及了许多样板文件和重复的工作。

我们的主要见解是，虽然技术不断快速发展，但应用的核心需求大多保持不变(身份验证、路由、数据模型 CRUD、ACL 等)。

这就是为什么大约两年前我们开始考虑将 web 应用规范(它应该做什么)从它的实现(它应该如何做)中分离出来。

这让我们想到了将常见的 web 应用功能和概念提取到一种特殊的规范语言(Wasp)中，而实现细节仍然通过现代堆栈(现在是 React、Node.js、Prisma)来描述。

我们对 Wasp 的愿景是创建一种强大而简单的语言，在这种语言中，您可以尽可能人性化地描述您的 web 应用程序。我们想让上面图片中的冰山顶部尽可能的令人愉快，同时让底部变得更小。

在这种语言中，只需几个词，您就可以指定页面及其路由，指定您想要的身份验证类型，定义基本实体/数据模型，描述基本数据流，选择您想要部署的位置，在 React/Node 中实现特定的细节，并让 Wasp 负责连接、构建和部署所有这些内容。

描述部分简单全栈 web 应用的 wasp 代码示例:

```
app todoApp {
  title: "ToDo App" /* visible in tab */
}route "/" -> page Main
page Main {
  component: import Main from "[@ext/Main](http://twitter.com/ext/Main).js"  /* Import your React code. */
}auth { /* full-stack auth out-of-the-box */
  userEntity: User,
  methods: [ EmailAndPassword ],
}entity User {=psl
  id          Int     [@id](http://twitter.com/id) [@default](http://twitter.com/default)(autoincrement())
  email       String  [@unique](http://twitter.com/unique)
  password    String
psl=}
```

查看[此处](https://github.com/wasp-lang/wasp/blob/main/examples/tutorials/TodoApp/main.wasp)的完整示例。

# 为什么是语言(DSL)，框架不是已经解决这个问题了吗？

框架(比如 Ruby on Rails 或 Meteor)对我们来说是一个很大的启发。然而，我们想更进一步——通过设计一种专门用于 web 应用领域的语言(一种 [DSL](https://en.wikipedia.org/wiki/Domain-specific_language) ),我们可以摆脱大量的样板文件，提供更干净&更简单的开发者体验。

另一方面，我们并不试图用 Wasp 取代一切，也不认为这有意义——恰恰相反，Wasp 充当 React 和 Node.js 代码之间的“粘合剂”,使您从繁重的工作中解脱出来，同时允许您保持编写自己代码的灵活性。大部分代码仍然是用 React 和 Node.js 编写的，Wasp 是整个应用程序的主干。

DSL 的另一个好处是，它允许 Wasp 在构建期间理解 web 应用程序的需求，并在生成最终代码之前进行推理，这是我们特别兴奋的地方。

例如，当生成要部署到生产中的代码时，它可以根据对 web 应用程序的理解选择最合适的架构，并将其部署到无服务器或其他类型的架构(甚至是组合)。另一个例子是在 Wasp 中定义一次数据模型逻辑，然后在栈的所有部分重用它。

DSL 开启了优化、静态分析、可扩展性和无与伦比的人体工程学的潜力。

# 它是如何工作的？

Wasp 编译器编译。wasp/React/Node.js 源代码转换成 React/Node.js 目标代码。目前，Wasp 只支持 Javascript，但我们计划很快添加 Typescript。
技术说明:Wasp 编译器是用 Haskell 实现的。

![](img/52346bcaf3dedd5516bb0da769d4c5ad.png)

虽然目前只支持 React 和 Node.js，但我们计划在未来支持多种其他技术。

生成的代码是人类可读的，并且可以很容易地被检查，甚至在 Wasp 变得过于受限时被抛弃。如果没有弹出，您就不需要查看生成的代码——它是由 Wasp 在后台生成的。

Wasp 是通过`wasp` CLI 使用的——要在开发中运行 Wasp 项目，您需要做的就是运行`wasp start`。

![](img/1691c6c7e770cc3425c7c624804d8e12.png)

# Wasp 现在在哪里，要去哪里？

我们的大愿景是将尽可能多的 web 应用领域知识移植到 Wasp 语言中，赋予 Wasp 更多的能力和灵活性。

最终，由于 Wasp 对 web 应用程序的需求有如此深刻的理解，我们可以在它的基础上生成一个可视化编辑器——允许非开发人员与开发人员一起参与开发。

此外，Wasp 不会局限于特定的技术，而是支持多种技术(React/Angular/…、Node/Go/…**。

Wasp 目前处于 Alpha 阶段，一些功能仍然粗糙或缺失，有些事情我们还没有解决，有些事情可能会随着我们的进展而改变，但您可以尝试一下，并构建和部署 web 应用程序！

# Wasp 目前支持什么:

*   ✅全栈认证(电子邮件和密码)
*   ✅页面和路由
*   ✅模糊了客户机和服务器之间的界限——定义你的服务器动作和查询，并在你的客户机代码(RPC)中直接调用它们！
*   服务器操作和查询的✅智能缓存(自动缓存失效)
*   用 Prisma.io 定义✅实体(数据模型)
*   前端的✅ ACL
*   ✅导入 NPM 依赖项

# 即将到来的是:

*   ⏳后端的 ACL
*   ⏳一键式部署
*   ⏳更多认证方法(谷歌、Linkedin、…*)
*   ⏳实体与其他功能的紧密集成
*   ⏳主题和布局
*   ⏳支持显式定义的服务器 API
*   ⏳内联 JS——混合 JS 代码和 Wasp 代码的能力！
*   ⏳类型脚本支持
*   ⏳服务器端渲染
*   ⏳可视化编辑器
*   ⏳后端对不同语言的支持
*   ⏳更丰富的 wasp 语言和更好的工具

想试试 Wasp 并得到一件很酷的 t 恤吗？加入我们的阿尔法测试计划！最多需要 30 分钟。加上包含了很多迷因:)

您可以在 https://github.com/wasp-lang/wasp[的](https://github.com/wasp-lang/wasp)查看我们的回购，并在 https://wasp-lang.dev/docs[的](https://wasp-lang.dev/docs) - >尝试一下。我们一直在寻找关于如何塑造 Wasp 的反馈和建议！

我们在 [Discord](https://discord.com/invite/rzdnErX) 上也有一个社区，我们在那里谈论与 Wasp 相关的事情——加入我们，看看我们在做什么，分享你的观点，或者在你的 Wasp 项目上获得帮助。