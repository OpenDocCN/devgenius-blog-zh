# 使用 Victory 将图表添加到我们的 React 应用程序中—直方图和散点图

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-histograms-and-scatterplots-b4870aedf1d8?source=collection_archive---------7----------------------->

![](img/39b2c11bdd79b037bdc26d1a625c026b.png)

基思·卢克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# 柱状图

我们可以用 Victory 的`VictoryHistogram`组件在 React 应用程序中添加一个直方图。

例如，我们可以写:

```
import React from "react";
import { VictoryChart, VictoryHistogram } from "victory";const data = [
  { x: 0 },
  { x: 1 },
  { x: 1 },
  { x: 1 },
  { x: 1 },
  { x: 2 },
  { x: 2 },
  { x: 3 },
  { x: 4 },
  { x: 7 },
  { x: 7 },
  { x: 10 }
];export default function App() {
  return (
    <VictoryChart>
      <VictoryHistogram
        style={{ data: { fill: "#F1737F" } }}
        cornerRadius={3}
        data={data}
      />
    </VictoryChart>
  );
}
```

添加`VictoryHistogram`组件来添加直方图。

`cornerRadius`让我们设置拐角半径。

我们用`data.fill`属性设置条的填充颜色。

我们可以用`bins`道具换箱子:

```
import React from "react";
import { VictoryChart, VictoryHistogram } from "victory";const data = [
  { x: 0 },
  { x: 1 },
  { x: 1 },
  { x: 1 },
  { x: 1 },
  { x: 2 },
  { x: 2 },
  { x: 3 },
  { x: 4 },
  { x: 7 },
  { x: 7 },
  { x: 10 }
];export default function App() {
  return (
    <VictoryChart>
      <VictoryHistogram
        style={{ data: { fill: "#F1737F" } }}
        cornerRadius={3}
        bins={[0, 2, 8, 15]}
        data={data}
      />
    </VictoryChart>
  );
}
```

我们可以用`VictoryStack`组件堆叠直方图:

```
import React from "react";
import { VictoryChart, VictoryHistogram, VictoryStack } from "victory";const data1 = [
  { x: 0 },
  { x: 1 },
  { x: 1 },
  { x: 1 },
  { x: 1 },
  { x: 2 },
  { x: 2 },
  { x: 3 },
  { x: 4 },
  { x: 7 },
  { x: 7 },
  { x: 10 }
];const data2 = [
  { x: 0 },
  { x: 1 },
  { x: 1 },
  { x: 1 },
  { x: 2 },
  { x: 2 },
  { x: 3 },
  { x: 4 },
  { x: 5 },
  { x: 7 },
  { x: 8 }
];export default function App() {
  return (
    <VictoryChart>
      <VictoryStack colorScale="qualitative">
        <VictoryHistogram data={data1} />
        <VictoryHistogram cornerRadius={3} data={data2} />
      </VictoryStack>
    </VictoryChart>
  );
}
```

我们只是将`VictoryHistogram`放在`VictoryStack`组件中。

# 散点图

我们可以用`VictoryScatter`组件创建散点图。

例如，我们可以写:

```
import React from "react";
import { VictoryChart, VictoryScatter } from "victory";const data = [
  { x: 1, y: 2 },
  { x: 2, y: 3 },
  { x: 3, y: 5 },
  { x: 4, y: 4 },
  { x: 5, y: 7 }
];export default function App() {
  return (
    <VictoryChart
      style={{
        background: { fill: "lavender" }
      }}
    >
      <VictoryScatter data={data} />
    </VictoryChart>
  );
}
```

我们可以用`background.fill`属性设置散点图的背景。

我们添加带有`VictoryScatter`属性的散点图。

# 结论

我们可以用 Victory 在 React 应用程序中添加直方图和散点图。