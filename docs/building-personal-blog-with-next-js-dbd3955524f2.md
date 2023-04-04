# 用 Next.js 构建个人博客

> 原文：<https://blog.devgenius.io/building-personal-blog-with-next-js-dbd3955524f2?source=collection_archive---------6----------------------->

![](img/5fef29d45f1a1d80f2a46d0e2bbba73c.png)

最近，我试图弄清楚我的页面应该是什么样子。它应该有博客吗？我应该使用一些流行的博客平台吗？我什么都试过了。我喜欢 [Hashnode](https://hashnode.com/) ,因为它提供了很棒的工具、对自定义域的支持以及其他很酷的特性。但是最后，我对结果并不满意。我需要更多的定制，我需要更多的自由。

我试着和 Spring Boot 以及热线一起自己建造它。我喜欢这个堆栈，并同意 Hotwire 的理念。不幸的是，要在我希望的范围内做到这一点需要很多时间，作为一名父亲，我手头没有那么多时间。

我知道接下来。JS 很受欢迎，出于工作原因，我正在研究它。我偶然看到他们的如何开始[教程](https://nextjs.org/learn/basics/create-nextjs-app)，在这个教程的范围内(花了我大约 4 个小时)，你将建立一个骨架博客。**我很惊讶它是如此的简单和快速。然后我决定用这段代码来完成这项工作。在几个晚上的时间里，我完成了我的[站点](https://ppolivka.com)的新版本，并部署到生产中。**

# 辅导的

我从没见过这么精致的教程。使用它是一种享受，我感觉我正在以光速前进。本教程将介绍创建一个基本博客的最重要的部分。您不需要非常熟悉 React 就能理解这些概念。你只需要 JavaScript 的基础知识。我建议每个人都去读一遍教程，即使只是为了看看如何制作合适的教程。

# 式样

我不擅长 CSS。我认为这是我的弱点之一。似乎总是有无数的方法去做你需要的一件事，但它从来都不清楚，对我来说这就是魔法。我听到了很多关于 TailwindCSS 的好消息，所以我决定尝试一下。你知道，自从我开始尝试新事物后。在那里我没有那么幸运。似乎很恐怖。它只是把 CSS 所有可怕的部分都拿走，移到了一个不同的层次。没有好的教程。这看起来完全是一团糟。也许我用错了，但看起来我无论如何都需要编写大量的自定义 CSS。

我决定采用最简单的方法，只用纯 CSS 来完成整个过程。没有框架，什么都没有。最后，我对结果很满意，但这是一个完美的 CSS 代码。我会说是 CSS 意面。

# 其他功能

现在，我决定添加几个本教程没有涉及到的特性。

一个是“联系我”表单。我发现了一个很棒的[教程](https://medium.com/nerd-for-tech/coding-a-contact-form-with-next-js-and-nodemailer-d3a8dc6cd645)。这很容易做到。

这里有一个小注意。我花了太多时间试图找出 API 响应没有提交的原因。而不是:

```
res.status(200)
```

你需要做的是:

```
res.status(200).end()
```

第二个是博客文章中的代码提升。为此，我决定使用 Prism.js 库。下面是一个代码片段，它为您实现了这一点。

```
const prism = require("prismjs")
 require('prismjs/components/prism-java'); require('prismjs/components/prism-typescript'); // add more if you need useEffect(() => {
 if (typeof window !== 'undefined') { prism.highlightAll(); } 
}, []);
```

# Vercel 托管

对我来说，最大的惊喜是 Vercel 主机。这也是教程的一部分，非常简单和直观。你只要给他们你的 git 回购，他们会做你的休息。您的 API 端点被部署为无服务器功能，应该由 CDN 提供的一切都自动由 CDN 提供。添加自定义域只需点击几下鼠标。

它像魔法一样有效。

# 摘要

我对 Next.JS 很感兴趣。如果我要创业，我会努力去做。它超级简单，但是非常强大。我会在 Vercel 上主持。

对于 CSS，我可能会选择 Bootstrap，它对我来说仍然是最容易使用的。

*最初发表于*[*【https://ppolivka.com】*](https://ppolivka.com/posts/building-personal-blog-with-nextjs)*。*