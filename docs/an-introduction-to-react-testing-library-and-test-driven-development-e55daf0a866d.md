# React 测试库和测试驱动开发简介

> 原文：<https://blog.devgenius.io/an-introduction-to-react-testing-library-and-test-driven-development-e55daf0a866d?source=collection_archive---------7----------------------->

![](img/baae26134f240ef495c5748e96f534ed.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

最近，我一直在自学更多关于测试驱动开发的知识，以及如何将它整合到我的前端 React 应用程序中。随着我继续发展我的技能，我想分享一下我到目前为止学到的东西。

测试驱动开发是很多训练营的学生(比如我自己)在他们的学习中经常听到的一个时髦词，但是它意味着什么呢？

测试驱动开发是一种使用测试框架(Mocha、Rspec、Jest 等)为应用程序编写测试的方法。)来测试代码的功能，然后再开发实际的代码。开发人员使用失败的测试来驱动他们如何开发他们的代码。它有一系列的好处，包括在编码前阐明特性目标，证明代码满足必要的需求，帮助调试，减少对及时点击测试的依赖，等等。

在开发 React 应用程序时，一个更专业的工具可以非常有助于以一种可访问的、用户友好的方式准确测试组件。进入 React 测试库。React 测试库是一个工具，它使 React 开发人员能够以非常类似于用户的方式轻松测试他们的组件。它不是一个框架，也不特定于任何框架(尽管它通常与 Jest 一起使用)。使用 create-react-app 时，已经安装了 react 测试库，并创建了 App.test.js 文件。或者，您可以安装它:

```
npm install --save-dev @testing-library/react
```

一旦它被安装并且在 src 文件夹中创建了 App.test.js 文件，您就可以导入库以及 react 和您的 App.js 文件(如果还没有的话)。

```
import React from 'react';import { render } from '@testing-library/react';import App from './App';
```

如你所见，我们正在从测试库中导入我们的应用组件 React 和 render。Render 是一种允许测试像用户一样正确检查组件的方法。既然我们已经进口了所有的东西。你可以写你的第一个测试。

```
test('App renders "Hello, World!"', () => {const { getByText } = render(<App />);expect(getByText("Hello, World!")).toBeInTheDocument();});
```

在上面的代码中，你可以看到我们正在声明一个测试，并给这个测试一个描述我们测试什么的名字。语法非常类似于标准的 Javascript 函数。在下一行，我们获取 getByText 并使用导入中的“render”来呈现我们的应用程序组件。GetByText 允许我们告诉我们的测试在呈现给用户的内容中找到一个特定的文本。最后一行非常易读，这是我们实际测试的地方。我们“期待”那个“你好，世界！”将“在文档中”。因为我们想测试我们是否能找到该文本，所以我们将文本包装在“getByText”中。

现在，如果我们运行我们的测试套件，而没有在 App.js 文件中编写任何代码，我们将会看到测试将会失败。要查看这一点，请运行以下命令:

```
npm test
```

所以我们现在知道，我们希望我们的应用程序呈现“你好，世界！”我们知道它不是。现在，我们可以进入 App.js 文件，尝试通过这个测试。

```
import React from 'react'const App = () => {
   return (
      <div>
        <h1>Hello, World!</h1>
      </div>
}export default App
```

现在，如果我们在终端中再次运行 test 命令，我们将看到它通过了！这是测试驱动开发最简单的例子。

React 测试库非常灵活和友好。它允许你测试你的应用程序应该呈现什么，它可以模拟用户交互，如填写输入，点击按钮，遍历页面，等等！它是一个很棒的库，可以帮助你开发用户如何与你的应用程序交互，我建议你下次开始一个 React 项目时，在构建组件时使用测试驱动开发。