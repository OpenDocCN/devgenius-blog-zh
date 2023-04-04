# 用 Visx 库创建 Voronoi 图

> 原文：<https://blog.devgenius.io/create-graphics-with-voronoi-diagram-with-the-visx-library-27fc1b4ddf67?source=collection_archive---------5----------------------->

![](img/7d8af8dd529e36e188100c9e9dca92f2.png)

约翰·西门子在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

Voronoi 图是由平面中的分区组成的图。

在本文中，我们将看看如何使用它来添加 Voronoi 图到我们的 React 应用程序中。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/clip-path @visx/event @visx/gradient @visx/group @visx/responsive @visx/voronoi 
```

安装软件包。

# 创建图表

我们可以通过添加模块提供的项目来创建图表。

我们使用来自`@visx/mock-data`模块的数据。

为了创建 Voronoi 图，我们写下:

```
import React, { useState, useMemo, useRef } from "react";
import { Group } from "@visx/group";
import { GradientOrangeRed, GradientPinkRed } from "@visx/gradient";
import { RectClipPath } from "@visx/clip-path";
import { voronoi, VoronoiPolygon } from "@visx/voronoi";
import { localPoint } from "@visx/event";const data = new Array(150).fill(null).map(() => ({
  x: Math.random(),
  y: Math.random(),
  id: Math.random().toString(36).slice(2)
}));const neighborRadius = 75;const defaultMargin = {
  top: 0,
  left: 0,
  right: 0,
  bottom: 76
};const Example = ({ width, height, margin = defaultMargin }) => {
  const innerWidth = width - margin.left - margin.right;
  const innerHeight = height - margin.top - margin.bottom;const voronoiLayout = useMemo(
    () =>
      voronoi({
        x: (d) => d.x * innerWidth,
        y: (d) => d.y * innerHeight,
        width: innerWidth,
        height: innerHeight
      })(data),
    [innerWidth, innerHeight]
  );const polygons = voronoiLayout.polygons();
  const svgRef = useRef(null);
  const [hoveredId, setHoveredId] = useState(null);
  const [neighborIds, setNeighborIds] = useState(new Set()); return width < 10 ? null : (
    <svg width={width} height={height} ref={svgRef}>
      <GradientOrangeRed id="voronoi_orange_red" />
      <GradientPinkRed id="voronoi_pink_red" />
      <RectClipPath
        id="voronoi_clip"
        width={innerWidth}
        height={innerHeight}
        rx={14}
      />
      <Group
        top={margin.top}
        left={margin.left}
        clipPath="url(#voronoi_clip)"
        onMouseMove={(event) => {
          if (!svgRef.current) return;const point = localPoint(svgRef.current, event);
          if (!point) return;const closest = voronoiLayout.find(point.x, point.y, neighborRadius);
          if (closest && closest.data.id !== hoveredId) {
            const neighbors = new Set();
            const cell = voronoiLayout.cells[closest.index];
            if (!cell) return;cell.halfedges.forEach((index) => {
              const edge = voronoiLayout.edges[index];
              const { left, right } = edge;
              if (left && left !== closest) neighbors.add(left.data.id);
              else if (right && right !== closest) neighbors.add(right.data.id);
            });setNeighborIds(neighbors);
            setHoveredId(closest.data.id);
          }
        }}
        onMouseLeave={() => {
          setHoveredId(null);
          setNeighborIds(new Set());
        }}
      >
        {polygons.map((polygon) => (
          <VoronoiPolygon
            key={`polygon-${polygon.data.id}`}
            polygon={polygon}
            fill={
              hoveredId &&
              (polygon.data.id === hoveredId ||
                neighborIds.has(polygon.data.id))
                ? "url(#voronoi_orange_red)"
                : "url(#voronoi_pink_red)"
            }
            stroke="#fff"
            strokeWidth={1}
            fillOpacity={
              hoveredId && neighborIds.has(polygon.data.id) ? 0.5 : 1
            }
          />
        ))}
        {data.map(({ x, y, id }) => (
          <circle
            key={`circle-${id}`}
            r={2}
            cx={x * innerWidth}
            cy={y * innerHeight}
            fill={id === hoveredId ? "fuchsia" : "#fff"}
            fillOpacity={0.8}
          />
        ))}
      </Group>
    </svg>
  );
};export default function App() {
  return (
    <div className="App">
      <Example width={500} height={300} />
    </div>
  );
}
```

我们用`data`数组为图表创建数据。

`x`和`y`属性用于分区的 x 和 y 坐标。

而`id`是每个分区的唯一 ID。

`neighborhoodRadius`具有当我们将鼠标悬停在一个分区上时高亮显示的分区的半径。

`defaultMargin`具有图表的默认边距。

`Example`组件有 Voronoi 图。

我们通过计算图表的`innerWidth`和`innerHeight`来计算图表的显示面积。

`voronoiLayout`变量根据图的维度计算分区布局。

`polygons`变量包含分区的多边形。

接下来，我们添加`GradientOrangeRed`来突出显示我们悬停的分区。

`GradientPinkRed`具有分区的正常颜色渐变。

在`Group`组件中，我们通过`polygons.map`调用创建分区多边形。

在`onMouseMove`处理程序中，我们检查是否有任何带有`closest`变量的相邻多边形，该变量由`voronoiLayout.find`调用生成。

我们用它来生成`cell`变量，我们用它来向`neighbord`对象添加多边形来突出显示相邻的分区。

它返回带有填充颜色的 ID 的`VoronoiPolygon`组件。

我们使用`x`和`y`坐标在每个多边形的中心显示一个点。

# 结论

我们可以使用 Visx 库在 React 应用程序中轻松创建 Voronoi 图。