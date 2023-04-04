# 使用 Visx 库将树形图添加到 React 应用程序中

> 原文：<https://blog.devgenius.io/add-treemaps-into-our-react-app-with-the-visx-library-7c0ba0094376?source=collection_archive---------4----------------------->

![](img/9dcd042bcfaca81b135abc6ebecb747c.png)

照片由 [Marita Kavelashvili](https://unsplash.com/@maritafox?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它将 treemaps 添加到 React 应用程序中。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/group @visx/hierarchy @visx/mock-data @visx/responsive @visx/scale
```

安装软件包。

# 添加树形图

我们可以通过编写以下代码将树形图添加到 React 应用程序中:

```
import React, { useState } from "react";
import { Group } from "@visx/group";
import {
  Treemap,
  hierarchy,
  stratify,
  treemapSquarify,
  treemapBinary,
  treemapDice,
  treemapResquarify,
  treemapSlice,
  treemapSliceDice
} from "@visx/hierarchy";
import shakespeare from "@visx/mock-data/lib/mocks/shakespeare";
import { scaleLinear } from "@visx/scale";const color1 = "#f3e9d2";
const color2 = "#4281a4";
const background = "#114b5f";const colorScale = scaleLinear({
  domain: [0, Math.max(...shakespeare.map((d) => d.size || 0))],
  range: [color2, color1]
});const data = stratify()
  .id((d) => d.id)
  .parentId((d) => d.parent)(shakespeare)
  .sum((d) => d.size || 0);const tileMethods = {
  treemapSquarify,
  treemapBinary,
  treemapDice,
  treemapResquarify,
  treemapSlice,
  treemapSliceDice
};const defaultMargin = { top: 10, left: 10, right: 10, bottom: 10 };function Example({ width, height, margin = defaultMargin }) {
  const [tileMethod, setTileMethod] = useState("treemapSquarify");
  const xMax = width - margin.left - margin.right;
  const yMax = height - margin.top - margin.bottom;
  const root = hierarchy(data).sort((a, b) => (b.value || 0) - (a.value || 0)); return width < 10 ? null : (
    <div>
      <label>tile method</label>{" "}
      <select
        onClick={(e) => e.stopPropagation()}
        onChange={(e) => setTileMethod(e.target.value)}
        value={tileMethod}
      >
        {Object.keys(tileMethods).map((tile) => (
          <option key={tile} value={tile}>
            {tile}
          </option>
        ))}
      </select>
      <div>
        <svg width={width} height={height}>
          <rect width={width} height={height} rx={14} fill={background} />
          <Treemap
            top={margin.top}
            root={root}
            size={[xMax, yMax]}
            tile={tileMethods[tileMethod]}
            round
          >
            {(treemap) => (
              <Group>
                {treemap
                  .descendants()
                  .reverse()
                  .map((node, i) => {
                    const nodeWidth = node.x1 - node.x0;
                    const nodeHeight = node.y1 - node.y0;
                    return (
                      <Group
                        key={`node-${i}`}
                        top={node.y0 + margin.top}
                        left={node.x0 + margin.left}
                      >
                        {node.depth === 1 && (
                          <rect
                            width={nodeWidth}
                            height={nodeHeight}
                            stroke={background}
                            strokeWidth={4}
                            fill="transparent"
                          />
                        )}
                        {node.depth > 2 && (
                          <rect
                            width={nodeWidth}
                            height={nodeHeight}
                            stroke={background}
                            fill={colorScale(node.value || 0)}
                          />
                        )}
                      </Group>
                    );
                  })}
              </Group>
            )}
          </Treemap>
        </svg>
      </div>
    </div>
  );
}export default function App() {
  return (
    <div className="App">
      <Example width={500} height={300} />
    </div>
  );
}
```

我们用`color1`和`color2`变量为树形图分区添加颜色。

`background`有背景色。

我们创建`colorScale`变量来为分区创建颜色变量。

然后，我们用`stratify`方法将数据转换成可以用于树形图的结构。

接下来，在`Example`组件中，我们添加一个下拉菜单来显示平铺方法选项。

这让我们可以为树形图设置平铺格式。

之后，我们添加`Treemap`组件来添加树形图。

在它的渲染属性中，我们调用`descendants`来获得后代。

然后我们调用`reverse`来反推数据。

`map`将它们映射到`rect` s，以便我们渲染分区。

我们根据深度级别对它们进行不同的渲染。

# 结论

我们可以通过 Visx 库轻松地将树形图添加到 React 应用程序中。