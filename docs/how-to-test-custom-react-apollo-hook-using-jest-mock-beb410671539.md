# 如何使用 Jest Mock 测试自定义的 React Apollo 钩子

> 原文：<https://blog.devgenius.io/how-to-test-custom-react-apollo-hook-using-jest-mock-beb410671539?source=collection_archive---------1----------------------->

![](img/379569bf5a057f7872ff552e5e25fbe8.png) [## 用 React cheatsheet 进行 Jest 测试

### 当需要使用 Jest 和 React 编写 JavaScript 测试时，需要参考一些注意事项:*这不是一篇全面的帖子…

www.devdecks.io](https://www.devdecks.io/2021-jest-testing-cheatsheet) 

我坚信编写好的测试是交付高质量产品的一个关键组成部分，并且我知道这并不总是简单明了的。在这篇文章中，我想分享我是如何使用 Jest Mock 函数测试一个用 Apollo 构建的自定义 React 钩子的。我正在使用**打字稿**。

**章节概述:**

1.  关于钩子
    我将在另一篇文章中分享如何使用 Apollo 构建一个定制的 React 钩子。然而，在这篇文章中，我想简单回顾一下这个阿波罗钩子做了什么，以便理解我们正在测试的东西。
2.  **使用模拟函数时 Jest 测试的基本结构/设置** 我将分享**我最喜欢的 Jest 模拟函数类比。**
3.  **测试**
4.  **一些我觉得有用的测试技巧**

# 1.钩子！

钩子是一种函数。在 React 中，作为开发人员，钩子允许我们从组件中提取**有状态逻辑**，以便**重用和独立测试，而不改变组件层次结构**。

现在，看一下这个定制挂钩:

这个定制钩子`yourFunction`使用 Apollo 的`useQuery`，这是一个在 React 应用程序中执行查询的 Apollo 钩子，并返回一个具有值`loading`、`error`和`data`的对象。

我将`yourFunction`定制为返回`loading`、`error`、`isName`，而不是返回`loading`、`error`和`data`的值。

`isName`从第 18 行`useQuery`输出的数据中返回`name`的字符串值。

你可以在阿波罗的网站上了解更多关于`useQuery`的信息:

[](https://www.apollographql.com/docs/react/api/react/hooks/#example-2) [## 钩住

### Apollo Client >= 3 包括现成的 React hooks 功能。你不需要安装任何额外的…

www.apollographql.com](https://www.apollographql.com/docs/react/api/react/hooks/#example-2) 

现在我们已经了解了我们的定制钩子，我们可以开始测试了。

# 2.测试的基本结构/设置

夜幕降临时，狼人挑选他们的受害者

我艰难地学习了 Jest，但是如果你知道 Jest 如何工作和它的基本测试结构，它实际上并不复杂。

在进入实际步骤之前，让我们确保我们知道 Mock 函数是做什么的。

根据 [Jest](https://jestjs.io/docs/mock-functions) ，

> 模拟函数允许您通过**删除函数**、**捕获对函数的调用**(以及这些调用中传递的参数)、**在用`*new*`实例化时捕获构造函数的实例**，并允许返回值的测试时配置，来测试代码之间的链接。

了解 Jest 的工作原理后，我觉得和打 [**狼人**](https://en.wikipedia.org/wiki/Mafia_(party_game)) **差不多。**在游戏中，版主(我一直是这样玩游戏的。这一直是版主的工作……)(1)**给玩家分配角色；**和(2) **协调玩家** **的动作，让玩家完成自己的角色。之后，玩家采取行动。**

当我完成每个步骤时，这将更有意义:

A.首先，我们需要将您的函数导入到测试文件中，就像邀请人们玩狼人游戏一样。

```
import yourFunction from '...'
```

B.类似于版主给玩家分配角色，使用`.mock`让应用程序知道你在用`jest.mock`嘲笑这个功能

模仿函数主要有两种方式:(1)创建一个模仿函数实例；(2)写一个`[*manual mock*](https://jestjs.io/docs/manual-mocks)`来覆盖一个模块依赖。

对于第一种方法，我们需要为 *yourFunction* 定义*类型*。

```
let yourFunction: jest.Mock;
```

如果您以上述方式定义*类型*，您将需要在以后声明`yourFunction`的用途，如下所示:

```
yourFunction = jest.fn()
```

注意`jest.fn()`方法默认返回`undefined`。我们可以通过不同的[模拟函数](https://jestjs.io/docs/mock-function-api)来控制返回值。我将在这篇文章的后面讨论这一部分。

前面讨论的第二种方法是模仿一个模块。这是我们将要用来实现测试的方法。

请注意，第一个参数`jest.mock('./moduleName')`是必需的。

```
*jest*.mock('file directory of yourFunction module', () => ({ yourFunction: *jest*.fn(),}));
```

还要注意它使用了隐式返回。这是用`yourFunction: jest.fn()`返回一个对象。*键*为`yourFunction`，*值*为`jest.fn()`。

除了上述方法，您还可以这样做:

```
jest.mock('file directory of your function');
```

这也可以写成:

```
jest.mock(‘file directory of your function’, ()=> jest.fn());
```

注意，第二个参数是可选的，但默认为`jest.fn()`:

```
jest.mock('file directory of yourFunction module', () => () => [      jest.fn(),  { someKey: true },]);
```

*C.* 现在游戏正在进行中，玩家们正全力投入到他们被分配的角色中。从这里开始，玩家将根据他们的角色实施不同的策略，并开始**采取行动**。

以同样的方式*，*你需要确切地定义函数如何模仿以及为了什么目的。 [Jest](https://jestjs.io/docs/mock-function-api) 将模拟函数描述为"*间谍*"，因为它们允许我们"*窥探被其他代码间接调用的函数的行为，而不仅仅是测试输出。*

```
(yourFunction as *jest*.*Mock*).mockImplementationOnce(() => result);
```

类似于狼人如何伪装成村民，这些模拟函数可以改变返回值和实现。

在上面的代码示例中，我使用了`mockImplementationOnce()`。您可以使用不同的模拟方法。

# 3.测试:将不同的例子放在一起:)

完成了！

# 4.一些提示

(a)在`beforeEach`功能中设置模拟功能。

(b)如果你的结构不正确，有时模仿一个函数/模块会产生错误，污染其他测试。每个函数都应该有一个新的设置。通过在每个测试规范之后清除模拟，确保您的代码没有“泄漏”:

```
afterEach(()) => {
   jest.clearAllMocks();
});
```

当你鼓掌时👏我的帖子和订阅，你支持我的科技之旅，我将不胜感激:)

谢谢，祝编码愉快！