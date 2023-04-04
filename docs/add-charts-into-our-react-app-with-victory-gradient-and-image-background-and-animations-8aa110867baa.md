# 使用 Victory 将图表添加到我们的 React 应用程序中，包括渐变、图像背景和动画

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-gradient-and-image-background-and-animations-8aa110867baa?source=collection_archive---------4----------------------->

![](img/0d2c40c822335f3752646a33f4a4223d.png)

由 [Katrina Berban](https://unsplash.com/@kattrinnaaaaa?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# 极坐标图的渐变背景

我们可以用`radialGradient`组件为极坐标图添加渐变/

例如，我们可以写:

```
import React from "react";
import { VictoryChart, VictoryPolarAxis, VictoryScatter } from "victory";export default function App() {
  return (
    <div>
      <svg>
        <defs>
          <radialGradient id="radial_gradient">
            <stop offset="10%" stopColor="red" />
            <stop offset="95%" stopColor="gold" />
          </radialGradient>
        </defs>
      </svg>
      ​
      <VictoryChart
        polar
        style={{
          background: { fill: "url(#radial_gradient)" }
        }}
      >
        <VictoryScatter />
        <VictoryPolarAxis
          style={{
            tickLabels: { angle: 0 }
          }}
          tickValues={[0, 90, 180, 270]}
        />
      </VictoryChart>
    </div>
  );
}
```

我们添加了`stop`组件和`offset`道具来设置渐变中的颜色数量。

`VictoryPolarAxis`组件让我们为图表添加轴。

`VictoryChart`组件有`polar`道具添加极坐标图/

# 带有图像的图表

我们可以通过设置`VictoryChart`的`backgroundComponent`道具来添加带有背景图片的图表。

例如，我们可以写:

```
import React from "react";
import { VictoryAxis, VictoryChart, VictoryLine } from "victory";const CustomBackground = (props) => {
  return <image href={"https://picsum.photos/id/906/525/300.jpg"} {...props} />;
};const Matterhorn = (props) => {
  return (
    <VictoryChart
      domain={{ y: [2000, 5000] }}
      style={{ background: { opacity: 0.8 } }}
      backgroundComponent={<CustomBackground />}
    >
      <VictoryLine
        data={[
          { x: 0, y: 2500 },
          { x: 1.25, y: 2600 },
          { x: 1.8, y: 3000 },
          { x: 2.7, y: 3300 },
          { x: 3.1, y: 3800 },
          { x: 3.25, y: 4000 },
          { x: 3.5, y: 4000 },
          { x: 4, y: 4478, label: "4,478m" },
          { x: 4.5, y: 4300 },
          { x: 5.1, y: 4200 },
          { x: 6.3, y: 3500 },
          { x: 6.75, y: 3400 },
          { x: 7, y: 3300 },
          { x: 7.25, y: 3200 },
          { x: 9, y: 2900 },
          { x: 12, y: 2000 }
        ]}
        style={{
          data: { strokeWidth: 4 }
        }}
      />
      <VictoryAxis dependentAxis />
    </VictoryChart>
  );
};export default function App() {
  return <Matterhorn />;
}
```

我们添加了`CustomBackground`组件，并将其设置为`backgroundComponent`的值，以将图像显示为背景图像。

# 动画片

我们可以用`animate`道具给胜利图表添加动画。

例如，我们可以写:

```
import React, { useEffect, useState } from "react";
import { VictoryChart, VictoryScatter } from "victory";const random = (min, max) => Math.floor(min + Math.random() * max);const getScatterData = () => {
  const colors = [
    "violet",
    "cornflowerblue",
    "gold",
    "orange",
    "turquoise",
    "tomato",
    "greenyellow"
  ];
  const symbols = [
    "circle",
    "star",
    "square",
    "triangleUp",
    "triangleDown",
    "diamond",
    "plus"
  ];
  return Array(25)
    .fill()
    .map((index) => {
      const scaledIndex = Math.floor(index % 7);
      return {
        x: random(10, 50),
        y: random(2, 100),
        size: random(8) + 3,
        symbol: symbols[scaledIndex],
        fill: colors[random(0, 6)],
        opacity: 0.6
      };
    });
};export default function App() {
  const [data, setData] = useState([]);
  useEffect(() => {
    const timer = setInterval(() => {
      setData(getScatterData());
    }, 3000);
    return () => clearInterval(timer);
  }, []); return (
    <VictoryChart animate={{ duration: 2000, easing: "bounce" }}>
      <VictoryScatter
        data={data}
        style={{
          data: {
            fill: ({ datum }) => datum.fill,
            opacity: ({ datum }) => datum.opacity
          }
        }}
      />
    </VictoryChart>
  );
}
```

设置`animate`道具来设置动画的`duration`和`easing`。

每当我们更改`data`状态时，图表将使用新的`data` 值重新呈现。

`getScatterData`只返回散点图的数组值。

# 结论

我们可以在我们的 React with Victory 应用程序中为极坐标图和图表动画添加渐变。