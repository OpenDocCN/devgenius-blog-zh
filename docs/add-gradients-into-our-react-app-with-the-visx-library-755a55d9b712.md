# 使用 Visx 库将渐变添加到 React 应用程序中

> 原文：<https://blog.devgenius.io/add-gradients-into-our-react-app-with-the-visx-library-755a55d9b712?source=collection_archive---------5----------------------->

![](img/b59f26323313943e6055d0879132b64a.png)

由[塞萨尔·库托](https://unsplash.com/@xcrap?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将看看如何使用它来添加渐变到我们的 React 应用程序。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/gradient @visx/responsive @visx/shape
```

安装软件包。

# 创建渐变

我们可以通过书写来创建渐变:

```
import React from "react";
import { Bar } from "@visx/shape";
import {
  GradientDarkgreenGreen,
  GradientLightgreenGreen,
  GradientOrangeRed,
  GradientPinkBlue,
  GradientPinkRed,
  GradientPurpleOrange,
  GradientPurpleRed,
  GradientTealBlue,
  RadialGradient,
  LinearGradient
} from "@visx/gradient";const defaultMargin = {
  top: 0,
  left: 0,
  right: 0,
  bottom: 0
};const Gradients = [
  GradientPinkRed,
  ({ id }) => <RadialGradient id={id} from="#55bdd5" to="#4f3681" r="80%" />,
  GradientOrangeRed,
  GradientPinkBlue,
  ({ id }) => (
    <LinearGradient id={id} from="#351CAB" to="#621A61" rotate="-45" />
  ),
  GradientLightgreenGreen,
  GradientPurpleOrange,
  GradientTealBlue,
  GradientPurpleRed,
  GradientDarkgreenGreen
];function Example({ width, height, margin = defaultMargin }) {
  const numColumns = width > 600 ? 5 : 2;
  const numRows = Gradients.length / numColumns;
  const columnWidth = Math.max(width / numColumns, 0);
  const rowHeight = Math.max((height - margin.bottom) / numRows, 0); return (
    <svg width={width} height={height}>
      {Gradients.map((Gradient, index) => {
        const columnIndex = index % numColumns;
        const rowIndex = Math.floor(index / numColumns);
        const id = `visx-gradient-demo-${index}-${rowIndex}${columnIndex}`; return (
          <React.Fragment key={id}>
            <Gradient id={id} />
            <Bar
              fill={`url(#${id})`}
              x={columnIndex * columnWidth}
              y={rowIndex * rowHeight}
              width={columnWidth}
              height={rowHeight}
              stroke="#ffffff"
              strokeWidth={8}
              rx={14}
            />
          </React.Fragment>
        );
      })}
    </svg>
  );
}export default function App() {
  return (
    <div className="App">
      <Example width={500} height={300} />
    </div>
  );
}
```

我们从`@visx/gradient`模块导入渐变组件。

然后我们把它们放在`Gradients`数组中。

我们可以直接使用进口元件。

或者我们可以创建一个返回`RadiantGradient`的函数来创建辐射渐变效果。

我们也可以使用`LinearGradient`组件来创建一个线性渐变。

在`Example`组件中，我们调用`Gradients.map`来呈现`map`回调中的组件。

通过添加`Bar`组件，我们用一个白条来分隔渐变。

# 结论

我们可以使用 Visx 库将渐变添加到 React 应用程序中。