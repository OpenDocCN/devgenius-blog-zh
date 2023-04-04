# 早期阶段的顶级 JavaScript 特性

> 原文：<https://blog.devgenius.io/top-starred-javascript-features-at-early-stage-982de948f2ae?source=collection_archive---------7----------------------->

![](img/bfa5af9edff48226dfeb52669fa0232a.png)

JS 是不断发展的，在这篇文章中，我想谈谈仍处于概念和最初提议阶段的内容。但是社区已经欣赏了这些创新，并期待着它们。

JS 有几个级别的新特性批准。在本文中，我将讨论 0 级和 1 级的特性。

# 第一阶段提案

第一级有以下目的:

*   提出增加的理由
*   描述溶液的形状
*   识别潜在挑战

让我们来看看最令人期待的第 1 阶段功能:

## 1.管道运营商

这个提案引入了一个类似于 [F#](https://en.wikibooks.org/wiki/F_Sharp_Programming/Higher_Order_Functions#The_.7C.3E_Operator) 、 [OCaml](http://caml.inria.fr/pub/docs/manual-ocaml/libref/Pervasives.html#VAL%28%7C%3E%29) 、[elixin](https://hexdocs.pm/elixir/Kernel.html#%7C%3E/2)、 [Elm](https://package.elm-lang.org/packages/elm/core/latest/Basics#%7C%3E) 、 [Julia](https://docs.julialang.org/en/v1/base/base/#Base.:%7C%3E) 、 [Hack](https://docs.hhvm.com/hack/expressions-and-operators/pipe) 和 [LiveScript](http://livescript.net/#piping) 的新操作符`|>`，以及 UNIX 管道和 [Haskell](https://hackage.haskell.org/package/base-4.12.0.0/docs/Data-Function.html#v:-38-) 的`&`。这是一种向后兼容的方式，以一种可读的、功能性的方式来简化链式函数调用，并为扩展内置原型提供了一种实用的替代方法。

管道操作符本质上是一个有用的语法糖，用于带有单个参数的函数调用。换句话说，`sqrt(64)`相当于`64 |> sqrt`。

当将几个函数链接在一起时，这允许更大的可读性。例如，给定以下函数:

更多用例:[https://github . com/tc39/proposal-pipeline-operator/wiki/Example-Use-Cases](https://github.com/tc39/proposal-pipeline-operator/wiki/Example-Use-Cases)

阅读更多关于提案的信息:[https://github.com/tc39/proposal-pipeline-operator](https://github.com/tc39/proposal-pipeline-operator)

## 2.模式匹配

这个提议在现有的[析构绑定模式](https://tc39.github.io/ecma262/#sec-destructuring-binding-patterns)的基础上，给语言增加了一个模式匹配表达式。

示例:

更多阅读:【https://github.com/tc39/proposal-pattern-matching 

## 3.可观察量

这个提议为 ECMAScript 标准库引入了一个可观察类型。Observable 类型可用于建模基于推送的数据源，如 DOM 事件、计时器间隔和套接字。此外，可观察到的有:

*   *组合*:可观测量可以用高阶组合子组合。
*   *懒惰*:直到观察者订阅后，可观察对象才开始发送数据。

**示例:观察键盘事件**

使用 Observable 构造函数，我们可以创建一个函数，为任意 DOM 元素和事件类型返回一个可观察的事件流。

然后，我们可以使用标准的组合子来过滤和映射流中的事件，就像我们处理数组一样。

*注:本建议书不包括“过滤”和“映射”方法。它们可能会被添加到本规范的未来版本中。*

当我们想要使用事件流时，我们向观察者订阅。

subscribe 返回的对象将允许我们随时取消订阅。一旦取消，可观察对象的清除功能将被执行。

更多阅读:[https://github.com/tc39/proposal-observable](https://github.com/tc39/proposal-observable)

## 4.部分应用程序语法

这个提议引入了一种新的语法，在参数列表中使用了`?`标记，这允许您通过充当参数的占位符，将参数列表部分地应用于调用表达式。

示例:

更多阅读:[https://github.com/tc39/proposal-partial-application](https://github.com/tc39/proposal-partial-application)

## 5.二元 AST

提出了一种基于 JavaScript 语法的高效抽象语法树表示的二进制编码。这是表面语法的一种新的替代编码，与文本表示的双向映射相当接近。新语义目前仅限于:

*   推迟早期错误
*   改变`Function.prototype.toString`行为
*   需要 UTF-8

为了进一步加快解析速度，建议将静态语义编码为实现提示，并将它们验证为延迟断言。

对于这种 AST 格式，没有建议从 ECMA-262 语法自动导出，因为它太冗长而不是有效的树表示，并且同样地，它需要大量的展平和内联。此外，通过有效地使语法成为二进制格式的标准规范，这可能会限制 JavaScript 规范的发展。

取而代之的是，提出了一个单独的树语法，在 AST 节点上的注释只表达关于语法的信息。有几种现有的树语法被作为起点，比如 Babylon 或 Shift AST。

借鉴 WebAssembly 方法，二进制编码将分为 3 层:

1.  使用基本原语(例如，字符串、数字、元组)对 AST 节点进行简单的二进制编码
2.  在前一层的基础上进行额外的结构压缩，利用关于文件格式和 AST 的性质的知识(例如，常数表)
3.  像 gzip 或 Brotli 这样的通用压缩算法。

预计这种格式将由现有的编译器(如 Babel 和 TypeScript)以及捆绑器(如 WebPack)输出。

更多阅读:[https://github.com/tc39/proposal-binary-ast](https://github.com/tc39/proposal-binary-ast)

## 6.内置模块

为了使开发者能够访问 JavaScript 引擎提供的通用功能，建议创建内置于主机中的模块。这将允许与程序捆绑在一起的库代码转向在主机 JavaScript 实现上可用。

在运行时有一个现成的标准库意味着程序不必包含标准库中可用的功能，从而降低了程序的下载和启动成本。该功能也将跨实现进行标准化，使开发人员在质量、行为和速度上保持一致。对于一些 JavaScript 引擎，内置模块将允许它们减少应用程序内存占用

虽然 JavaScript 引擎已经有了通过全局对象的公共库的概念，但是可以使用当前的异步`import()` API 来访问标准库组件。模块脚本也可以使用`import`语法访问内置的库组件。传统的脚本代码将能够通过一个新的同步 API`importNow()`访问这些相同的标准库组件。开发人员应该已经熟悉了这些机制中大多数，允许他们毫不费力地选择使用这个内置的库功能。

标准库的大部分模块应该能够用普通的 JavaScript 编写，但是对于热门代码，引擎可以提供本地实现。

更多阅读:【https://github.com/tc39/proposal-built-in-modules 

## 7.`do`表情

以面向表达式的方式编写，尽可能局部地限定变量的范围:

使用条件语句作为表达式，而不是笨拙的嵌套 ternaries:

特别适合像 JSX 这样的模板语言:

更多阅读:[https://github.com/tc39/proposal-do-expressions](https://github.com/tc39/proposal-do-expressions)

## 8.切片符号

切片符号为 Array.prototype、TypedArray.prototype 等上出现的各种切片方法提供了一种符合人体工程学的替代方法。

这种表示法可以用于像 Array 和 TypedArray 这样的原语上的切片操作。

更多阅读:[https://github.com/tc39/proposal-slice-notation/](https://github.com/tc39/proposal-slice-notation/)

## 9.UUID 标准图书馆

[JavaScript 标准库](https://github.com/tc39/proposal-javascript-standard-library) UUID 描述了一个基于 [IETF RFC 4122](https://tools.ietf.org/html/rfc4122) 的用于生成字符编码通用唯一标识符(UUID)的 API，可在 JavaScript 引擎中导入。

UUID 标准库提供了一个用于生成 RFC 4122 标识符的 API。

最初支持的 UUID 库的唯一导出是`randomUUID()`，这是一种实现[版本 4“从真随机数或伪随机数创建 UUID 的算法”](https://tools.ietf.org/html/rfc4122#section-4.4)，并返回字符串表示形式*(如 RFC-4122 中所述)*的方法。

更多阅读:[https://github.com/tc39/proposal-uuid](https://github.com/tc39/proposal-uuid)

还有更多有趣的第一阶段提案，在这里阅读它们—[https://github . com/tc39/proposals/blob/master/Stage-1-proposals . MD](https://github.com/tc39/proposals/blob/master/stage-1-proposals.md)

# 阶段 0 建议

阶段 0 允许输入到规范中。在第 0 级，只有新功能的概念。但有 2 个值得关注。

让我们来看看它们:

## 1.这种绑定语法

这个提议引入了一个新的操作符`::`，它执行`this`绑定和方法提取。

更多阅读:[https://github.com/tc39/proposal-bind-operator](https://github.com/tc39/proposal-bind-operator)

## 2.嵌套的`import`声明

这个提议主张放松当前的限制，即`import`声明只能出现在模块的顶层。

具体来说，这项提案将允许`import`宣布

1.  可在函数和块中嵌套，支持多个新的有价值的`import`习惯用法；
2.  提升到封闭范围的开始；即*陈述性*而非*命令性*；
3.  词法上作用于封闭块；
4.  同步评估，对运行时模块获取具有清晰和可管理的结果；和
5.  向后兼容现有顶级`import`声明的语义。

目前，对于`export`声明的位置和语义没有提出任何修改。在作者看来，对于 ECMAScript 模块系统的许多静态属性来说，将所有的`export`声明保持在顶层仍然很重要。

更多阅读:[https://github.com/benjamn/reify/blob/master/PROPOSAL.md](https://github.com/benjamn/reify/blob/master/PROPOSAL.md)

还有更多第 0 阶段的提议，在这里阅读它们—[https://github . com/tc39/proposals/blob/master/Stage-0-proposals . MD](https://github.com/tc39/proposals/blob/master/stage-0-proposals.md)

感谢阅读！

P.S .如果你想了解更多最期待的功能——关注我！