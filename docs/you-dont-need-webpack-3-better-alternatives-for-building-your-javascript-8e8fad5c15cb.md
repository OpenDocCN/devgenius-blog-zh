# 你不需要 web pack——构建 JavaScript 的 3 个更好的选择

> 原文：<https://blog.devgenius.io/you-dont-need-webpack-3-better-alternatives-for-building-your-javascript-8e8fad5c15cb?source=collection_archive---------7----------------------->

## 新一代 JavaScript bundlers 提供了极大改进的开发人员体验

如果您正在构建任何类型的具有多个源文件和依赖项的前端应用程序，您将需要一个构建工具来帮助您解决*捆绑的问题—* 将所有源文件和依赖项放在一起，形成浏览器可以理解的形式。

![](img/97c7505b081ac2a5baee5278e88a903a.png)

由[卡伦·彭罗斯](https://unsplash.com/@penrosekaren?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

大多数人都熟悉 Webpack，这是解决这个问题的最流行的工具，但是它也有一些很大的缺点。新一代 JavaScript 构建工具可以极大地改善开发人员的体验。在本帖中，我们将回顾几个选项。

# JavaScript 构建工具是做什么的？

在我们进入不同的工具之前，让我们简单地讨论一下 JavaScript 构建工具*通常*做什么。简而言之，这些构建工具获取 JavaScript 和依赖项，并准备在浏览器中使用。以下是涉及的一些步骤:

*   解决方案—查找各种源组件在磁盘上的位置。
*   转换—将不同形式的 JavaScript/TypeScript(通常还有其他资源，如 HTML、CSS 和图像)转换成浏览器可以使用的标准打包形式。
*   捆绑——将许多源组件编译成少量 JavaScript 包，浏览器可以轻松加载这些包。*一些较新的构建工具在某些情况下能够避免这一步，以提高性能*。
*   缩小——通过删除不必要的代码、重命名变量和许多其他优化技术来缩小 JavaScript 包的大小。
*   服务—在开发模式下向浏览器提供内容，包括像“热”重新加载这样的特性

请注意，并非所有工具都执行所有这些步骤，或者以完全相同的方式执行，但是这些是大多数构建工具以某种方式解决的一些核心问题。

# Webpack 有什么问题

许多年来， [Webpack](https://webpack.js.org/) 几乎是镇上唯一的游戏。从好的方面来看，Webpack 非常强大&非常灵活:它可以通过一个可扩展的配置文件对您的 JavaScript 包做任何事情。

但是 Webpack 的核心捆绑策略和难以置信的灵活性也导致了它最严重的缺点:速度。随着 JavaScript 项目的增长，Webpack 重新构建代码库所花费的时间也在增长，这最终会成为开发和发布周期的一个真正问题。当开发人员在进行代码更改后需要等待 5 秒、10 秒或更长时间才能看到结果时，整个开发过程会明显变慢。尽管有各种通过缓存、优化和其他配置来提高速度的策略，但这可能是一个要解决的重要问题，需要付出巨大的努力。

第二个相关的问题是配置的复杂性。因为 Webpack 是如此的开放和强大，所以配置可能相当复杂。开发人员有时不得不拼凑他们自己复杂的 JavaScript 管道，包括外部工具和加载器，即使是做简单的事情。实际上，这些外部工具和加载程序有时会相互冲突，或者版本或配置不兼容。

# 替代方案

因此，让我们深入研究 JavaScript bundlers 的选择，看看它们比 Webpack 有什么优势。

## 轻快地

[Vite](http://vitejs.dev) 是一种新的构建工具，通过采用一种非常不同的方法极大地加速了开发循环。在这篇文章的开始，我们说 JavaScript 构建工具有助于解决*捆绑的问题。*然而，事实证明捆绑步骤本身并不总是*所需要的*。特别是，当在现代浏览器中用于开发目的时，在 ES6 模块的帮助下，我们实际上可以跳过捆绑步骤。

Vite 让浏览器自己请求导入，而不是预处理和捆绑项目的所有模块到一个 JS 文件中。Vite 所要做的就是解析导入，如果必要的话，将它们翻译成 ES6 模块。这大大加快了我们在开发过程中做出改变后重新加载代码的速度。

![](img/6507d01abace59b6f05e543a5e45e8a2.png)

Vite Logo(图片来自 [vitejs.dev](/vitejs.dev) )

到了部署到*产品的时候，*我们仍然需要捆绑我们的代码，所以 Vite 确实提供了这个功能，尽管它使用了其他工具，包括 Esbuild，我们将在下面讨论。

我很容易地在一个新项目上安装并运行了 Vite，尽管由于更复杂的配置和依赖性，我在尝试移植现有的 Webpack 项目时遇到了一些麻烦。

**何时使用:**

✅:你想在开发中优先考虑闪电般的快速重装

新项目或没有复杂配置的现有项目的✅

## 包裹

[Parcel](https://parceljs.org/) 是一款出色的零配置捆扎机。在新项目中设置和使用非常容易，几乎不需要任何配置。简单地从您的项目入口点开始，通常是一个 HTML 文件，并且包含的任何资产都被适当地打包和服务。它支持开箱即用的大量语言和配置。

![](img/2b115e2cbbb9c7a02cb48d514fef57ba.png)

包裹标志(图片来自[parceljs.org](https://parceljs.org/))

尽管它很简单，但它也非常强大和灵活，支持许多高级捆绑选项，几乎不需要配置，而且还具有可扩展的插件系统。它也比 Webpack 快得多，尽管没有 Esbuild 快，我们将在下面讨论。

**何时使用:**

✅:你想要一个比 Webpack 更快更简单的捆扎机

✅你想利用一些先进的功能，而不需要大量的配置

## esbuild

[Esbuild](https://esbuild.github.io/) 也采用了传统的捆绑方法，但是速度非常快。Esbuild 使用`go`而不是 JavaScript 编写，并合理利用并行化，估计其速度是包括 package 和 Webpack 在内的其他捆绑器的 50-100 倍。

![](img/928f8803ea07533c763cbea65028f2f9.png)

esbuild(图片来自 [esbuild.github.io](/esbuild.github.io) )

尽管 es build[快得令人难以置信](https://esbuild.github.io/faq/#benchmark-details)，但其他构建器缺少一些特性，这可能会使它在某些项目的生产中使用起来具有挑战性。例如，Esbuild 不像其他一些构建工具那样具有广泛的*内置*语言和框架支持(尽管在许多情况下第三方开发者已经构建了解决方案)。

此外，与其他一些解决方案不同，它是一个只构建的工具，没有内置的开发服务器。设置单独的开发服务器非常容易，但是对于 Esbuild 来说这是一个额外的步骤，可能无法提供相同级别的集成。

自从几个版本前 Esbuild 成为凤凰框架的默认捆绑器以来，我就一直在使用它，我对它非常满意。这种配置工作得特别好，因为 Phoenix 负责服务器方面，并且开发和生产之间的设置基本相同

**何时选择 Esbuild:**

✅:你想要传统的捆绑销售方式，但又希望它快如闪电

✅:你可以设置自己的开发服务器(或者已经在使用了，就像 Phoenix 这样的框架)。

✅:你确定你的特定语言/框架是受支持的——你对 Esbuild 的一些“缺失”特性没有意见

# 重述 JavaScript 捆绑的三大选择

我们已经看到了 JavaScript 工作流的三个强大的替代方案:Vite、Parcel 和 Esbuild。所有这些工具都可以为您的 JavaScript 开发流程提供比 Webpack 更大的改进。如果您已经有一段时间没有更新您的 JavaScript 工具了，现在是时候了！

*原载于*[*Blixtdev*](https://blixtdev.com/you-dont-need-webpack-3-better-alternatives-for-building-your-javascript)*。*

*Jonathan 在大型和小型创业公司中拥有超过 20 年的工程领导经验。如果你喜欢这篇文章，请考虑加入 Medium 来支持* [*乔纳森和其他成千上万的作者*](https://medium.com/@jonnystartup/membership) *。*

[](https://medium.com/@jonnystartup/membership) [## 用我的推荐链接加入媒体-乔纳森

### 阅读乔纳森(以及媒体上成千上万的其他作家)的每一个故事。您的会员费直接支持…

medium.com](https://medium.com/@jonnystartup/membership)