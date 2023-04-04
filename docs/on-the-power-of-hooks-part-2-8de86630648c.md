# 钩子的力量:第二部分

> 原文：<https://blog.devgenius.io/on-the-power-of-hooks-part-2-8de86630648c?source=collection_archive---------14----------------------->

![](img/49cc23203acab9cd757537800dd531b1.png)

由 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍照

[在前一章中，](https://medium.com/@accounts_32070/on-the-power-of-react-hooks-49094e76709c)我们已经学习了如何利用 React 钩子的能力来方便地增强带有音频回放的组件。今天，我们将继续探索钩子如何帮助我们的组件知道当前时间。好奇？让我们开始吧。

# 先生…你知道现在几点了吗？

为了让我们的组件能够回答这样一个看似简单的问题，我们需要一些要素:

*   一个可靠的 API 来访问当前时间。
*   我们组件的时钟。
*   成为时间主人的坚定信念。

假设给出了第三个要求，社区使我们能够完成前两个要求的简短工作。为了访问当前时间，我们将使用 [Moment.js](https://momentjs.com/) ，它提供了一种方便的方式来访问和显示现成的时间。为了构建我们的“时钟”，我们将利用[丹·阿布拉莫夫的工作](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)并使用他的`useInterval`钩子来创建一个内部时钟。

丹·阿布拉莫夫提供

我们现在有了我们需要的一切，可以开始拼凑我们的钩子了。

# 时间是一种幻觉

首先，让我们编写一个钩子，在调用时返回当前时间。

我们现在可以问时间了。

然后，我们将利用`useInterval`在每个`1000ms`更新当前时间。注意，我们使用`useState`而不是`useRef`，因为我们希望主机组件在每次时间更新时再次呈现。如果出于某种原因你不想要这种行为，`useRef`是你的朋友。

我们现在将被告知每秒钟的时间。

快到了。由于我们坚定不移地相信自己是时间的主人，等待整整一秒钟才知道当前时间似乎有点奇怪。因此，我们将提取这个幻数作为参数，瞧，时间成了我们的仆人。

现在时间掌握在我们手中…

# 包扎

我们现在有一个方便的钩子来通知我们的组件当前时间。让我们好好利用它吧！

我们可爱的小钟

这是我穿越 React Hooks 之旅的第二部分。你可以在这里找到第一部分的，在那里找到第三部分的[。](https://medium.com/@accounts_32070/on-the-power-of-hooks-part-3-78d79cc2a110)