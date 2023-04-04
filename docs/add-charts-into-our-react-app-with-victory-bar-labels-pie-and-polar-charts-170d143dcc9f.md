# 使用 Victory 将图表添加到我们的 React 应用程序中——条形图、饼图和极坐标图

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-bar-labels-pie-and-polar-charts-170d143dcc9f?source=collection_archive---------1----------------------->

![](img/c1cf5d512d39d3c90183bd2d588bd0ef.png)

照片由[谢里·西尔弗](https://unsplash.com/@sheri_silver?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# 多个条形标签

我们可以用一组文本添加多个条形标签。

例如，我们可以写:

```
import React from "react";
import { Bar, VictoryBar, VictoryChart, VictoryLabel } from "victory";const data = [
  { x: 1, y: 3, label: ["first", "label"] },
  { x: 2, y: 4, label: ["second", "label"] },
  { x: 3, y: 2, label: ["third", "final", "label"] }
];export default function App() {
  return (
    <VictoryChart domainPadding={{ x: 40, y: 40 }}>
      <VictoryBar
        style={{ data: { fill: "#c43a31" } }}
        data={data}
        labels={({ datum }) => `y: ${datum.y}`}
        labelComponent={
          <VictoryLabel
            backgroundStyle={[{ fill: "orange" }, { fill: "gold" }]}
            backgroundPadding={{ left: 5, right: 5 }}
          />
        }
        dataComponent={
          <Bar tabIndex={0} ariaLabel={({ datum }) => `x: ${datum.x}`} />
        }
      />
    </VictoryChart>
  );
}
```

我们在`data`数组中有`label`属性。

我们将一系列样式添加到`backgroundStyle`道具中。

# 带标签的极坐标图

我们可以用`VictoryBar`的`polar`道具添加一个带有标签的极坐标图。

例如，我们可以写:

```
import React from "react";
import { VictoryBar, VictoryLabel, VictoryTooltip } from "victory";const data = [
  { x: 1, y: 3, label: ["first", "label"] },
  { x: 2, y: 4, label: ["second", "label"] },
  { x: 3, y: 2, label: ["third", "final", "label"] }
];export default function App() {
  return (
    <VictoryBar
      polar
      data={data}
      style={{ data: { width: 40, fill: "tomato" } }}
      labelComponent={
        <VictoryTooltip
          active
          labelPlacement="perpendicular"
          pointerLength={30}
          pointerWidth={0}
          flyoutPadding={0}
          labelComponent={
            <VictoryLabel
              verticalAnchor="end"
              dy={8}
              backgroundStyle={{ fill: "white" }}
              backgroundPadding={8}
            />
          }
        />
      }
    />
  );
}
```

我们的`1abelComponent`道具有`VictoryTooltip`组件。

我们将`labelPlacement`设置为`perpendicular`来放置标签，使其与极线段齐平。

我们还设置了`pointerLength`、`pointerWidth`来设置标签的长度和宽度。

`VictoryLabel`有标签文本。

# 带标签的饼图

我们可以添加带有标签的饼图，标签带有`VictoryPie`和`VictoryTooltip`组件。

例如，我们可以写:

```
import React from "react";
import { VictoryPie, VictoryTooltip } from "victory";const data = [
  { x: 1, y: 3, placement: "vertical" },
  { x: 2, y: 4, placement: "parallel" },
  { x: 3, y: 2, placement: "perpendicular" }
];export default function App() {
  return (
    <VictoryPie
      colorScale="warm"
      radius={120}
      style={{ labels: { padding: 5, fontSize: 15 } }}
      data={data}
      labels={({ datum }) => `${datum.placement}\nlabel`}
      labelPlacement={({ datum }) => datum.placement}
      labelPosition="startAngle"
      labelComponent={<VictoryTooltip active />}
    />
  );
}
```

我们用`labelPlacement`道具设置标签位置。

它获取`placement`属性值并返回它。

我们用 th `labelComponent`渲染标签。

我们用`labelPosition`道具将标签放在与饼图段齐平的位置。

# 结论

我们可以在我们的 React 应用程序中添加多个条形标签和其他自定义标签选项。