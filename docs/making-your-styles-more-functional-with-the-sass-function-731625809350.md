# 用 SASS @函数让你的风格更实用

> 原文：<https://blog.devgenius.io/making-your-styles-more-functional-with-the-sass-function-731625809350?source=collection_archive---------3----------------------->

![](img/3953c615d91890016cdcde5a6f71d4ef.png)

来自 [Pexels](https://www.pexels.com/photo/cold-night-1853542/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Artem Saranin](https://www.pexels.com/@arts?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 摄影

你知道你可以在 SCSS 写函数，就像用 Javascript 一样吗？我也是，直到最近。我被这个震惊了，所以我想分享我的发现。

要编写一个新的函数，我们只需进入我们的`.scss`文件，并设置一个，如下所示。就像在 Javascript 中一样，您只需要函数名、各种参数以及您期望返回的任何内容。

例如，让我们创建一个函数，它从 param pixelSize 中获取一个值，执行一个非常基本的计算，然后输出一个我们可以在类中使用的值。这将把 pixelSize 转换成一个 px 值，并将字符串 px 附加到末尾，这样当我们调用函数时就不需要再做什么了。

正如您在上面看到的，将 32 传递到函数中会将其转换为 16。虽然这是一个非常普通的例子，但是您可以创建一个函数，允许您根据用户屏幕的大小应用不同的值。将一个函数与 mixins 和 SCSS 的各种工具配对将使您的代码更加强大。如果你想阅读更多关于这个主题的内容，我在这里附上了文档[的链接。](https://sass-lang.com/documentation/at-rules/function)

如果你对这个话题有任何问题，请在下面的评论中告诉我，或者联系我的社交媒体账户。谢谢，祝你有美好的一天！

# 结论

这只是对 SASS 函数的一个粗浅的探讨，但我希望如果您有任何反馈，发表评论并让我知道我可以改进的地方，会对您有所帮助。

如果你想看看我的其他帖子，可以在这里找到。我写的都是前端特有的东西，所以我有关于 SASS 、 [Regex](https://javascript.plainenglish.io/how-to-get-a-better-grasp-on-patterns-with-regex-regular-expressions-an-introduction-9f1be83da76) 、[上下文 API](https://javascript.plainenglish.io/write-neater-code-by-utilizing-the-react-context-api-an-introduc-ti-a6a450b54e11) 、 [JavaScript](https://javascript.plainenglish.io/want-to-write-better-javascript-a-few-cool-features-to-help-out-54b80eddf85d) 、[获取 API](https://avetwhocodes.com/fetching-data-from-an-api-with-the-fetch-api-in-react-5dbe0abcfb41) 、[反应](https://javascript.plainenglish.io/level-up-your-react-skills-with-the-use-of-composition-766a41f544c9)、&更多关于 [SASS](https://medium.com/codex/writing-better-sass-with-dynamic-class-generators-e486a0413d0d) 的文章。