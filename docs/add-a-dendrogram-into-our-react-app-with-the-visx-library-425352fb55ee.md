# 使用 Visx 库将树状图添加到 React 应用程序中

> 原文：<https://blog.devgenius.io/add-a-dendrogram-into-our-react-app-with-the-visx-library-425352fb55ee?source=collection_archive---------3----------------------->

![](img/35d4951d21a2f8468ad4a899035a8634.png)

Todd Quackenbush 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加树状图。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/gradient @visx/group @visx/hierarchy @visx/responsive @visx/shape
```

安装软件包。

# 创建树状图

我们可以通过编写以下内容来创建树状图:

```
import React, { useMemo } from "react";
import { Group } from "@visx/group";
import { Cluster, hierarchy } from "@visx/hierarchy";
import { LinkVertical } from "@visx/shape";
import { LinearGradient } from "@visx/gradient";const citrus = "#ddf163";
const white = "#ffffff";
const green = "#79d259";
const aqua = "#37ac8c";
const merlinsbeard = "#f7f7f3";
const background = "#306c90";const clusterData = {
  name: "$",
  children: [
    {
      name: "A",
      children: [
        { name: "A1" },
        { name: "A2" },
        {
          name: "C",
          children: [
            {
              name: "C1"
            }
          ]
        }
      ]
    },
    {
      name: "B",
      children: [{ name: "B1" }, { name: "B2" }]
    },
    {
      name: "X",
      children: [
        {
          name: "Z"
        }
      ]
    }
  ]
};function Node({ node }) {
  const isRoot = node.depth === 0;
  const isParent = !!node.children; if (isRoot) return <RootNode node={node} />; return (
    <Group top={node.y} left={node.x}>
      {node.depth !== 0 && (
        <circle
          r={12}
          fill={background}
          stroke={isParent ? white : citrus}
          onClick={() => {
            alert(`clicked: ${JSON.stringify(node.data.name)}`);
          }}
        />
      )}
      <text
        dy=".33em"
        fontSize={9}
        fontFamily="Arial"
        textAnchor="middle"
        style={{ pointerEvents: "none" }}
        fill={isParent ? white : citrus}
      >
        {node.data.name}
      </text>
    </Group>
  );
}function RootNode({ node }) {
  const width = 40;
  const height = 20;
  const centerX = -width / 2;
  const centerY = -height / 2; return (
    <Group top={node.y} left={node.x}>
      <rect
        width={width}
        height={height}
        y={centerY}
        x={centerX}
        fill="url('#top')"
      />
      <text
        dy=".33em"
        fontSize={9}
        fontFamily="Arial"
        textAnchor="middle"
        style={{ pointerEvents: "none" }}
        fill={background}
      >
        {node.data.name}
      </text>
    </Group>
  );
}const defaultMargin = { top: 40, left: 0, right: 0, bottom: 40 };function Example({ width, height, margin = defaultMargin }) {
  const data = useMemo(() => hierarchy(clusterData), []);
  const xMax = width - margin.left - margin.right;
  const yMax = height - margin.top - margin.bottom; return width < 10 ? null : (
    <svg width={width} height={height}>
      <LinearGradient id="top" from={green} to={aqua} />
      <rect width={width} height={height} rx={14} fill={background} />
      <Cluster root={data} size={[xMax, yMax]}>
        {(cluster) => (
          <Group top={margin.top} left={margin.left}>
            {cluster.links().map((link, i) => (
              <LinkVertical
                key={`cluster-link-${i}`}
                data={link}
                stroke={merlinsbeard}
                strokeWidth="1"
                strokeOpacity={0.2}
                fill="none"
              />
            ))}
            {cluster.descendants().map((node, i) => (
              <Node key={`cluster-node-${i}`} node={node} />
            ))}
          </Group>
        )}
      </Cluster>
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

我们在代码的顶部为树状图创建变量。

`clusterData`变量包含了树状图的树数据。

接下来，我们创建`Node`组件来呈现树状图的节点。

它的`Group`组件里面有一些`text`。

根节点是用`RootBNode`组件创建的。

它与`Node`的不同之处在于它在文本周围多了一个矩形。

`Example`组件是整个树形图的集合。

我们用`Cluster`组件的渲染属性来渲染树节点。

该数据被设置为`root`道具的值。

在渲染道具的`Group`组件中，我们有`LinkVertical`组件来添加链接父节点和子节点的线条。

我们用`cluster.descendants`方法得到子节点。

我们将它们映射到。

# 结论

我们可以使用 Visx 库将树状图添加到 React 应用程序中。