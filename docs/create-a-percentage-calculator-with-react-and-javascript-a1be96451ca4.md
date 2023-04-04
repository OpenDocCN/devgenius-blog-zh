# 用 React 和 JavaScript 创建一个百分比计算器

> 原文：<https://blog.devgenius.io/create-a-percentage-calculator-with-react-and-javascript-a1be96451ca4?source=collection_archive---------4----------------------->

![](img/0b006eda2b23dbdc5205e425e96b13ed.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

React 是一个易于使用的 JavaScript 框架，让我们可以创建前端应用程序。

在本文中，我们将看看如何用 React 和 JavaScript 创建一个百分比计算器。

# 创建项目

我们可以用 Create React App 创建 React 项目。

要安装它，我们运行:

```
npx create-react-app percentage-calculator
```

和 NPM 一起创建我们的 React 项目。

# 创建百分比计算器

为了创建百分比计算器，我们编写:

```
import React, { useState } from "react";export default function App() {
  const [pointsGiven, setPointsGiven] = useState(0);
  const [pointsPossible, setPointsPossible] = useState(0);
  const [percentage, setPercentage] = useState(0); const calculate = (e) => {
    e.preventDefault();
    const formValid = +pointsGiven >= 0 && +pointsPossible > 0;
    if (!formValid) {
      return;
    }
    setPercentage((+pointsGiven / +pointsPossible) * 100);
  }; return (
    <div className="App">
      <form onSubmit={calculate}>
        <div>
          <label>points given</label>
          <input
            value={pointsGiven}
            onChange={(e) => setPointsGiven(e.target.value)}
          />
        </div> <div>
          <label>points possible</label>
          <input
            value={pointsPossible}
            onChange={(e) => setPointsPossible(e.target.value)}
          />
        </div>
        <button type="submit">calculate</button>
      </form>
      <div>percentage:{percentage}</div>
    </div>
  );
}
```

我们有`pointsGiven`、`pointsPossible`和`percentage`状态，它们都是数字。

接下来，我们定义`calculate`函数来根据`pointsGiven`和`pointsPossible`计算`percentage`。

在其中，我们调用`e.preventDefault()`进行客户端表单提交。

接下来，我们检查`pointsGiven`和`pointsPossible`是否为 0 或更大。

如果是，那么我们用表达式调用`setPercentage`来计算`percentage`。

接下来，我们添加带有`onSubmit`属性的表单来添加提交处理程序。

其中，我们有带`value`和`onChange`属性的输入，分别用于获取和设置状态。

`e.target.value`具有输入值，因此我们可以使用它来设置状态。

当我们点击类型为`submit`的按钮时，提交处理程序运行。

在表格下方，我们显示了`percentage`的结果。

# 结论

我们可以用 React 和 JavaScript 轻松创建一个百分比计算器。