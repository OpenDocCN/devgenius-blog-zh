# 使用 Victory 将图表添加到我们的 React 应用程序中——过渡、笔刷和缩放

> 原文：<https://blog.devgenius.io/add-charts-into-our-react-app-with-victory-transition-brush-and-zoom-4eeaeed1431c?source=collection_archive---------8----------------------->

![](img/28adcd09b64a39c2092c1884e615fd57.png)

罗斯·斯奈登在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

胜利让我们能够将图表和数据可视化添加到 React 应用程序中。

在本文中，我们将了解如何使用 Victory 将图表添加到 React 应用程序中。

# 过渡

我们可以用`VictoryBar`组件设置动画的过渡效果。

例如，我们可以写:

```
import React, { useEffect, useState } from "react";
import { VictoryBar, VictoryChart } from "victory";const random = (min, max) => Math.floor(min + Math.random() * max);const getData = () => {
  const bars = random(6, 10);
  return Array(bars)
    .fill()
    .map((_, index) => {
      return {
        x: index + 1,
        y: random(2, 100)
      };
    });
};export default function App() {
  const [data, setData] = useState([]);
  useEffect(() => {
    const timer = setInterval(() => {
      setData(getData());
    }, 3000);
    return () => clearInterval(timer);
  }, []); return (
    <VictoryChart domainPadding={{ x: 20 }} animate={{ duration: 500 }}>
      <VictoryBar
        data={data}
        style={{
          data: { fill: "tomato", width: 12 }
        }}
        animate={{
          onExit: {
            duration: 500,
            before: () => ({
              _y: 0,
              fill: "orange",
              label: "BYE"
            })
          }
        }}
      />
    </VictoryChart>
  );
}
```

我们为图表生成随机数据，并每 3 秒钟用新数据更新一次。

我们在`VictoryBar`组件上设置了`animate`道具，为条形添加过渡效果。

`onExit.duration`已经离开动画的持续时间。

我们用`fill`属性改变条形的颜色。

当条形离开屏幕时，显示`label`。

# 笔刷和缩放

我们可以用`VictoryChart`的`containerComponent`道具添加画笔和放大图表。

例如，我们可以写:

```
import React from "react";
import { VictoryChart, VictoryScatter, VictoryZoomContainer } from "victory";const random = (min, max) => Math.floor(min + Math.random() * max);const getData = () => {
  return Array(50)
    .fill()
    .map((index) => {
      return {
        x: random(1, 50),
        y: random(10, 90),
        size: random(8) + 3
      };
    });
};export default function App() {
  return (
    <VictoryChart
      domain={{ y: [0, 100] }}
      containerComponent={
        <VictoryZoomContainer zoomDomain={{ x: [5, 35], y: [0, 100] }} />
      }
    >
      <VictoryScatter
        data={getData()}
        style={{
          data: {
            opacity: ({ datum }) => (datum.y % 5 === 0 ? 1 : 0.7),
            fill: ({ datum }) => (datum.y % 5 === 0 ? "tomato" : "black")
          }
        }}
      />
    </VictoryChart>
  );
}
```

我们将`containerComponent`设置为`VictoryZoomContainer`组件。

`zoomDomain`道具允许我们使用`x`和`y`属性设置 x 轴和 y 轴的缩放级别范围。

我们还可以为画笔创建一个单独的图表:

```
import React, { useState } from "react";
import {
  VictoryAxis,
  VictoryBrushContainer,
  VictoryChart,
  VictoryLine,
  VictoryZoomContainer
} from "victory";export default function App() {
  const [selectedDomain, setSelectedDomain] = useState();
  const [zoomDomain, setZoomDomain] = useState();const handleZoom = (domain) => {
    setSelectedDomain(domain);
  };const handleBrush = (domain) => {
    setZoomDomain(domain);
  };return (
    <>
      <VictoryChart
        width={550}
        height={300}
        scale={{ x: "time" }}
        containerComponent={
          <VictoryZoomContainer
            responsive={false}
            zoomDimension="x"
            zoomDomain={zoomDomain}
            onZoomDomainChange={handleZoom}
          />
        }
      >
        <VictoryLine
          style={{
            data: { stroke: "tomato" }
          }}
          data={[
            { x: new Date(1982, 1, 1), y: 125 },
            { x: new Date(1987, 1, 1), y: 257 },
            { x: new Date(1993, 1, 1), y: 345 },
            { x: new Date(1997, 1, 1), y: 515 },
            { x: new Date(2001, 1, 1), y: 132 },
            { x: new Date(2005, 1, 1), y: 305 },
            { x: new Date(2011, 1, 1), y: 270 },
            { x: new Date(2015, 1, 1), y: 470 }
          ]}
        />
      </VictoryChart><VictoryChart
        width={550}
        height={90}
        scale={{ x: "time" }}
        padding={{ top: 0, left: 50, right: 50, bottom: 30 }}
        containerComponent={
          <VictoryBrushContainer
            responsive={false}
            brushDimension="x"
            brushDomain={selectedDomain}
            onBrushDomainChange={handleBrush}
          />
        }
      >
        <VictoryAxis
          tickValues={[
            new Date(1985, 1, 1),
            new Date(1990, 1, 1),
            new Date(1995, 1, 1),
            new Date(2000, 1, 1),
            new Date(2005, 1, 1),
            new Date(2010, 1, 1),
            new Date(2015, 1, 1)
          ]}
          tickFormat={(x) => new Date(x).getFullYear()}
        />
        <VictoryLine
          style={{
            data: { stroke: "tomato" }
          }}
          data={[
            { x: new Date(1982, 1, 1), y: 125 },
            { x: new Date(1987, 1, 1), y: 257 },
            { x: new Date(1993, 1, 1), y: 345 },
            { x: new Date(1997, 1, 1), y: 515 },
            { x: new Date(2001, 1, 1), y: 132 },
            { x: new Date(2005, 1, 1), y: 305 },
            { x: new Date(2011, 1, 1), y: 270 },
            { x: new Date(2015, 1, 1), y: 470 }
          ]}
        />
      </VictoryChart>
    </>
  );
}
```

我们通过拖动底部图形来设置`zoomDomain`和`selectedDomain`来实现这一点。

在底部`VictoryChart`中，我们将`containerComponent`设置为`VictoryBrushContainer`。

`brushDomain`设置为`selectedDomain`，

并且将`onBrushDomainChange`设置到`handleBrush`功能来改变`selectedDomain`。

该位置将反映在顶部图表中。

我们有类似的代码来设置顶部图表中的`zoomDomain`。

缩放反映在底部的图表中。

# 结论

我们可以使用 Victory 在 React 应用程序中添加过渡、画笔和放大图表。