# React Turbo —优化 React 性能的终极解决方案

> 原文：<https://blog.devgenius.io/react-turbo-the-ultimate-solution-to-optimize-react-performance-b666ca9db0b5?source=collection_archive---------0----------------------->

> 我在脸书做前端工程师，创建了大约 4000 颗星星

# TL；速度三角形定位法(dead reckoning)

[React Turbo](https://github.com/oney/react-turbo) 在编译时自动优化 React 性能，所以**开发者不需要手动做优化。**

# 反应的痛苦

反应是可爱的发展。JSX 比模板好。钩子对于业务逻辑的隔离和组合是非常强大的。

React 几乎拥有最好的开发者体验(DX)，除了一点:性能优化。

## 变化检测/反应

我们不得不承认[反应是*从不*反应](https://twitter.com/johnlindquist/status/1109498707475488768)。React 的*变化检测(或反应性)*策略基于 **diff(值比较)**。相比之下，Vue、Angular 和 Svelte 基于**价值观察/订阅**。

## 价值比较

*值比较*表示:

1.  我们需要*记忆并稳定*到**的值，避免*不相关的*重新计算**；道具**避免*无关*重新渲染**。
2.  我们无法自动实现**细粒度的反应性**，因为 React 中的*最小级别的反应性*是组件(不是值或元素)。

问题 1。就是为什么我们需要`useMemo`、`useCallback`、`React.memo`甚至是不可变的数据。我们修复问题 2。通过订阅容器，如[的`<Controller render={( => />`反应-挂钩形式](https://react-hook-form.com/)。

所以，当我们遇到以上 2 个性能问题时，我们不得不添加`useMemo`、`useCallback`或者手动用`<Controller />`、**包裹**。那太可怕了。

# 反应涡轮增压

[React Turbo](https://github.com/oney/react-turbo) 是一个 babel 插件，在**编译时**将您的 React 组件 ***的变更检测策略从值比较转换为值订阅*** 。所以变成细粒度的反应性，缓存所有值，避免不必要的重新计算和重新渲染。

安装 React Turbo 后，不需要写任何`useMemo`、`useCallback`甚至可以用 Redux 用[重选](https://github.com/reduxjs/reselect)。实际上，您不必修改/编写任何代码来优化性能。

## 装置

请查看 React Turbo repo 中的[安装说明。](https://github.com/oney/react-turbo)

# vs .反应忘记

听起来像是[反应过来忘了](https://www.youtube.com/watch?v=lGEMwh32soc)，嗯？

不同的是，遗忘仍然是基于价值观的比较。

React Turbo 是 V*value Subscription*，可以在**值计算和元素渲染**中实现*细粒度的反应性*。
和 React 忘记仍然不能实现细粒度的反应性。它能回忆起一切。举个例子:

## 价值比较与价值订阅

假设你在组件 A 中有一个值`text`，你把`text`从 A 传递给组件 B，B 传递给 C，C 传递给 D，…，最后 Y 传递给 Z，然后，Z 中的一个`<input value={text}/>`使用`text`。

当`text`改变时，即使使用 React Forget，组件 A，B，C，…和 Z 都会重新渲染。
这意味着组件 A 到 Z 中的所有变量和 React 元素都被**重新初始化并重新创建以进行比较**，即使大部分值是相同的(因为只有`text`改变)。

## 开销

重建和比较的**开销**(例如，Vdom diff、useMemo、React.memo 或 create selector of re select**)**相同值的变量和 React 元素当你有一个像脸书广告管理器网站这样的大型应用程序时，会损害性能(是的，我曾在那个代码库上工作过，这很痛苦，也很愉快😉).

特别是当`text`这样的值频繁变化时，例如每次输入 100-200 毫秒，甚至每次拖动滑块(`<input type="range"/>`)和颜色选择器 33 毫秒，用户会感觉到滞后。

使用 React Turbo 即价值订阅，当`text`改变时，只有 Z 中的`<input/>`重新渲染(Z 不会重新渲染)，因为只有那个`<input/>` **订阅** `text`。

**不进行比较。**

这就是我们所说的**细粒度反应性**。

# 它是如何工作的

基本上，它将*状态* *(* 值来自`useState` *)* 从普通的 JavaScript 值转换成可观察/可订阅的信号/原子，如[反冲](https://recoiljs.org/)、 [zustand](https://github.com/pmndrs/zustand) 和[效应器](https://effector.dev/)。

然后自动收集组件中*变量声明*的依赖关系，生成计算原子(比如`useMemo`，其中 linters 提示缺少依赖关系)。

它还用订阅容器*及其依赖关系*包装每个`React.createElement`*(以实现对**元素**的细粒度反应)。*

所有这些都在编译时被转换。

# 对比 MobX

不同之处在于 MobX 的值是可变的，订阅在运行时发生并决定**。**

React Turbo 的值仍然是不可变的，它在并发模式下工作，值订阅在编译时决定。

# 将来的

React Forget 是在编译时优化性能的伟大举措，但值订阅绝对比值比较有更小的开销和更好的性能。所以我希望 React 核心团队能够采用 React Turbo 的理念。

# 注意

1.  React Turbo 编译的组件/钩子可以与 React Turbo 编译的**而不是**的组件/钩子一起工作。所以我们不需要编译任何第三方库来运行 React Turbo 应用。
2.  React Turbo 应用的捆绑包大小不会呈指数级增长。通常情况下，它只会增加 10-20%，就像您通过添加`useMemo`、`useCallback`手动优化一样。
3.  React Turbo 中的可观测原子现在是基于[效应器](https://effector.dev/)的。
4.  React Turbo 可以轻松解决`useEvent`想要解决的性能问题。React Turbo 的回调仍然会改变并导致订阅组件重新渲染，但是**因为它是细粒度的**，所以不会影响性能。