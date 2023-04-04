# 使用 Victory 将图表添加到我们的 React 应用程序中—条形图颜色和标签

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-bar-colors-and-labels-6190c78c0a70?source=collection_archive---------9----------------------->

![](img/52e6498b6d302e76c52a1712d127300a.png)

图片由 [Xan Griffin](https://unsplash.com/@xangriffin?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# 更改条形图颜色

我们可以用`VictoryStack`组件的`colorScale`属性改变条形的颜色。

例如，我们可以写:

```
import React from "react";
import {
  VictoryBar,
  VictoryChart,
  VictoryAxis,
  VictoryStack,
  VictoryTheme
} from "victory";const data2018 = [
  { quarter: 1, earnings: 13000 },
  { quarter: 2, earnings: 16500 },
  { quarter: 3, earnings: 14250 },
  { quarter: 4, earnings: 19000 }
];const data2019 = [
  { quarter: 1, earnings: 15000 },
  { quarter: 2, earnings: 12500 },
  { quarter: 3, earnings: 19500 },
  { quarter: 4, earnings: 13000 }
];const data2020 = [
  { quarter: 1, earnings: 11500 },
  { quarter: 2, earnings: 13250 },
  { quarter: 3, earnings: 20000 },
  { quarter: 4, earnings: 15500 }
];const data2021 = [
  { quarter: 1, earnings: 18000 },
  { quarter: 2, earnings: 13250 },
  { quarter: 3, earnings: 15000 },
  { quarter: 4, earnings: 12000 }
];export default function App() {
  return (
    <VictoryChart domainPadding={20}>
      <VictoryAxis
        tickValues={[1, 2, 3, 4]}
        tickFormat={["Quarter 1", "Quarter 2", "Quarter 3", "Quarter 4"]}
      />
      <VictoryAxis dependentAxis tickFormat={(x) => `$${x / 1000}k`} />
      <VictoryStack colorScale="warm">
        <VictoryBar data={data2018} x="quarter" y="earnings" />
        <VictoryBar data={data2019} x="quarter" y="earnings" />
        <VictoryBar data={data2020} x="quarter" y="earnings" />
        <VictoryBar data={data2021} x="quarter" y="earnings" />
      </VictoryStack>
    </VictoryChart>
  );
}
```

# 添加条形图标签

例如，我们可以写:

```
import React from "react";
import { Bar, VictoryBar, VictoryChart, VictoryTooltip } from "victory";const data = [
  { x: 1, y: 13000 },
  { x: 2, y: 16500 },
  { x: 3, y: 14250 },
  { x: 4, y: 19000 }
];export default function App() {
  return (
    <VictoryChart domainPadding={{ x: 40, y: 40 }}>
      <VictoryBar
        style={{ data: { fill: "#c43a31" } }}
        data={data}
        labels={({ datum }) => `y: ${datum.y}`}
        labelComponent={<VictoryTooltip />}
        dataComponent={
          <Bar tabIndex={0} ariaLabel={({ datum }) => `x: ${datum.x}`} />
        }
      />
    </VictoryChart>
  );
}
```

我们通过设置`labels`属性从`datum`属性获取数据来呈现条形图。

我们通过设置`arialLabel`道具对 x 轴做同样的事情。

我们还设置了`labelComponent`道具，当鼠标悬停在工具栏上时，它会为工具栏添加一个工具提示，以显示其值。

# 标签的背景

我们可以通过使用`VictoryLabel`组件来添加样式栏标签。

例如，我们可以写:

```
import React from "react";
import { Bar, VictoryBar, VictoryChart, VictoryLabel } from "victory";const data = [
  { x: 1, y: 13000, label: "first label" },
  { x: 2, y: 16500, label: "second label" },
  { x: 3, y: 14250, label: "3rd label" },
  { x: 4, y: 19000, label: "4th label" }
];export default function App() {
  return (
    <VictoryChart domainPadding={{ x: 40, y: 40 }}>
      <VictoryBar
        style={{ data: { fill: "#c43a31" } }}
        data={data}
        labels={({ datum }) => `y: ${datum.y}`}
        labelComponent={
          <VictoryLabel
            dy={-20}
            backgroundStyle={{ fill: "tomato", opacity: 0.6 }}
            backgroundPadding={{ bottom: 5, top: 5, left: 10, right: 10 }}
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

添加`VictoryLabel`组件，用我们想要的样式设置`backgroundStyle`和`backgroundPadding`道具。

`label`属性将自动显示为标签文本。

# 结论

我们可以在我们的 React 应用程序 with Victory 中自定义带有标签和颜色的条形图。