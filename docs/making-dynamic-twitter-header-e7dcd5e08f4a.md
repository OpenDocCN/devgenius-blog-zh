# 制作动态 Twitter 标题

> 原文：<https://blog.devgenius.io/making-dynamic-twitter-header-e7dcd5e08f4a?source=collection_archive---------8----------------------->

![](img/c437aa46e0fcd079c70ac152aab8312d.png)

最近我看到一个动态显示新粉丝图片的 Twitter 标题。我爱上了这个想法，所以我决定自己创造一个。

这应该很简单，我只会写一个简单的脚本，将采取一个背景图像，通过 Twitter API 下载追随者的列表，他们的个人资料图像，并把他们放入背景图像。之后，通过相同的 API，它将上传图像作为一个新的头。

作为一名真正的开发人员，我决定在谷歌上搜索如何做到这一点，我发现了 Thobias Schimdt 的这篇令人惊叹的文章。我无耻地抄袭了他的大部分代码。我决定以不同的方式部署它(不是在 AWS 上)。在这篇文章中，我将回顾我的变化。

最后，我的代码看起来像这样。

我使用的背景图片是由 [Canva Twitter Header Tool](https://www.canva.com/create/twitter-headers/) 创建的，即使你不擅长设计，你也可以创建一个令人惊叹的标题。

为了让 Twitter API 允许你下载你的追随者信息，你需要一个叫做提升 API 级别的访问权限。更多信息请点击[。](https://developer.twitter.com/en/docs/twitter-api/getting-started/about-twitter-api#Access)

我决定将其部署为 [Netlify 函数](https://www.netlify.com/products/functions/)。所以我的代码保存在 netlify/function/header.js 文件中。

要在本地推出该产品，您可以

```
npm run-func netlify/functions/header.js handler
```

您可以像这样将它添加到 package.json 文件中:

我将资产存储在网络/功能/资产文件夹中。为了让 Netlify 用您的函数部署这些文件，您需要这样告诉它。您可以使用项目根目录中的 netlify.toml 文件来完成此操作。

要部署到 Netlify，只需将所有代码推送到 GitHub。登录/注册 Netlify 并选择您的 GitHub repo。Netlify 会为你表演所有的魔术。几秒钟后，他们将为您提供一个 URL，您可以调用它来触发您的功能。

太好了。现在我们需要定期运行这个，这样我们就可以捕捉到所有新的关注者和文章。为此，我决定使用 EasyCron。这是一个超级易用的平台，你可以说。好的，每分钟都给这个网址打电话。对于我们的用例，这就足够了，而且是免费的。

现在我们拥有了一切。我们可以享受我们的真棒免费动态 Twitter 头。

如果你喜欢这篇文章，你可以在 Twitter 上关注我。

*最初发表于*[*【https://ppolivka.com】*](https://ppolivka.com/posts/dynamic-twitter-header)*。*

*更多内容尽在*[*blog . dev genius . io*](http://blog.devgenius.io)*。*