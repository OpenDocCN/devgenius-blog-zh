# 使用这 7 个技巧来获得干净和可维护的代码

> 原文：<https://blog.devgenius.io/use-these-7-tips-for-clean-and-maintainable-code-d6eface6149d?source=collection_archive---------7----------------------->

## 利用这些技巧在你的开发者朋友中获得尊重😎

![](img/51bbd4b2b4d40065b93d853b3bc39df4.png)

JESHOOTS.COM 在 [Unsplash](https://unsplash.com/s/photos/cleaning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上[拍照](https://unsplash.com/@jeshoots?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

这一切都是为了维护干净和可读的代码(这当然是可行的)。所以，我想和你们分享一些建议。欢迎在评论中纠正我或分享更多你自己的建议，为我们所有人创造一个学习的经历。让我们开始吧:

# 1.评论，评论和更多评论

我不是一个理想的开发人员，我打赌你也不是。我将告诉你不要做什么，而不是解释该做什么。让我们来看一些示例代码:

相反，当您有复杂代码块时，请使用注释。

# 2.有意义的名字

你想用那种读者会说，“是的！我很清楚这是干什么的”。

# 3.不要忽略控制台/ESLint 警告

这是一个巨大的问题，所以我见过很多次开发人员提交带有 eslint 警告的代码。事实上，几个月前，我开始着手一个现有的项目。当我编译前端的时候，有超过 100 个来自 jsx-ally，eslint 等的警告。

我们可以将 [husky](https://www.npmjs.com/package/husky) 与 [lint-staged](https://www.npmjs.com/package/lint-staged) 一起使用，它不会让您提交代码，直到您清除所有警告和错误。

# 4.不要在打字稿中使用“`as unknown”`

Typescript 很聪明，但是有时候，它不够聪明！我在打字稿代码中见过很多`// @ts-ignore`或`as unknown`。所以，看看这个例子:

尽管这样做是不可取的，但至少你得到了一些类型安全。

# 5.用巴别塔代替 tsc

从版本 7 开始，Babel 增加了对 TypeScript 的支持，这意味着您不再需要使用 TypeScript 编译器(即`tsc`来构建您的项目，而是可以只使用 Babel，它只是从所有 TypeScript 文件中剥离您的类型，然后以 JavaScript 的形式发出结果。

这不仅比 tsc 快得多，尤其是在更大的项目中，而且允许您在您的项目中使用整个 Babel 生态系统。例如，当您想使用仍处于第 3 阶段的 react 或 javascript 特性时，它非常有用。

对于后端项目，这意味着您可以简化笨重的文件查看脚本，只需使用 babel-node 来查看更改。

# 6.使用 SonarJS 和 Eslint

Eslint 有许多实施最佳实践和惯例的规则，也有助于防止错误。

(不赞成使用 TSLint，而赞成使用 typescript-eslint；TSLint 插件 SonarTS 已经被采用，现在是 SonarJS 的一部分。

除了 ESLint 的特性之外，SonarJS 还为您的代码添加了一些复杂性检查，这有助于您先编写代码，然后将您的方法分成更小的部分。

# 7.不透明类型

我不打算解释，我只是给你演示一下。

假设我们正在构建一个银行 API。

account.ts

控制器. ts

你发现窃听器了吗？如果没有，看看我们调用`spend()`函数的地方。我已经(有意地)在 accountNumber 之前传递了金额，但是 typescript 没有抱怨。

如果你想知道为什么会发生这种情况，这是因为`AccountNumberType`和`PaymentAmt`可以互相赋值，因为它们都是类型`number`。

关于这一点，typescript repo 中有一个长期存在的问题。在 typescript 团队对此采取措施之前，我们可以使用下面的方法:

实用函数 Opaque 简单地定义了一个新类型，除了变量值之外，它还存储一个(唯一的)键，比如 Uuid。

# 结论

感谢阅读！在 [twitter](https://twitter.com/AkashShyam11) 上关注我，在那里我为开发者发布提示、技巧和迷因。再见🤟

*原载于 2020 年 10 月 26 日*[*https://dev . to*](https://dev.to/akashshyam/7-tips-for-clean-code-3nk1)*。*