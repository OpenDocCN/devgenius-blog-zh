# 在 Next.js 博客中添加分页

> 原文：<https://blog.devgenius.io/adding-pagination-into-next-js-blog-685d04dcb2c1?source=collection_archive---------0----------------------->

![](img/649127b603fb774046b7c41cace91479.png)

我最近用 Next.js 重新设计了我的博客。我使用了令人惊叹的 Next.js 教程，我对此非常满意。但是随着时间的推移，我写了越来越多的文章，很明显我需要添加分页。我不是 Next 方面的专家，事实证明添加分页并不容易。我对我的列表页面使用静态生成，生成所有页面不是一个选项。出于搜索引擎优化的原因，我决定切换到服务器端渲染，但我也想在飞行中切换页面。

# 添加 API

首先，我需要添加一个 API 调用来提供分页信息和列表文章。我在一个根 api 文件夹中创建了一个 posts 目录，并在那里创建了一个[page]。js 文件。这个文件将是我的 api 处理程序。

这是非常简单的代码。它正在对所有帖子的数组进行统计。注意，如果您部署到 Vercel，那么您的 api 调用将作为无服务器函数部署，您需要告诉 Vercel 将您的 markdown 文件添加到无服务器部署中。这是通过根 vercel.json 文件完成的。

根帖子目录是我存放所有降价文件的地方。

# 修改博客列表页面

我使用了 next.js 教程中的博客列表页面。我使用的是静态页面生成。所以我做的第一件事就是把它改成服务器端渲染。

它获取我们的新 api 调用，并将其作为组件属性返回。对于 localhost 和 prod，服务器变量是不同的。我们需要指定完整的路径，因为这将从服务器调用。

我使用 next/router 在页面间导航。为了让所有的事情更加用户友好，我添加了一个装载路线变化的动画。

为了呈现帖子或加载内容，我在这个样式中有一个 if。

对于实际的分页导航，我使用了令人敬畏的组件 react-paginate。

它引用了分页处理函数，该函数具有实际的导航逻辑。

你可以在这个[要点](https://gist.github.com/PavlikPolivka/a3125f354757a498f769ed28a2840991)里看到整个博客页面。

如果你喜欢这篇文章，你可以在[推特](https://twitter.com/pavel_polivka)上关注我。

【https://ppolivka.com】最初发表于[](https://ppolivka.com/posts/nextjs-pagination)**。**