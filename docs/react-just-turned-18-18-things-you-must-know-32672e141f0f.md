# React 刚满 18 岁！你必须知道的 18 件事！

> 原文：<https://blog.devgenius.io/react-just-turned-18-18-things-you-must-know-32672e141f0f?source=collection_archive---------3----------------------->

![](img/7ddf0cbbf4a48a104411673c6117bf16.png)

由[安德里亚·米尼尼](https://unsplash.com/@_andreamin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

刚刚发布的 eact 18 这里列出了你需要知道的一切，以及更多！React 的团队已经做出了一些定义性和根本性的改变，这些改变将塑造 React 在这个版本之后的发展道路。其中包括一些重要的变化和许多新的 API。

## **1。安装。**

安装新的 React18 真的很容易。

如果您正在使用`npm`，则:

```
npm install react react-dom
```

或者如果你用的是纱线:

```
yarn add react react-dom
```

你会在你的控制台上看到类似`ReactDOM.render is no longer supported in React 18` 的警告。

## 2.升级客户端渲染 API

上述错误可以在代码更新后得到解决

```
// React v17 and before
import { render } from 'react-dom';
const container = document.getElementById('app');
render(<App/>, container);

// React v18
import { createRoot } from 'react-dom/client';
const container = document.getElementById('app');
const root = createRoot(container);
root.render(<App/>);
```

如果您没有切换到新的 API，即使您安装了最新版本，您的应用程序也将继续运行 React 17。这个新的根 API 允许您享受 React18 的新实现细节，称为**并发 React。**

## 3.并发反应

**Concurrent React** 在 React 内部实现了一个软件开发的基本概念，称为*“并发”。*众所周知，JavaScript 是单线程语言。这意味着，如果你正在执行一个任务，它将阻塞线程，下一个任务不能开始，除非前一个任务已经完成。这就像你的父母让你吃完蔬菜再吃冰淇淋一样。😆

但是并发性允许您将这些任务分成更小的部分，并作为独立的任务无序地执行它们。这意味着，你可以吃 3 勺蔬菜，然后 1 勺冰淇淋，然后 2 勺蔬菜，等等。😜

## 4.渲染— React 17 及之前

在 React 17 之前，更新是在单个不间断的同步事务中呈现的。假设我们的应用程序中有以下功能。

```
const renderPopup = () => { setData(input.data); setLoader(input.isEnabled);}
```

随着同步渲染，一旦`setData`更新开始渲染，没有什么可以中断它，直到用户可以在屏幕上看到结果。只有在此之后，`setLoader`才能执行渲染。

## 5.渲染-反应 18

借助于**并发反应**渲染是可中断的。有了新的并发 API React**可以开始呈现更新，在执行过程中停止，获取更紧急的更新，然后稍后继续。然而，React 保证即使渲染被中断，UI 也将保持一致。这意味着即使在大型渲染任务中，用户界面也可以立即响应用户输入，创造流畅的用户体验。非常类似于在中间放一勺冰淇淋，如果你认为这是一个更紧急的行动。**

## **6.官方特性#1:自动配料**

**状态批处理就是将多个状态更新组合成一个单独的重新渲染，以获得更好的性能。让我们把注意力转移到上面的`renderPopup`函数上。在 React 17 和之前的版本中，这两种不同的状态会一个接一个地触发两个独立的渲染。默认情况下，React 仅在 React 事件处理程序中进行批处理更新，而不在 promises、setTimeout、本机事件处理程序或任何其他事件中进行批处理更新。**

**使用 React 18，状态批处理将在任何处理程序中自动发生，无需任何代码更改。**

```
// Before
setTimeout(() => {
  setName(name);
  setAge(18);
  // React will render twice, one for each state update
}, 300);

// After: updates inside of timeouts, promises,
// or any other event are batched.`
setTimeout(() => {
  setName(name);
  setAge(18);
  // React will only re-render once at the end
}, 300);
```

## **7.如何避免自动配料**

**如果您不希望在代码的某些部分使用批处理，您可以通过使用`flushSync`退出:**

```
import { flushSync } from 'react-dom';function handleClick() {
  flushSync(() => {
    setName(name);
  });
  // React has updated the DOM by now
  flushSync(() => {
    setAge(18);
  });
  // React has updated the DOM by now
}
```

**[](https://reactjs.org/docs/react-dom.html#flushsync) [## 反应

### react-dom 包提供了特定于 dom 的方法，可以在应用程序的顶层使用，也可以作为转义…

reactjs.org](https://reactjs.org/docs/react-dom.html#flushsync) 

## 8.官方功能#2:过渡

React 中有两种类型的更新:

**a .紧急更新** →通过输入、点击或按压触发的 DOM 更新需要在 DOM 上立即响应。

**b .过渡更新→** DOM 更新，如呈现用户可以等待查看的列表或演示组件。这是从一个视图到另一个视图的 UI 转换。

借助 concurrent React，我们可以将紧急更新优先于过渡更新，这样用户就不会感到体验不佳。

## 9.过渡 API

`startTransition` API 让我们通知 React 哪些更新是*紧急*哪些是*转换*:

```
import {startTransition} from 'react';// Urgent: Show what was typed
setInputName(input);// Mark any state updates inside as transitions
startTransition(() => {
  setFilteredNames(input);
});
```

包装在 startTransition 中的状态更新被认为是非紧急的，如果有更紧急的更新进来，就会被中断。如果一个过渡被用户中断，React 将抛出没有完成的陈旧渲染工作，只渲染最新的更新。

你可以使用`useTransition`作为钩子，或者在不能像类组件一样使用`useTransition`的地方使用`startTransition`函数。

## 10.如何实现 useTransition

`useTransition`是一个钩子，它返回`startTransition`函数和一个`isPending`值来跟踪挂起状态。

```
const [isPending, startTransition] = useTransition();
```

`startTransition`可如上例所述使用，而`isPending`可用于显示加载器，指示过渡何时激活或完成。React 在下面提供了一个简单的例子。

[](https://reactjs.org/docs/hooks-reference.html#usetransition) [## 钩子 API 参考-反应

### 钩子是 React 16.8 中的新增功能。它们允许您使用状态和其他 React 特性，而无需编写类。这个…

reactjs.org](https://reactjs.org/docs/hooks-reference.html#usetransition) 

## 11.如何实现 startTransition

```
React.startTransition(callback)
```

`startTransition`易于使用，工作原理与上述完全相同。

[](https://reactjs.org/docs/react-api.html#starttransition) [## 反应顶级 API -反应

### React 是 React 库的入口点。如果从一个标签加载 React，这些顶级 API 可以在…

reactjs.org](https://reactjs.org/docs/react-api.html#starttransition) 

## 12.官方特性#3:对 SSR 的悬念支持

悬疑以前支持用 React.lazy 进行代码拆分。使用 React 18，它在服务器上支持相同的功能。

> 在 React 18 中，我们在服务器上添加了对悬念的支持，并使用并发渲染功能扩展了它的功能。
> 
> React 18 中的悬念与过渡 API 结合使用效果最佳。如果您在过渡期间暂停，React 将防止已经可见的内容被回退替换。相反，React 将延迟渲染，直到加载了足够的数据，以防止出现错误的加载状态。

## 13.官方特性#4:新的 hook - useId

`useId`是一个钩子，用于生成跨服务器和客户端稳定的惟一 id。这个 ID 意味着 HTML 标签的`id`属性，而不是 UUID 生成器。我们可以声明如下:

```
import { useId } from "react";const Name = useId();
```

并按如下方式使用:

```
<div>      
   <label htmlFor={Name}>Name</label>      
   <input type="text" id={Name} name="Name" /> 
</div>
```

`useId`返回类似`:r10:`的字符串

## 14.官方特性#5:新钩子——useDeferredValue

`useDeferredValue`与`startTransition`非常相似，它允许你延迟重新渲染树的非紧急部分。由于没有固定的时间延迟，React 将在第一次渲染反映在屏幕上后立即尝试延迟渲染。延迟呈现是可中断的，不会阻止用户输入。

[](https://reactjs.org/docs/hooks-reference.html#usedeferredvalue) [## 钩子 API 参考-反应

### 钩子是 React 16.8 中的新增功能。它们允许您使用状态和其他 React 特性，而无需编写类。这个…

reactjs.org](https://reactjs.org/docs/hooks-reference.html#usedeferredvalue) 

## 15.官方特性#6:库挂钩

引入了一些像`useSyncExternalStore, useInsertionEffect`这样的钩子，它们应该主要由库作者而不是普通开发人员使用。但是你总是可以多读一些关于他们的书来增强你的理解。

## 16.突破性变化#1:

在这个版本中，React 将放弃对将于 2022 年停止支持的 Internet Explorer 的支持。此外，React now 依赖于现代浏览器的功能，如`Promise`、`Symbol`等。如果您支持旧的浏览器和设备，您应该考虑在您的捆绑应用程序中包含一个全局 polyfill。

## 17.突破性变化#2:

由于所有更新都将被自动批处理，无论它们来自何处，这可能会在您的应用程序中导致意外的行为。总是在产品发布之前进行测试！

## 18.突破性变化#3:

一些重要的反对意见。请检查以下列表。

[](https://reactjs.org/blog/2022/03/08/react-18-upgrade-guide.html#deprecations) [## 如何升级到 React 18 - React 博客

### 正如我们在发布会上分享的，React 18 引入了由我们新的并发渲染器支持的功能，并逐步…

reactjs.org](https://reactjs.org/blog/2022/03/08/react-18-upgrade-guide.html#deprecations) 

# 结论

随着 React 18 在社区中被采用，我们将在这些基础上学习更多。最终，许多最佳实践将会出现，这将帮助我们编写更高效、更干净的代码。在不久的将来，我一定会写一篇同样的文章。如果你认为这篇文章对你有价值，请在 [**中**](https://medium.com/@abhayganjoo) 关注和**订阅**给我！

在这篇文章中，我从 React 文档中引用了许多很棒的片段，所以不要忘记也去看看。此外，还有一些关于增强 React 应用程序性能的优秀文章👇

[](https://blog.bitsrc.io/how-to-make-your-react-application-even-faster-3efe9387cbb1) [## 我优化了我的 React 应用程序，让它变得更快

### 今天，我们将深入探讨记忆化(有例子！).这是一个伟大的技术，帮助我们提高…

blog.bitsrc.io](https://blog.bitsrc.io/how-to-make-your-react-application-even-faster-3efe9387cbb1) [](https://javascript.plainenglish.io/the-abc-of-code-splitting-ebb7060ca704) [## 代码拆分的基础知识

### 代码分割是优化 JavaScript/React 应用程序加载性能的绝佳工具。我们将实现这一点…

javascript.plainenglish.io](https://javascript.plainenglish.io/the-abc-of-code-splitting-ebb7060ca704) [](https://17.reactjs.org/docs/concurrent-mode-intro.html) [## 引入并发模式(实验)- React

### 注意:这一页是关于稳定版本中还没有的实验性特性。它旨在早期…

17.reactjs.org](https://17.reactjs.org/docs/concurrent-mode-intro.html) 

```
**Want to Connect?**LinkedIn : [https://www.linkedin.com/in/abhayganjoo/](https://www.linkedin.com/in/abhayganjoo/)
```