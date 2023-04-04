# 使用 jsdiff 库将比较显示添加到 React 应用程序中

> 原文：<https://blog.devgenius.io/add-a-diff-display-into-a-react-app-with-the-jsdiff-library-5df6fe85f66d?source=collection_archive---------6----------------------->

![](img/0603c5e67098d6078a09be24d42c43bd.png)

照片由[埃琳娜·莫日维洛](https://unsplash.com/@miracleday?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

有时，我们可能想向用户展示两段文本之间的区别。

我们可以通过 jsdiff 库轻松做到这一点。

在本文中，我们将了解如何在 React 中使用 jsdiff 库。

# 装置

我们可以通过运行以下命令来安装该库:

```
npm install diff --save
```

# 添加差异显示

我们可以通过编写以下代码将差异显示添加到 React 应用程序中:

```
import React from "react";
const Diff = require("diff");const one = "beep boop";
const other = "beep boob blah";const diff = Diff.diffChars(one, other);
export default function App() {
  return (
    <div className="App">
      {diff.map((part) => {
        const color = part.added ? "green" : part.removed ? "red" : "grey";
        return <span style={{ color }}>{part.value}</span>;
      })}
    </div>
  );
}
```

`one`和`other`字符串是我们想要区分的。

我们用它们两个调用`Diff.diffChars`来创建`diff`数组。

然后我们调用`diff.map`将不同的部分映射到`span`元素。

当零件在第二串而不是第一串时,`part.added`是`true`。

`part.removed`是`true`当零件不在第二串中但在第一串中。

`part.value`具有部分的价值。

现在我们应该看到显示的比较。

显示屏将显示第二串，即`other`串，添加到第一串的零件为绿色，从第一串中移除的零件为红色。

我们可以用`diffJson`方法显示不同的 JSON 对象。

为了使用它，我们写:

```
import React from "react";
const Diff = require("diff");const one = { foo: "bar", bar: "baz" };
const other = { foo: "bar", bar: "bar" };const diff = Diff.diffJson(one, other);
export default function App() {
  return (
    <div className="App">
      {diff.map((part) => {
        const color = part.added ? "green" : part.removed ? "red" : "grey";
        return <span style={{ color }}>{part.value}</span>;
      })}
    </div>
  );
}
```

它将用我们设置的相同颜色显示添加、删除或保持不变的属性。

其他方法包括`diffTrimmedLines`方法来逐行比较文本，忽略前导和尾随空白。

`diffSentences`逐句比较两个文本块。

`diffCss`比较 2 个 CSS 标记。

`diffArrays`用`===`操作符比较 2 个数组的条目。

它们都返回与示例中属性相同的对象数组。

# 结论

我们可以将 jsdiff 库与 React 一起使用，轻松地向用户显示文本差异。