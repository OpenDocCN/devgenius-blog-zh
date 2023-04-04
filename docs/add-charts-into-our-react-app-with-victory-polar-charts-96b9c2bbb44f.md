# 将图表添加到我们的 Victory React 应用程序中——极坐标图

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-polar-charts-96b9c2bbb44f?source=collection_archive---------9----------------------->

![](img/b5dcd27d80582dca3c6b43fe7bdcffee.png)

照片由[顶石事件](https://unsplash.com/@capstoneeventgroup?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# 响应式胜利集装箱

我们可以设置`VictoryContainer`组件的`responsive`属性来呈现一个响应式图表:

```
import React from "react";
import { VictoryChart, VictoryContainer, VictoryLine } from "victory";export default function App() {
  return (
    <div>
      <VictoryChart
        height={200}
        width={300}
        containerComponent={<VictoryContainer responsive />}
      >
        <VictoryLine y={(data) => Math.sin(2 * Math.PI * data.x)} />
      </VictoryChart>
    </div>
  );
}
```

# 在自定义容器中呈现组件

我们可以在自定义容器中呈现组件。

例如，我们可以写:

```
import React from "react";
import { VictoryLabel, VictoryPie } from "victory";export default function App() {
  return (
    <svg viewBox="0 0 400 400">
      <VictoryPie
        standalone={false}
        width={400}
        height={400}
        data={[
          { x: "A", y: 10 },
          { x: "B", y: 20 },
          { x: "C", y: 80 }
        ]}
        innerRadius={70}
        labelRadius={100}
        style={{ labels: { fontSize: 20, fill: "white" } }}
      />
      <circle
        cx="200"
        cy="200"
        r="65"
        fill="none"
        stroke="black"
        strokeWidth={3}
      />
      <circle
        cx="200"
        cy="200"
        r="155"
        fill="none"
        stroke="black"
        strokeWidth={3}
      />
      <VictoryLabel
        textAnchor="middle"
        verticalAnchor="middle"
        x={200}
        y={200}
        style={{ fontSize: 30 }}
        text="Label"
      />
    </svg>
  );
}
```

在`svg`元素中呈现饼图。

这是因为图表组件被呈现为 SVG。

# 极坐标图

我们可以在 React 应用程序中添加极坐标图。

例如，我们可以写:

```
import React from "react";
import {
  VictoryAxis,
  VictoryBar,
  VictoryChart,
  VictoryPolarAxis,
  VictoryTheme
} from "victory";
const data = [
  { x: 1, y: 3 },
  { x: 2, y: 4 },
  { x: 3, y: 2 }
];export default function App() {
  return (
    <div>
      <VictoryChart polar domain={{ x: [0, 10] }} theme={VictoryTheme.material}>
        <VictoryPolarAxis tickCount={8} />
        <VictoryBar
          data={data}
          style={{ data: { fill: "#c43a31", stroke: "black", strokeWidth: 2 } }}
        />
      </VictoryChart>      ​
    </div>
  );
}
```

使用`VictoryChart`组件添加极坐标图。

`VictoryAxis`拥有图表的坐标轴。

`VictoryBar`在图表中呈现为段。

我们可以添加`VictoryPolarAxis`组件来将刻度线添加到极坐标图中:

```
import React from "react";
import {
  VictoryBar,
  VictoryChart,
  VictoryPolarAxis,
  VictoryTheme
} from "victory";
const data = [
  { x: 1, y: 3 },
  { x: 2, y: 4 },
  { x: 3, y: 2 }
];export default function App() {
  return (
    <div>
      <VictoryChart polar theme={VictoryTheme.material}>
        <VictoryPolarAxis
          dependentAxis
          style={{
            axis: { stroke: "none" },
            tickLabels: { fill: "none" },
            grid: { stroke: "grey", strokeDasharray: "4, 8" }
          }}
        />
        <VictoryPolarAxis tickValues={[0, 3, 6, 9]} />
        <VictoryBar
          style={{ data: { fill: "#c43a31", width: 50 } }}
          data={data}
        />
      </VictoryChart>
      ​
    </div>
  );
}
```

我们可以使用`VictoryBar`组件将多个分段添加到极坐标图中:

```
import React from "react";
import {
  VictoryBar,
  VictoryChart,
  VictoryPolarAxis,
  VictoryStack,
  VictoryTheme
} from "victory";
const data = [
  { x: 1, y: 3 },
  { x: 2, y: 4 },
  { x: 3, y: 2 }
];export default function App() {
  return (
    <div>
      <VictoryChart
        polar
        maxDomain={{ x: 360 }}
        height={250}
        width={250}
        padding={30}
      >
        <VictoryPolarAxis
          dependentAxis
          style={{
            axis: { stroke: "none" },
            tickLabels: { fill: "none" },
            grid: { stroke: "grey", strokeDasharray: "4, 8" }
          }}
        />
        <VictoryPolarAxis tickValues={[0, 45, 90, 135, 180, 225, 270, 315]} />
        <VictoryStack
          colorScale={["#ad1b11", "#c43a31", "#dc7a6b"]}
          style={{ data: { width: 50 } }}
        >
          <VictoryBar data={data} />
          <VictoryBar data={data} />
          <VictoryBar data={data} />
        </VictoryStack>
      </VictoryChart>
      ​
    </div>
  );
}
```

# 结论

我们可以用 React Victory 添加各种极坐标图。