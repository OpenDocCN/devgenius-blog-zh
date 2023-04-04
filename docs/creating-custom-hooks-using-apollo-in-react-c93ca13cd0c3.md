# 在 React 中使用 Apollo 创建自定义挂钩

> 原文：<https://blog.devgenius.io/creating-custom-hooks-using-apollo-in-react-c93ca13cd0c3?source=collection_archive---------5----------------------->

![](img/8a8ada3668eaf85924d547f1ae68c7ac.png)

你如何从后端获取数据到前端？你用 GraphQL 吗？我一直在频繁地使用 Apollo，并且非常兴奋地分享我所学到的东西。用 Apollo 钩子在 React 中获取 GraphQL 数据可以简化前端的数据管理。

在这篇文章中，我想解释一下`useQuery`和`useLazyQuery`的区别，以及如何使用这两种钩子创建自定义钩子。

**概述:**

1.  **阿波罗钩子**
2.  **使用查询 vs 使用无效查询**
3.  **使用查询**
4.  **无用查询**
5.  **总结**

作为题外话，我在这篇文章中分享了我是如何为 Apollo customed hook 创建测试的:

[](/how-to-test-custom-react-apollo-hook-using-jest-mock-beb410671539) [## 如何使用 Jest Mock 测试自定义的 React Apollo 钩子

### 我坚信编写好的测试是交付高质量产品的关键组成部分，并且我已经了解到它…

blog.devgenius.io](/how-to-test-custom-react-apollo-hook-using-jest-mock-beb410671539) 

也可以随意查看！

# 1.阿波罗胡克！

钩子是一种函数。在 React 中，一个钩子允许我们，开发者，从一个组件中提取**有状态逻辑**，以便**重用和独立测试而不改变组件层次**。

一个 Apollo 钩子在 React 应用程序中执行 GraphQL 查询。我认为它是一个方便的工具，因为它加载状态并跟踪错误。

# 2.useQuery vs useLazyQuery

这两个钩子的主要区别在于钩子是否立即执行。`useQuery`组件渲染时运行并获取数据。另一方面，当组件挂载时，`useLazyQuery`不会运行，也不会获取数据。它在结果元组中的查询函数被调用时运行。

当您的组件呈现时，`useQuery`从 Apollo 客户端返回一个包含`loading`、`error`和`data`属性的对象。`useLazyQUery`也返回该数据，但作为其结果元组的第二个元素。

[](https://www.apollographql.com/docs/react/data/queries) [## 问题

### 本文展示了如何使用 useQuery 钩子在 React 中获取 GraphQL 数据，并将结果附加到您的 UI 中。你会…

www.apollographql.com](https://www.apollographql.com/docs/react/data/queries) 

# 3.使用查询

让我们来看看这个定制挂钩:

这个定制钩子`yourFunction`使用 Apollo 的`useQuery`并返回一个值为`loading`、`error`和`data`的对象。第一次呈现`loading`会返回`true`。然后它再次运行以返回获取的`data`，然后`loading`将返回`false`。

我没有返回`loading`、`error`和`data`的值，而是自定义`yourFunction`返回`loading`、`error`、`isName`。

`isName`是从我们从第 18 行的`useQuery`返回的数据中返回`name`的字符串值。

# 4.无用查询

在`useLazyQuery`的返回元组中，第一个元素是查询函数，我将其命名为`getNyName`。第二个元素`nameResult`，是由`useQuery` — `loading`、`error`和`data`返回的同一个结果对象。

通过在钩子中异步调用`myName`查询函数，我们可以避免使用`useEffect`。

# 5.摘要

这两个钩子的主要区别在于`useQuery`会立即执行，而`useLazyQuery`不会。理解了这种差异之后，我们回顾了使用 Apollo 创建定制钩子的方法。

当你鼓掌时👏我的帖子和订阅，你支持我的科技之旅，我将不胜感激:)

谢谢，祝编码愉快！