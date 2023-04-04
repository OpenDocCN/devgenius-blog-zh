# React 组件的最佳文件结构

> 原文：<https://blog.devgenius.io/the-best-file-structure-for-your-react-components-d352b20efd?source=collection_archive---------1----------------------->

# 组织重要吗？

罗伯特·马丁有一个组织图书馆的好例子。

如果你的桌子上有少量的书，你可以把它们放在一起，你总是可以在几秒钟内找到你需要的书，而不需要任何系统。

![](img/68f18c2e057dcaa58eb929f220a2d71f.png)

然而，随着系统复杂性的增长，找到所需信息变得越来越困难。

![](img/59e91476313cf9b19b7152f14019ee56.png)

在这样一堆书里，没有人能找到任何东西，因为没有系统。新手和专业人士之间没有太大的区别，每个人都会花大量的时间去寻找一些东西。

为了控制复杂性，你需要一个组织系统。当然，这是有代价的。你必须花相当多的精力来整理你的图书馆。

![](img/b2d56bfac74799f9600d94d4b67d3ca6.png)

即使组织系统准备好了，使用起来也不容易。新手不能只是来找一本书，他们需要花一些时间学习组织方案。但是图书管理员会在几秒钟内找到所要的书。

这是组织代码库的一个很好的例子。你需要一直组织你的代码吗？简单的回答是否定的。这取决于你的系统的大小。

如果你正在开发一个需要 1000 行代码的应用程序，答案是否定的。你可以把所有的事情都实现为一个单一的函数，你会很开心的。但是当你的代码库增长时，你需要开始考虑组织方案。

它不仅与文件结构有关，还与组织组件、类、函数以及用于构建应用程序的任何其他东西的方式有关。

好的。我承诺为 React 组件提供最好的文件结构。在这里。

# 最佳文件结构

我们来谈谈 React 组件。没有 ES6 导入也可以使用 React 组件，但是我希望你不要这样做。所以这个建议是基于 ES6 导入的，幸运的是，这是 React 世界中的默认做法。

当我们创建一个 React 组件时，我们几乎可以在每个组件中找到几个公共部分。我强调接下来的部分:钩子、实用程序、常量、组件和类型。

## 组件模块

首先，让我们定义一些关于保存组件本身的规则。每一个大的组件都应该放在一个单独的目录中，目录的名称应该与组件的文件名相同。但是您不应该直接导入文件，最好创建一个索引文件并从那里导入您的组件。所以它应该是这样的:

```
[ComponentName]:
 - ComponentName.tsx
 - index.ts
```

相关的导入将如下所示:

```
import ComponentName from ‘./ComponentName’;
```

它提供了导入组件的自由，而无需了解内部组件的组织。

## 钩住

钩子的发明是为了简化开发者的生活。所以把钩子移到单独的模块是原生的。我们有一个规则，如果钩子超过 30 行代码，最好把它移到一个单独的模块中。如果您将一个钩子移动到一个模块中，请为每个模块保留一个钩子，并使用钩子的名称作为文件名。因此，建议在组件的文件夹中创建一个单独的文件夹“钩子”,并将所有大钩子保存在那里。文件结构如下所示:

```
[ComponentName]:
 - ComponentName.tsx
 - index.ts
 - [hooks]:
   - useThis.ts
   - useThat.ts
   - index.ts
```

在钩子目录中，我们应该创建一个索引文件，并从这个入口点导入每个钩子。你不应该在组件之外导入钩子。如果你需要这个，这意味着架构中有问题，你需要把你的钩子移到更高的层次。

## 实用工具

当你构建一个应用程序的时候，你总会有一些可以重用的小代码。有时组件逻辑的重要部分可以移到 utils，在这种情况下，最好说它是帮助器。我会把它们描述为纯粹的逻辑，没有任何使用框架的知识。在这样的函数中保持业务逻辑是一个很好的实践。这样，逻辑将独立于框架，编写测试也很容易。

