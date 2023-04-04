# 正面朝上:无头 Wordpress 和正面反应

> 原文：<https://blog.devgenius.io/fronting-up-headless-wordpress-and-react-with-frontity-10422993a9db?source=collection_archive---------17----------------------->

![](img/c527259ebe0fe3f5483d1e63b513fd67.png)

网站上的 Frontity 演示

大约一年前，我接触了关于建立一个新的网站与 CMS 为客户作为一个自由职业者的项目。首先，我现在没有做那么多付费的自由职业项目，其次，当我过去做的时候，我通常保持简单，使用世界上最受欢迎的内容管理系统——Wordpress。我曾经在很多小型自由职业项目中使用 Wordpress 作为 CMS 的入口，因为它提供了一个基本的免费 CMS，并且已经存在了很长时间，我很熟悉它的工作方式。我不是 PHP 开发人员，所以我通常只购买一个高级主题并安装它，进行一些定制，然后嘣…我们就可以开始了。

然而这一次，我想看看是否有一种方法可以在项目中使用 React 并构建一个 React 应用程序，因为我的技能是 Javascript，并且我正在尝试使用 React 构建前端。所以我花了一些时间研究免费的无头内容管理系统，只有几个我认为可行的选项。几乎所有这些都依赖于利用像 Gatsby 或 Hugo 这样的东西，以及 GraphQL，在某些情况下还有像 Drupal 或 Netlify 这样的 CMS 和其他 JAMStack 技术。所有这些我以前都没有用过，因为这是一份有偿工作，我没有那么多时间开始学习这些框架或技术堆栈。

然后我偶然发现了一个叫做 [Frontity](https://frontity.org/) 的框架。Frontity 允许你使用 Wordpress REST API 将 Wordpress 作为一个无头 CMS。然后，您可以使用 React 组件在 Frontity 中构建您的前端应用程序，“Frontity 将使用无服务器预渲染(SPR)提供动态内容的即时体验。”然后，你可以将前端应用部署到任何 Node.js 或 severless 主机提供商，尽管他们建议使用 [Vercel](https://vercel.com/) ，反正我在我的其他项目中碰巧使用了它，所以这是我的另一个优势。推荐的安装模式&是我坚持使用的，将你的 Wordpress 安装在你的主域名的一个子域上，然后你的主域名指向 Vercel 服务器。

> " Frontity 将使用无服务器预渲染(SPR)提供动态内容的即时体验."

总而言之，Frontity 为这类项目选择了很多选项。我的客户熟悉在 Wordpress 中发布和编辑内容，我也熟悉和自由地在 React 和 Javascript 中构建前端组件。与 Wordpress 网站相比，您完成的网站/Frontity 应用程序也非常快，页面路由由 Frontity API 负责，因此不需要 React Router 方面的专业知识。事实上，因为它是一个自以为是的框架，是专门为 Wordpress 构建的，它有自己的状态管理器，并使用 CSS-in-JS 进行样式化。由于这一点，你不必弄清楚如何配置这些工具或学习额外的框架和技术，如 GraphQL，Redux 或 Webpack。一切都是固定的，很容易上手并运行。

Frontity 也有一些自己的节点包，您需要安装它们来处理某些功能，您也可以安装任何其他您想要的 npm 包。Frontity 也有一些预先存在的主题，您可以安装这些主题，以一个基本的结构开始您的项目，然后进行定制。它还有扩展，可以在不改变代码的情况下激活和停用，并且可以跨项目重用。它也是 SEO 友好的，可以在服务器端和客户端渲染。

我发现非常有用的一点是，Wordpress 插件仍然可以在前端使用，只要它们与 Wordpress REST API 兼容。这意味着像联系表格和 PDF 阅读器都是由 Wordpress 处理的。它还允许你使用定制的文章类型和定制的分类法，如果你愿意的话，你只需要在你的项目中调整一些设置。

我听到你问有好的文档吗？嗯，是的，有优秀的文档和活跃的社区和留言板。还有一个 youtube 频道，里面有一些非常有用的视频。我唯一的担忧，也是最近的担忧，是 Frontity 最近被 Wordpress 的母公司 Automattic 收购了。这对于框架意味着什么，我最初并不确定，但是根据[的博客帖子](https://frontity.org/blog/frontity-is-joining-automattic/)和[的 Twitter 帖子【Frontity 背后的团队已经加入 Automattic 来开发 Wordpress Core。我确实注意到 Frontity 社区的活跃用户数量有所下降，有时在获得支持方面确实开始变得缓慢，但后来其他用户通常会伸出援手，我会尽可能地回报他们。然而，因为它是一项开源技术，所以希望它的用户社区仍然会努力帮助它向前发展。不管怎样，我还是希望如此，所以我希望通过传播这个框架来吸引其他人来检验它。](https://twitter.com/luisherranz/status/1437354344844832776?s=20&t=TNczWni233h--Dvr7X9vGg)

总之，我认为 Frontity 为爱好 web 开发人员和自由 web 开发人员提供了一个真正有用的框架和易于使用的 API，他们需要一个负担得起的、可能熟悉的、将与 React 技术堆栈一起工作的 headless CMS。我只是希望 Frontity 有一个未来，现在它不像创始团队那样积极维护。希望我已经激起了你的兴趣，你会检查它，并考虑在未来的项目。

这个故事也有@[https://www . nmk . dev/post/fronting-up-headless-WordPress-and-react-with-frontity-10422993 a9db](https://www.nmk.dev/post/fronting-up-headless-wordpress-and-react-with-frontity-10422993a9db)