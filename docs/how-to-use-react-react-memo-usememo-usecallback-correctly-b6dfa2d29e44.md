# 如何正确使用 react React.memo，useMemo，useCallback

> 原文：<https://blog.devgenius.io/how-to-use-react-react-memo-usememo-usecallback-correctly-b6dfa2d29e44?source=collection_archive---------0----------------------->

![](img/8801e1d13cb1a5435730f0564e38973d.png)

图片来自[https://www.youtube.com/watch?v=3cYtqrNUiV](https://www.youtube.com/watch?v=3cYtqrNUiVw)

**目的**然而，这三个是相似目的，所以我不明白如何区分这三个 API。我有一个想法要用，这就是为什么我要分享它。

**为什么要用 React.memo，useMemo，useCallback？？一般来说，这三个 API 需要用来优化我们应用程序。基本的基础是相同的，这是防止浪费我们的电脑资源。例如，在 react 术语中，当从父组件传递了一些属性的子组件被呈现时，父组件也被呈现，即使该属性和值没有改变。这似乎不是一个小应用程序的大问题，因为这不是一个错误，所以小应用程序仍然可以正常工作。然而，更大的 app 呢？这也没有任何错误，但是如果有许多子组件从父组件传递了一些道具，这将影响页面加载。如果每次页面加载超过 1 秒，用户就会感到压力，可能会离开网站。这只是最糟糕的情况之一，但它随时都可能发生。那么，应该如何应对呢？这些是选项之一，但我们可以使用 React.memo、useMemo、useCallback(对于不同的用法，我将在后面解释)来处理防止不必要的渲染和浪费资源来记忆数据缓存并重用它。**

**react . memo，useMemo，useCallback 有什么区别？我将逐一解释这三个，虽然首先我将展示基本的想法。简单来说，React.memo 与'组件'有关，useMemo 与'值'有关，useCallback 与函数有关。准确的说，useMemo 返回值，useCallback 返回函数。
好吧，我来按顺序解释一下。**

**react . memo**
react . memo 允许我们记忆一个组件缓存，并重用它。第一个渲染信息存储在 React.memo .中，并重用输入(如道具和状态)来记忆它。

例如，这对于有很多照片或图表的组件很有用，因为这个组件有很高的重新渲染压力。
注意，您应该将这个高应力组件定义为子组件，并在父组件中调用它。如果你在一个父组件中定义了这个 React.memo，并试图在父组件中也使用它，这是行不通的，因为这些数据处理的是所有父组件内部的数据，所以没有效果可以渲染。

```
// React.memoimport React from "react";
import HighStressed from "./HighStressed";// To memorize
const ReactMemoCmp = React.memo(({ option }) => {
  console.log("render!");
  return <HighStressed option={option} />;
  // Above is a high-stressed component such as a lot of pictures or charts.
});
```

**useMemo**
useMemo 允许我们记忆一个值缓存，并重用它。下面是 useMemo 的基本用法。
useMemo 只渲染被改变的参数，在这种情况下是 a 或 b。
我们可以省略不必要的渲染，所以性能会很好。

```
// useMemoimport React, { useMemo } from "react";const memoizedValue = useMemo(() => HighStressedValue(a, b), [a, b]);
```

注意，这个钩子是在渲染过程中使用的，而不是在渲染后使用，比如 useEffect。所以，如果你想处理有副作用的方法(例如，API 请求)，你应该使用 useEffect 而不是 useMemo。

**use callback**
use callback 允许我们记忆一个函数缓存，并重用它。
下面是 useCallback 的一个基本用法。
在 useCallback 方面，可以重用任何事件处理程序，比如按钮点击。因此，如果您希望在子组件中的按钮被单击时防止子组件重新呈现，您需要使用 useCallback。

```
// useCallbackimport { useCallback } from "react";const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

注意，这个钩子基本上与 React.memo 一起使用。因为你在父组件中使用 useCallback，但是你没有在子组件中使用任何 memo，所以当这个子组件正在呈现时，这个父组件仍然在重新呈现。因此，在这种情况下，您应该对父组件使用 useCallback，并对子组件使用 React.memo。但是，如果你想使用 useCallback 和 useEffect，你可以在不使用 React.memo 的情况下处理 useCallback。这实际上取决于你的情况。

**结论**
正如我提到的，那三个 API 对于优化你的 app 非常有用。然而，这是很重要的一个方面，这实际上取决于你的应用程序，你的应用程序是否有一个高应力的组件、值、方法。因为如果你的应用程序很小，你不需要使用这些 API，因为这些 API 仍然需要一些计算机资源。换句话说，使用这些 API 可能会降低应用程序的性能。因此，首先你应该检查你应用程序是否真的需要使用这些 API，包括你的应用程序的性能、大小、组件、功能和价值。如果你想让你的应用程序变得更快，而且你有一个高度紧张的功能或组件或价值，这些 API 会帮助你。

**参考**
如何使用 useMemo React 钩子:[https://flaviocopes.com/react-hook-usememo/](https://flaviocopes.com/react-hook-usememo/)

use callback vs . use memo:[https://medium . com/@ Jan . hesters/use callback-vs-use memo-c 23 ad 1 DC 60](https://medium.com/@jan.hesters/usecallback-vs-usememo-c23ad1dc60)

深入看看 useMemo 和 useCallback 的区别:[https://procoders . tech/blog/Difference-Between-use memo-and-use callback/](https://procoders.tech/blog/difference-between-usememo-and-usecallback/)

React.memo / useCallback / useMemo の使い方、使い所を理解してパフォーマンス最適化をする: [https://qiita.com/soarflat/items/b9d3d17b8ab1f5dbfed2](https://qiita.com/soarflat/items/b9d3d17b8ab1f5dbfed2)

useMemo, useCallback, React.memoとは何か？違いは？
[https://yukimasablog.com/usememo-usecallback-react-memo](https://yukimasablog.com/usememo-usecallback-react-memo)

感谢您的阅读！！