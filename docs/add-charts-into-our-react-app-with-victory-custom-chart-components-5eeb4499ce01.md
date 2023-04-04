# 使用 Victory 将图表添加到我们的 React 应用程序中—自定义图表组件

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-custom-chart-components-5eeb4499ce01?source=collection_archive---------10----------------------->

![](img/16578c00e67695cdefef348e6859548a.png)

Solen Feyissa 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# 多边形图表

我们可以用`Polygon`组件添加多边形图表。

例如，我们可以写:

```
import React from "react";
import { VictoryChart, VictoryScatter } from "victory";const data = [
  { x: 2, y: 1 },
  { x: 3, y: 5 },
  { x: 6, y: 3 }
];const Polygon = (props) => {
  const getPoints = (data, scale) => {
    return data.reduce(
      (pointStr, { x, y }) => `${pointStr} ${scale.x(x)},${scale.y(y)}`,
      ""
    );
  }; const { data, style, scale } = props;
  const points = getPoints(data, scale);
  return <polygon points={points} style={style} />;
};export default function App() {
  return (
    <VictoryChart height={400} width={400} domain={[-10, 10]}>
      <Polygon data={data} style={{ fill: "tomato", opacity: 0.5 }} />
      <VictoryScatter data={data} />
    </VictoryChart>
  );
}
```

我们在`Polygon`组件中创建了`getPoint`函数来创建多边形路径的点串。

然后我们将它传递给`polygon`元素来创建点。

在`App`中，我们通过`Polygon`组件和`data`道具从给定的角点创建多边形。

我们用`VictoryScatter`组件将相同的点渲染成点。

# 使用 Victory 组件创建定制组件

我们可以使用 Victory 组件来创建定制组件。

例如，我们可以写:

```
import React from "react";
import {
  VictoryAxis,
  VictoryChart,
  VictoryGroup,
  VictoryLine,
  VictoryPie,
  VictoryScatter
} from "victory";const data = [
  { x: 2, y: 1 },
  { x: 3, y: 5 },
  { x: 6, y: 3 }
];const CustomPie = (props) => {
  const { datum, x, y } = props;
  const pieWidth = 120; return (
    <g transform={`translate(${x - pieWidth / 2}, ${y - pieWidth / 2})`}>
      <VictoryPie
        standalone={false}
        height={pieWidth}
        width={pieWidth}
        data={datum.pie}
        style={{ labels: { fontSize: 0 } }}
        colorScale={["#f77", "#55e", "#8af"]}
      />
    </g>
  );
};export default function App() {
  return (
    <VictoryChart domain={{ y: [0, 100] }}>
      <VictoryAxis />
      <VictoryGroup data={data}>
        <VictoryLine />
        <VictoryScatter dataComponent={<CustomPie />} />
      </VictoryGroup>
    </VictoryChart>
  );
}
```

我们创建了呈现小饼图的`CustomPie`组件，并用`VictoryPie`组件将饼图呈现为点。

在`App`中，我们通过将`VictoryGroup`设置为`data`道具来渲染`data`。

`VictoryScatter`让我们渲染这些点。

我们将`CustomPie`设置为`dataComponent`道具，将馅饼渲染为点。

我们可以将任何 SVG 组件作为点组件传入`dataComponent`属性中。

例如，我们可以使用样式化组件为点创建 SVG 圆:

```
import React from "react";
import styled from "styled-components";
import { VictoryChart, VictoryLine, VictoryScatter } from "victory";const data = [
  { x: 2, y: 1 },
  { x: 3, y: 5 },
  { x: 6, y: 3 }
];const StyledPoint = styled.circle`
  fill: ${(props) => props.color};
`;const colors = ["#A8E6CE", "#DCEDC2", "#FFD3B5", "#FFAAA6", "#FF8C94"];const ScatterPoint = ({ x, y, datum, min, max }) => {
  const i = React.useMemo(() => {
    return Math.floor(((datum.y - min) / (max - min)) * (colors.length - 1));
  }, [datum, min, max]);return <StyledPoint color={colors[i]} cx={x} cy={y} r={6} />;
};export default function App() {
  const temperatures = data.map(({ y }) => y);
  const min = Math.min(...temperatures);
  const max = Math.max(...temperatures); return (
    <VictoryChart>
      <VictoryLine data={data} />
      <VictoryScatter
        data={data}
        dataComponent={<ScatterPoint min={min} max={max} />}
      />
    </VictoryChart>
  );
}
```

`StyledPoint`组件具有点。

并且`ScatterPoint`用该点的填充颜色渲染`StyledPoint`。

然后我们通过`ScatterPoint`作为`dataComponent`的值来渲染这些点。

# 结论

我们可以使用 React Victory 渲染多边形图表并为图表创建自定义组件。