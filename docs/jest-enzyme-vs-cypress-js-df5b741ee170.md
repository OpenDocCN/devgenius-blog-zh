# Jest，Enzyme vs cypress . js——为什么只写 e2e 测试可能更聪明

> 原文：<https://blog.devgenius.io/jest-enzyme-vs-cypress-js-df5b741ee170?source=collection_archive---------0----------------------->

今天我不得不为一些 React.js 组件编写测试，并且想知道使用 Cypress.js 来实现这个目的是不是更聪明。我知道 Cypress.js 旨在成为一个端到端的测试工具，其中 Jest+Enzyme 用于单独测试一个组件。

我认为使用 Cypress.js 有几个好处:

*   测试套件不依赖于前端框架。如果你决定用 Vue.js 替换 React.js，你不必扔掉你的测试套件。相反，您可以使用现有的测试来验证重写的组件。
*   我发现用 Jest 测试用户交互非常麻烦。然而，用户交互是用户界面中棘手的部分，也是错误的主要来源。Cypress.js 旨在测试用户自己进行的交互。
*   Jest and Enzyme 强调一种“[模仿者测试风格](https://medium.com/@adrianbooth/test-driven-development-wars-detroit-vs-london-classicist-vs-mockist-9956c78ae95f)”。这使得重构组件变得更加困难，因为子组件通常是分开进行存根化和测试的。
*   即使使用 Cypress.js，您也可以通过将组件装载到只包含该组件的空应用程序中来只测试组件。另外，Cypress.js 提供了存根 XHR 请求的功能。因此不需要启动并运行后端。

我很乐意听听你对此的想法。留言评论！

最诚挚的问候，

奥利弗，在[工作](https://www.monsterwriter.app/)