# 具有使用效果的诚实编程

> 原文：<https://blog.devgenius.io/honest-programming-with-useeffect-730208d26a6c?source=collection_archive---------8----------------------->

![](img/81d3a45a4a0392ae724f73730a7841a9.png)

照片由[克里斯蒂安·埃斯科瓦尔](https://unsplash.com/@cristian1?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/light?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

`useEffect`是一个看似简单的函数，但是要很好地使用它，需要对 React 和 Javascript 有相当的了解。各种经验水平的程序员都发现自己有时会遇到这种情况。这很棘手的一个原因是，许多熟悉 React 旧 API 的生命周期方法的人(包括我自己)发现自己试图将`useEffect`翻译回`componentDidMount, componentDidUpdate`等，却发现`useEffect`不是这种范式的新语法，而是一种新范式本身。

在下面的例子中，我将向您展示我在自己的经验中遇到的一些模式，以及使 React 代码更习惯、认知更简单、错误更少的改进。

## **对**使用效果撒谎

丹·阿布拉莫夫称之为撒谎，即开发人员为了迫使`useEffect`更像一种生命周期方法那样运作而误报依赖关系。这是可能的，但它打破了`useEffect`范式，增加了复杂性，并且可以隐藏 bug。

这里有一个例子。假设我们想要呈现一个 todo 项目列表，并且我们还想要显示每个项目所属的类别。我们从 props 中获得一些信息，然后我们想从一个定制的钩子中添加类别数据，并将这个新对象传递给一个显示组件。

我们将可编辑项存储为 state，因为这个组件需要对它们进行更新，可能下游组件也需要这样做。

这里需要注意的关键点是，我们使用了`useEffect`,并给了它一个空的依赖数组(第 43 行),因为我们已经这样做了，我们还禁用了 lint 警告，告诉我们缺少依赖。

在这样做的时候，我们强迫`useEffect`只运行一次。这允许我们在第一次安装组件时获得正确的数据。然而，这并不是`useEffect`想要的表现。`useEffect`不处理组件生命周期方面的时间安排；它响应数据的变化，以使您的组件重新呈现。

## 尊重棉绒

在最近一次来自 [React Training](https://reacttraining.com/) 的研讨会上，讲师给出了这样一条经验法则:*当涉及到详尽的 deps for useEffect* 时，一定要听从你的建议。*如果你做了 linter 要求的修改，而你觉得你的代码变得更差或者不能按照你想要的方式工作，重构你的代码。*

我很少赞同“总是”，但迄今为止，听取这个建议对我很有帮助。所以我们来试试吧。

如果我们去掉`eslint-disable`，我们将从 linter 得到这个警告:

```
React Hook useEffect has missing dependencies: 'categories' and 'listItems'. Either include them or remove the dependency array  react-hooks/exhaustive-deps
```

很容易把这些加进去。下面是我们的`useEffect`代码现在的样子:

然而，我们有一个问题。如果我们在本地运行我们的应用程序并查看控制台，我们会看到以下错误:

```
Maximum update depth exceeded. This can happen when a component calls setState inside useEffect, but useEffect either doesn't have a dependency array, or one of the dependencies changes on every render.
```

为什么会出现这种无限循环？根据错误消息，我们必须有一个在每次渲染时都会改变的依赖关系。乍一看，这没有多大意义。`useEffect`唯一修改的是状态，`editableItems`，但这不在我们的 deps 数组中。不一定是因为我们没有提到`editableItems`本身；我们只调用更新它们的 set state 函数。

事情是这样的。在第一个循环中，我们调用自定义钩子来获取`categories`，这是一个类别对象数组。我们的组件渲染。然后`useEffect`被调用，它执行将类别和列表项组合在一起的逻辑。因为我们的效果改变了状态，它触发了我们的组件重新渲染。每次重新呈现之前，我们调用我们的类别钩子，它返回给我们一个全新的对象。在效果的依赖数组中，对象通过内存位置进行比较，而不是通过值进行比较。因为`categories`是`useEffect`的一个依赖项，而 categories 对象是内存中的一个新对象，这触发了我们的效果，触发了一个 render，你可以看到我们现在是如何进入一个无限循环的。

## 重构

我们听从了短绒商的建议，事情当然变得更糟。现在怎么办？有几种方法可以让我们摆脱这种情况，但对于这篇文章，我将保持我们现有的结构，只改变`useEffect.`

事实上，我要把它完全移除。让我们后退一步，记住我们正在努力实现的目标。当组件第一次呈现时，我们希望组合一些数据，以便在子组件的下游使用。最终，我们只想执行一次数据管理。

为此，我们可以依赖`useState`而完全不用`useEffect`。看起来是这样的:

我们已经将数据管理逻辑转移到了组件外部声明的函数中，并且能够将组件简化很多。没有了`useEffect`，这读起来更加清晰，因为当我们知道它们不能改变时，这个钩子天生就在寻找可以改变的东西。我们只想做一次逻辑，现在很明显我们是。

## 扩展—数据什么时候会改变呢？

到目前为止，在这个例子中，我们已经假设我们的自定义`useCategories`钩子的结果是幂等的。它是一个钩子，执行一些逻辑，并在每次被调用时返回给我们相同的类别列表。但是这个的变体呢？假设我们不是从钩子中获取数据，而是从上下文中获取数据。上下文是我们可以进行 API 调用来获取类别数据的地方。

在这个例子中，我们知道在进行 API 调用时，会有一段时间我们没有来自上下文的数据，然后在它完成后我们会有数据。

在第 40 行，我们订阅了上下文(假设我们的 ToDoList 组件被`CategoriesContextProvider`包装)。在第 43 行，我们传递类别数据来初始化我们的状态。

这看起来与我们之前的代码非常相似，但是如果我们运行它，我们会发现它从根本上被破坏了。我们将永远看不到 DOM 中可编辑项目的类别名称。这是因为开始，上下文中的`categories`是`undefined`因为我们的 API 调用还没有完成。我们用未定义的数据初始化了组件状态。状态在组件重新呈现器之间保持不变，除非我们用 set state 函数更新它。因此，即使在我们从 API 调用中获取数据后，`categories`的值在不同的渲染中会有所不同，但组件中的状态永远不会改变。

我们如何确保我们的组件响应数据的变化并相应地更新？`useEffect`当然。我们可以将它添加回去，以观察上下文值的变化。

此时，您可能会想，这看起来几乎与导致无限循环的代码相同。不同之处在于，上下文值不会在每次渲染时重新定义。对于这个例子，我们只有两种状态`categories`可以处于其中。可以是`undefined`，也可以是类别数据的数组。一旦我们有了数据，类别数据的数组将是内存中的同一个对象，在组件的整个生命周期中具有相同的值。所以我们的`useEffect`像预期的那样工作——响应可能改变的数据，并相应地更新我们的组件状态。

## 总结

`useEffect`开始使用容易，但很难持续使用。如果你发现自己有被骗上钩的冲动，不如退一步，看看能不能理清自己在什么概念上的知识。在这个过程中，您可能需要学习更多关于 React 约定以及核心 JS 概念，比如闭包和对象引用。这太好了！这意味着通过学习更多关于 React 如何工作的知识，我们可以成为更好的 JS 程序员，反之亦然。

如果你正在寻找一个真正深入的关于`useEffect`和使用它的许多细微差别的指南，我鼓励你阅读丹·阿布拉莫夫的这篇博文。他的博客涵盖了与我这里类似的问题，以及我所谈到的替代解决方案，以及更多使用钩子时可能遇到的陷阱的例子。