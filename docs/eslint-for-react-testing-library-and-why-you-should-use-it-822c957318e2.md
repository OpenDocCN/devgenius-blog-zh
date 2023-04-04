# React 测试库的 ESLint 和为什么你应该使用它！

> 原文：<https://blog.devgenius.io/eslint-for-react-testing-library-and-why-you-should-use-it-822c957318e2?source=collection_archive---------5----------------------->

React 测试库用于在 React 应用中编写单元测试。React 测试库侧重于可访问性，这意味着我们更像真实用户一样测试功能。

要编写优秀的单元测试，需要遵循一些最佳实践。为了确保我们用最佳实践编写单元测试，我们应该使用 ESLint 来帮助我们实现最佳实践。

![](img/55f0f473f25b96964f85eb9d8e80fd4a.png)

照片由[你好我是尼克](https://unsplash.com/@helloimnik?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 埃斯林特

ESLint 静态地分析代码库，以发现问题或没有遵循最佳实践的代码。静态分析的代码库帮助我们使代码库在团队成员之间保持一致。

ESLint 可以配置的一些规则示例:

*   没有影子变量命名。
*   没有未使用的变量。
*   在定义变量和方法之前没有用。
*   React 组件中没有无用的碎片。
*   React 组件内没有道具传播。
*   在 TypeScript 中没有使用任何类型。
*   在 TypeScript 中强制返回类型。

上面的列表只是数百个可配置规则中的几个。将 eslint 与规则结合使用的好处是确保团队中的所有开发人员都遵循相同的模式。这样做的结果是一个一致的代码库，更易于维护和接纳新的团队成员。

我不打算在本文中详细解释 ESLint。请在 https://eslint.org/docs/latest的文档中阅读更多关于 ESLint 的信息。

# React 测试库的 ESLint

ESLint 有一个测试库插件，可以帮助开发人员遵循最佳实践并预测编写单元测试时的常见错误。

***est lint-plugin-testing-library****包括以下内容。*

*   *林挺测试库特定代码的若干规则。*
*   *推荐规则的可共享配置。*
*   *通过框架 Angular、React 和 Vue 共享特定规则的配置。*
*   *一些可自动修复的规则。*

## *React 测试库的推荐配置*

*Testing Library eslint-plugin 导出了几个可共享的配置来实施最佳实践。每个可共享的配置都属于一个框架。在本文中，我们将研究 ***插件:testing-library/react。****

*让我们看看从插件:testing-library/react 导出的 React-Testing-Library 的 19 条推荐规则中的 7 条。*

## ***1。测试库/等待异步查询***

*该规则确保开发人员正确处理异步查询的承诺。请看下面的例子:*

```
*// async query without handling the promise.
test('should display button with text-value "Save"', () => {
  const button = findByRole('button');
  expect(button).toHaveTextContent("Save");
})

// use the await operator to handle the promise
test('should display button with text-value "Save"', async() => {
  const button = await findByRole('button');
  expect(button).toHaveTextContent("Save");
})*
```

*请随意在文档中阅读有关该规则的更多信息，[https://github . com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/await-async-query . MD](https://github.com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/await-async-query.md)*

## ***2。测试库/等待异步实用程序***

*该规则确保 utils 返回的承诺得到正确处理。测试库提供了多个 utils 方法，如 waitFor 和 waitForElementToBeRemoved。*

```
*// incorrect because we are not handling the promise return by waitFor
waitFor(() => getByLabelText('First name'));

// use the await operator to handle the promise correctly
await waitFor(() => getByLabelText('First name'));*
```

*请随意阅读更多关于规则的文档，[https://github . com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/await-async-utils . MD](https://github.com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/await-async-utils.md)*

## ***3。测试库/无等待同步查询***

*该规则确保 await 操作符不会等待同步查询。像 getBy*、getByAll*、queryBy*和 queryAllBy*这样的方法是同步的，不需要 await 操作符。*

```
*// incorrect because we are using await on synchronous methods.
const firstNameInput = await getByLabelText('First name');

// correct because we are not using await on synchronous methods.
const firstNameInput = getByLabelText('First name');*
```

*可能会混淆哪些方法是同步的，哪些是异步的，因此这条规则将非常有助于避免错误。*

*请随意在文档中阅读关于该规则的更多信息，[https://github . com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/no-await-sync-query . MD](https://github.com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/no-await-sync-query.md)*

## ***4。测试库/无容器***

*该规则确保避免使用容器，因为真实用户可能无法访问您正在查询的元素。通过使用容器，我们能够通过 CSS 类查询 DOM 元素，这样做破坏了可访问性测试。CSS 类名更改时，单元测试可能会更频繁地中断。*

```
*// Using contianer is incorrect
const page = render(<MyPage/>);
const button = page.container.getElementByClassName('primary-button');

// Correct
render(<MyPage/>);
const button = screen.getByRole('button', { name: 'Save' });* 
```

*请随意在文档中阅读有关该规则的更多信息，[https://github . com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/no-container . MD](https://github.com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/no-container.md)*

## ***5。测试库/无调试工具***

*该规则确保不提交调试实用程序。调试工具只是用于调试测试，因此不应该被推送到代码库中。调试工具会污染测试输出。*

```
*// Codeexample for debugging
const { debug } = render(<MyPage />);
debug();*
```

*请随意在文档中阅读关于该规则的更多信息，[https://github . com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/no-debugging-utils . MD](https://github.com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/no-debugging-utils.md)*

## ***6。testing-library/no-DOM-import***

*确保没有来自`@testing-library/dom`或`dom-testing-library.`的直接进口，进口应为`@testing-library/react`。*

```
*// Incorrect
import { fireEvent } from 'dom-testing-library';

// Correct
import { fireEvent } from 'testing-library/react';*
```

*请随意在文档中阅读有关该规则的更多信息，[https://github . com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/no-DOM-import . MD](https://github.com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/no-dom-import.md)*

## ***7。测试库/无节点访问***

*此规则确保不允许使用本机 HTML 方法和属性(如 parentElement、children、lastChild 和 HTML 树中节点元素的所有其他属性)遍历 DOM。*

```
*// Incorrect because the test is acccessing .lastChild which is a node elment.
const buttons = screen.getAllByRole('button');
expect(buttons.lastChild).toBeInTheDocument();*
```

*请随意在文档中阅读关于规则的更多信息，[https://github . com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/no-node-access . MD](https://github.com/testing-library/eslint-plugin-testing-library/blob/main/docs/rules/no-node-access.md)*

## *所有推荐的规则*

*要查看为 testing-library/react 导出的所有 19 个规则，请访问源代码，[https://github . com/testing-library/eslint-plugin-testing-library/blob/main/lib/configs/react . ts](https://github.com/testing-library/eslint-plugin-testing-library/blob/main/lib/configs/react.ts)*

## *结论*

*应该在所有团队中使用 ESLint 测试库，以确保单元测试遵循最佳实践，并避免错误导致不可靠的测试。通过使用 ESLint，团队可以确保代码库语法和模式在开发人员之间保持一致。结果是可靠的测试和可维护的代码库。*

*请随意鼓掌、分享或评论！:)*