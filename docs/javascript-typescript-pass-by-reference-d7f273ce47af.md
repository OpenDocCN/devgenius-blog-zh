# Javascript/Typescript 通过引用传递

> 原文：<https://blog.devgenius.io/javascript-typescript-pass-by-reference-d7f273ce47af?source=collection_archive---------1----------------------->

## 为 Javascript/Typescript 提供类似 C 的宏函数

![](img/0a8fded8d0b2e1387136344bc6eaab4e.png)

图片来自 Pixabay.com

## 介绍

大家好，我的名字是 Marcelo Mancini，又名 Hipreme/MrcSnm，我主要是一名游戏开发人员，我一直在为开源社区编写大量代码。

## 摘要

一个插件，它将启用 Javascript/Typescript 代码中定义的宏函数。这样，你可以在脚本中传递引用变量，然后，你可以在任何地方调用宏函数。它的工作方式很像 C，它会遵循一些规则进行智能文本替换。

## 它是如何工作的

这是一个 webpack 加载器，它使用 saves 宏定义。定义总是从一个文件导入另一个文件。

要添加到您的配置中，只需:

`{ test: /\.(t|j)s$/, use: [{ loader: path.resolve('../define_macro-loader.js')}]}`

要在代码中应用它，只需调用以下代码:

你可以注意到，它在第一次是相当奇怪的。因为我们设置的是 v，应该是复制的值。

发生这种情况是因为这不是复制的值，而是传递给函数的实际值，因此，对“v”参数的任何更改都会改变传递给函数的参数，这样，我们就能够创建不复制值的函数。

在`DEFINE`调用之后，你将能够调用你定义的变量，在那种情况下是`GetRef`。

用简单的就地替换来替换任何“返回”调用是足够聪明的。代码输出将是:

所以这几乎是对任何代码片段的简单复制和粘贴，允许在 javascript/typescript 中使用变量引用。这让你想起指针了吗？GetRef(variable)的工作方式与&variable 非常相似

其他一些简单的例子:

## 催单

*   不能在第一个文件中使用其他地方定义的宏。如果你想使用它，你需要建立一个间接层来使用它们，例如:index.ts 需要“宏”，然后，你的 app.ts 将能够使用在 macros.ts 中定义的宏
*   如果您进行任何类型的变量操作，它将为插入任何操作创建一个新的范围

## 交互工作的推荐插件

*   [Babel Preval Plugin](https://www.npmjs.com/package/babel-plugin-preval) ，原因:你可以创建宏来获取文件名并在编译时对其进行预评估。preval 对于宏变得更加有用，查看它在 macro _FILENAME_NO_EXT 上的用例。如果您检查输出，您会看到它快了多少。
*   [Typescript Nameof Plugin](https://www.npmjs.com/package/babel-plugin-ts-nameof) ，原因:你可以将你的变量名保存为一个字符串。这个插件实际上是由宏授权的，因为你通过引用而不是值来获得变量，所以名字不会丢失。

## 特征

*   不接受参数宏(但括号仍然是必要的)
*   宏接受多个变量。这样，我们就有可能:

> 是的，有了新的宏，我们可以进行就地交换。

*   在 define 中定义函数的可能性。例如:

这样，Test 将获得方法 toString()，该方法将返回“Test”，不仅如此，您还可以为不同的类定义多少样板代码，并使用传递的参数对这些样板代码进行参数化，从而提高您的生产率。

## 酷！我想尝试一下

只需找到您的 package.json 并编写以下代码:

`npm i -D @hipreme_entertainment/define_macro-loader`

*学习是我的生活方式，马塞洛·席尔瓦·纳西门托·曼奇尼*

[*Github*](https://www.github.com/MrcSnm)

[*领英*](https://www.linkedin.com/in/mrcsnm)

*Hipreme/MrcSnm*