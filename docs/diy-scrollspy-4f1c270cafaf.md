# DIY: ScrollSpy

> 原文：<https://blog.devgenius.io/diy-scrollspy-4f1c270cafaf?source=collection_archive---------0----------------------->

## 使用反应和交叉点观察器

![](img/ce197d7573ae27e5a9797b5a25bd9927.png)

我经常发现，如果没有某种形式的实际应用，软件开发的理论方面对我来说将会迷失。在我职业生涯的早期，我会通过尝试自己建造东西的形式来寻求对事物如何运作的更好理解。这种“自己动手”的态度帮助我培养了充分利用现有库编写代码的能力。我坚信，我们不仅应该努力`know the documentation`和`use the tools`，还应该能够构建我们希望在项目中使用的东西。

考虑到这一点，我想我应该开始一个 DIY 系列，采用一些我们在野外很容易找到的功能，并提供一个易于实现的版本。

本文将重点介绍 ScrollSpy。

# 什么是 ScrollSpy？

很高兴你问了！

一个`ScrollSpy`只是一些模式，允许你监视滚动事件，并在用户或浏览器滚动页面时做一些事情。这对于创建导航菜单非常方便，每当用户滚动到页面上的一个新部分时，或者当开发人员自动执行一些滚动到这一部分的魔术时，导航菜单会更新以显示哪个项目是活动的。这是一个`ScrollSpy`非常常见的用法。事实上，在 [Bootstrap 对 Scrollspy](https://getbootstrap.com/docs/5.0/components/scrollspy/) 的实现中可以找到一个流行的例子。如果你想变得有创意，你也可以用它来制作动画部分，惰性加载内容，或者任何你能想到的东西。在谷歌商店页面上可以找到一个使用`ScrollSpy`模式制作一些内容动画的极好例子，它是关于[谷歌像素芽](https://store.google.com/au/product/pixel_buds?utm_source=google&utm_medium=cpc&utm_campaign=japac-AU-en-dr-bkws-super-all-buy-e-dr-1008675&utm_content=text-ad-none-none-DEV_c-CRE_460680811804-ADGP_Hybrid+%7C+BKWS+-+EXA+~+Pixel+Buds+~+%5B1:1%5D+~+AU+~+en+~+google+buds-KWID_43700056950091231-kwd-370572617021-userloc_9071405&utm_term=KW_google+buds-ST_google+buds&gclid=CjwKCAjwtJ2FBhAuEiwAIKu19j2P4BXmx1snw9k5cDtLb9uMkiJRYWx5_mfm27G27SQ_H73GUUsRahoCGkYQAvD_BwE&gclsrc=aw.ds&hl=en-GB)(这不是广告😬！).那一页上有一些精彩的 UX，我会让你自己去翻阅，看看我的意思。

# 创建自己的 ScrollSpy

有很多方法可以为你的项目创建一个`ScrollSpy`模式。我将重点介绍如何结合使用 Intersection Observer 和 React 来创建一个简单的导航菜单，当用户滚动页面时，它会更新所选的值。我将假设您了解在项目中设置和使用 React 的相关知识，但将详细介绍交叉点观察器。

## 交叉观察者

Intersection Observer 是一个 API，它允许您观察某个元素与其父元素或祖先元素的交集的变化。我们可以使用这个 API 来检查一个元素是否在视窗中，或者它在页面上相对于其祖先的位置。我们将使用交叉点观察器 API 作为我们代码的核心部分，来确定与更新我们将要构建的导航菜单相关的`when, what, and how`。

如果您想了解更多关于交叉点观察器的信息，您可以在 [MDN Web 文档](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)中找到。

让我们开始写一些代码。好吗？

## ScrollSpy 代码

首先让我们创建我们的`ScrollSpy`组件。在这个组件中，我们有两个关键的功能部分— `isInViewPort`和`Intersection Observer`。

`isInViewPort`

isInViewPort 函数

`IsInViewPort`是一个非常简单的函数，它接收来自`IntersectionObserver`的`entry`，并使用来自条目`boundingClientRect`的属性来计算元素是否仍在视口中。这将有助于我们防止多个菜单项同时被激活。

`Intersection Observer`

交叉口观察者代码

这里我们将从 DOM 中获取我们想要观察的元素，然后对它们进行迭代，为每个元素创建一个`new IntersectionObserver`。我们的目标是一个`data-scrollspy`属性，但是可以很容易地传入一个选择器来使用。我有目的地这样做是为了将 ScrollSpy 代码从依赖于被告知*选择什么*中抽象出来。它应该知道。继续，我们的`new IntersectionObserver`接受回调和一些选择。我们使用的回调函数将对`entries`执行`forEach`，并将`entry`和`isInViewPort`结果传递给作为`ScrollSpy`组件的属性传递的`handleScroll`函数。这些选项会将浏览器设置为视区、视区周围的边距，以及要在其中执行回调的被观察元素的可见性百分比。最后我们用`observer.observe()`设置我们想要观察的元素

此时，您的`ScrollSpy`组件应该如下所示:

ScrollSpy 组件

我已经将它编码为一个返回 null 的 React 组件，但是有很多方法可以做到这一点。

*更多关于路口观察者回调的信息和选项可以在* [*这里找到*](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#creating_an_intersection_observer) *。*

接下来，让我们创建一个将利用我们的`ScrollSpy`组件的`NavMenu`。

`NavMenu`文件由一个效用函数和两个 React 组件组成:`onScrollUpdate`、`NavMenu`和`WithNavMenu`。

`onScrollUpdate`

onScrollUpdate 代码

`onScrollUpdate`(前面提到过)作为`handleScroll`道具传入`ScrollSpy`。每当观察到的元素之一满足特定条件时，该功能将允许我们更新菜单项的活动状态。在这种情况下，我们希望检查`y`值是否小于或等于 0，元素是否在视口中。如果这是真的，那么我们将它设置为活动的，否则我们将活动的类从元素中移除，为另一个元素让路。

`NavMenu`

导航菜单组件

`NavMenu`组件接收`options`并使用它们构建导航菜单。通常情况下，我们会希望使用本地浏览器功能的锚标签，以滚动到页面上的一节。在我们的例子中，我们希望对页面滚动到哪里有更多的控制，以避免任何奇怪的行为。出于这个原因，我们创建了一个`onClick`函数，它否定了锚标签的默认行为，并接管了在浏览器中设置散列以及滚动到正确元素的工作。

`WithNavMenu`

带导航菜单的高阶组件

`WithNavMenu`是一个高阶组件(HOC ),我们将使用它来生成选项数组并呈现我们的`ScrollSpy`组件和可观察元素。这个组件接收`children`和一个`selector`。`selector`是我们想要用来构建`NavMenu`选项的组件的标识符。当查询 DOM 中的选择器时，我们映射返回的元素，以有意义的方式形成选项。例如，导航菜单选项中使用的标题是从映射元素`dataset`属性中提取的。

完整的`NavMenu`文件应该如下所示:

所有导航菜单代码

最后，我们可以像这样实现我们的 NavMenu 和 ScrollSpy:

NavMenu 和 ScrollSpy 示例实现

这里我们使用`WithNavMenu` HOC 来包装一组具有 id 的`section`标签，通过`data-nav-title`属性设置标题，并通过`data-scrollspy`属性将它们自己标记为准备好被我们的`ScrollSpy`观察。

也有一些 css 使这一切看起来很漂亮，但现在我只想关注这一点:

平滑滚动 css

这段 css 只是告诉浏览器让用户有流畅的滚动体验。

解决所有这些问题后，您应该有一个这样的页面:

当您点击菜单选项时，一个新的部分被滚动到视窗的顶部，当您滚动与该部分匹配的菜单选项时，该部分被设置为`active`。超级爽！

这只是一个 ScrollSpy 的简单实现。它有更多的功能，你可以用它做更多的事情！

*查看所有这些代码一起工作* [*检查这个代码沙箱！*](https://codesandbox.io/s/reverent-surf-nkzzv?) *或者，您可以* [*查看 GitHub*](https://gist.github.com/dcayers/771fa5f0c0d14f31ae4a47d36ee7f376) *上的要点，查看本文中使用的所有代码和片段。*

## ScrollSpy 库

快速的谷歌搜索会为 ScrollSpy 图书馆带来一些极好的结果。我想到的几个例子是:

*   [反应-滚动](https://www.npmjs.com/package/react-scroll)
*   [vue2-scrollspy](https://www.npmjs.com/package/vue2-scrollspy)
*   [angular-scrollspy](https://www.npmjs.com/package/@thisissoon/angular-scrollspy)
*   [自举 Scrollspy](https://getbootstrap.com/docs/5.0/components/scrollspy/)

请浏览一下这些代码，如果您还没有的话，请尝试一下！

好了，这就是我们要说的。希望我已经帮助你获得了一些知识，你也明白了自己动手做东西的重要性。我个人认为，对于处于各个阶段的开发人员来说，这是一个很好的练习，有助于培养您实现外部库使用的能力，而不仅仅是了解文档告诉您的内容。

感谢你的阅读，我希望你在下一个项目中好运连连！