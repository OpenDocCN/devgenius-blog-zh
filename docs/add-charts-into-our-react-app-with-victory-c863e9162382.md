# 将图表添加到我们的 React 应用程序中

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-c863e9162382?source=collection_archive---------8----------------------->

![](img/2a47456d3f150062a1ce51c8dfaf5368.png)

[雅弗桅杆](https://unsplash.com/@japhethmast?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# 装置

我们可以通过运行来安装胜利:

```
npm install victory
```

# 入门指南

我们可以用`VictoryBar`组件创建一个条形图。

为此，我们写道:

```
import React from "react";
import { VictoryBar, VictoryChart } from "victory";const data = [
  { quarter: 1, earnings: 13000 },
  { quarter: 2, earnings: 16500 },
  { quarter: 3, earnings: 14250 },
  { quarter: 4, earnings: 19000 }
];export default function App() {
  return (
    <VictoryChart domainPadding={20}>
      <VictoryBar data={data} x="quarter" y="earnings" />
    </VictoryChart>
  );
}
```

添加条形图。

我们有一个对象数组作为图表数据。

我们把它作为`data`属性的值传递。

然后我们分别用`x`和`y`属性为 x 和 y 轴值设置属性名。

我们设置`domainPadding`来移动条，这样它们就不会与轴重叠。

我们可以使用`VictoryAxis`组件定制轴:

```
import React from "react";
import { VictoryBar, VictoryChart, VictoryAxis } from "victory";const data = [
  { quarter: 1, earnings: 13000 },
  { quarter: 2, earnings: 16500 },
  { quarter: 3, earnings: 14250 },
  { quarter: 4, earnings: 19000 }
];export default function App() {
  return (
    <VictoryChart domainPadding={20}>
      <VictoryAxis
        tickValues={[1, 2, 3, 4]}
        tickFormat={["Quarter 1", "Quarter 2", "Quarter 3", "Quarter 4"]}
      />
      <VictoryAxis dependentAxis tickFormat={(x) => `$${x / 1000}k`} />
      <VictoryBar data={data} x="quarter" y="earnings" />
    </VictoryChart>
  );
}
```

`dependentAxis`道具让我们修改 y 轴。

`tickFormat`有一个函数返回我们想要在轴刻度上显示的内容。

我们还可以传入一组值来显示在轴上。

# 堆积条形图

我们可以用`VictoryStack`组件添加一个堆叠条形图。

为此，我们写道:

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
    <VictoryChart domainPadding={20} theme={VictoryTheme.material}>
      <VictoryAxis
        tickValues={[1, 2, 3, 4]}
        tickFormat={["Quarter 1", "Quarter 2", "Quarter 3", "Quarter 4"]}
      />
      <VictoryAxis dependentAxis tickFormat={(x) => `$${x / 1000}k`} />
      <VictoryStack>
        <VictoryBar data={data2018} x="quarter" y="earnings" />
        <VictoryBar data={data2019} x="quarter" y="earnings" />
        <VictoryBar data={data2020} x="quarter" y="earnings" />
        <VictoryBar data={data2021} x="quarter" y="earnings" />
      </VictoryStack>
    </VictoryChart>
  );
}
```

我们只是将`VictoryBar`嵌套在`VictoryStack`组件中。

# 结论

我们可以通过 Victory library 轻松地将条形图添加到 React 应用程序中。