如果你只有几个工具。您可以在组件文件夹的根目录下创建一个 utils.ts 文件，这就足够了。

```
[ComponentName]:
 - ComponentName.tsx
 - index.ts
 - utils.ts
```

但是如果您的 utils.ts 文件在增长，最好将其分成子模块。在这种情况下，结构将如下所示:

```
[ComponentName]:
 - ComponentName.tsx
 - index.ts
 - [utils]:
   - mathUtils.ts
   - geoUtils.ts
   - index.ts
```

同样，我们只通过索引文件导入实用程序。它允许我们保持重构 utils 模块的能力，而无需在其他文件中做任何修改。

有时，其他组件也需要实用程序。在这种情况下，我们不应该将一个组件的工具导入到另一个组件中。最好把它们移到顶层。例如:

```
[ComponentName]:
 - ComponentName.tsx
 - index.ts
 - [utils]:
   - mathUtils.ts
   - geoUtils.ts
   - index.ts
[AnotherComponent]:
 - AnotherComponent.tsx
 - index.ts
 - [utils]
   - commonUtils.ts
   - index.ts
```

## 常数

关于常量有一个主要的规则，即它们应该在靠近使用它们的地方定义。我认为它不仅与常数有关。但出于某种原因，开发人员通常认为常量应该在应用程序的顶层定义。

所以关于常量的方法和 utils 是一样的。如果您有几个 constants.ts 文件，则创建一个单独的 constants . ts 文件；如果您有一堆 constants . ts 文件，则创建一个单独的文件夹:

```
[ComponentName]:
 - ComponentName.tsx
 - index.ts
 - [constants]:
   - errorConstants.ts
   - config.ts
   - index.ts
```

我想你已经明白，如果你需要一个组件之外的常量，这意味着它应该被放在一个更高的层次上。

## 类型

哦，我的上帝，没有类型我们怎么生活？类型很好。但是如何组织它们呢？如果你的类型只在一个地方使用，你可以把它放在同一个模块里，不要在意任何组织逻辑。但是一旦导出了类型，就应该将它移动到最近的 types.ts 文件中。然后，您可以从与命名导出相同的索引文件中导出该类型。

```
[ComponentName]:
 - ComponentName.tsx
 - index.ts
 - types.ts
```

因此，类型的导入将如下所示:

```
import ComponentName, {ComponentNameProps} from ‘./ComponentName’;
```

## 成分

什么？组件内部的组件？是的。当你的组件增长时，你需要把它分成几个部分。如果拆分组件仅在父组件内部使用。有这样的结构真好:

```
[ComponentName]:
 - ComponentName.tsx
 - index.ts
 - types.ts
[components]:
 - ChildComponent.tsx
 - AnotherChildComponent.tsx
```

这里是建议中最重要的部分——如果你孩子的组件变大了，你可以使用递归方法进行内部组织。子组件只是重复相同的结构。它可以有自己的实用程序、常量、挂钩等。

```
[ComponentName]:
 - ComponentName.tsx
 - index.ts
 - types.ts
 - [components]:
   - [ChildComponent]:
    - ChildComponent.tsx
    - index.ts
    - utils.ts
    - constants.ts
    - types.ts
   - [AnotherChildComponent]:
    - index.ts
    - AnotherChildComponent.tsx
```

这种结构允许可伸缩的代码基础，易于维护、重构和重用复杂系统中的组件。

## 试验

在这样的结构中，在哪里放置测试呢？您应该在应用程序的顶层编写所有测试吗？没有。还是遵循同样的“接近原则”比较好。每个组件都应该有自己的 **__tests__** 文件夹，用于与特定组件及其实用程序相关的测试。像这样:

```
[ComponentName]:
 - [__tests__]:
   - ComponentName.test.tsx
   - utils.test.ts
 - ComponentName.tsx
 - utils.ts
 - index.ts
```

在过去的几年里，这种方法已经证明了它对大型系统的适用性。我希望你也会发现它很有用。