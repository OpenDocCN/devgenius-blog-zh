# 使用 Visx 库将树视图添加到 React 应用程序中

> 原文：<https://blog.devgenius.io/add-tree-views-into-our-react-app-with-the-visx-library-b6c890a5d8ad?source=collection_archive---------1----------------------->

![](img/1d08a6f5e46e699308e68aeb347c4657.png)

照片由[吉利·斯图尔特](https://unsplash.com/@gillystewart?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Visx 是一个库，让我们可以轻松地将图形添加到 React 应用程序中。

在本文中，我们将了解如何使用它将树视图添加到 React 应用程序中。

# 安装所需的软件包

我们必须安装一些模块。

首先，我们运行:

```
npm i @visx/gradient @visx/group @visx/hierarchy @visx/responsive @visx/shape
```

安装软件包。

# 创建树视图

我们可以通过编写以下内容来创建具有不同链接类型、动画和布局的树视图:

```
import React, { useState } from "react";
import { Group } from "@visx/group";
import { hierarchy, Tree } from "@visx/hierarchy";
import { LinearGradient } from "@visx/gradient";
import { pointRadial } from "d3-shape";
import {
  LinkHorizontal,
  LinkVertical,
  LinkRadial,
  LinkHorizontalStep,
  LinkVerticalStep,
  LinkRadialStep,
  LinkHorizontalCurve,
  LinkVerticalCurve,
  LinkRadialCurve,
  LinkHorizontalLine,
  LinkVerticalLine,
  LinkRadialLine
} from "@visx/shape";const data = {
  name: "T",
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
            },
            {
              name: "D",
              children: [
                {
                  name: "D1"
                },
                {
                  name: "D2"
                },
                {
                  name: "D3"
                }
              ]
            }
          ]
        }
      ]
    },
    { name: "Z" },
    {
      name: "B",
      children: [{ name: "B1" }, { name: "B2" }]
    }
  ]
};function useForceUpdate() {
  const [, setValue] = useState(0);
  return () => setValue((value) => value + 1);
}const defaultMargin = { top: 30, left: 30, right: 30, bottom: 70 };function LinkControls({
  layout,
  orientation,
  linkType,
  stepPercent,
  setLayout,
  setOrientation,
  setLinkType,
  setStepPercent
}) {
  return (
    <div>
      <label>layout:</label>&nbsp;
      <select
        onClick={(e) => e.stopPropagation()}
        onChange={(e) => setLayout(e.target.value)}
        value={layout}
      >
        <option value="cartesian">cartesian</option>
        <option value="polar">polar</option>
      </select>
      &nbsp;&nbsp;
      <label>orientation:</label>&nbsp;
      <select
        onClick={(e) => e.stopPropagation()}
        onChange={(e) => setOrientation(e.target.value)}
        value={orientation}
        disabled={layout === "polar"}
      >
        <option value="vertical">vertical</option>
        <option value="horizontal">horizontal</option>
      </select>
      &nbsp;&nbsp;
      <label>link:</label>&nbsp;
      <select
        onClick={(e) => e.stopPropagation()}
        onChange={(e) => setLinkType(e.target.value)}
        value={linkType}
      >
        <option value="diagonal">diagonal</option>
        <option value="step">step</option>
        <option value="curve">curve</option>
        <option value="line">line</option>
      </select>
      {linkType === "step" && layout !== "polar" && (
        <>
          &nbsp;&nbsp;
          <label>step:</label>&nbsp;
          <input
            onClick={(e) => e.stopPropagation()}
            type="range"
            min={0}
            max={1}
            step={0.1}
            onChange={(e) => setStepPercent(Number(e.target.value))}
            value={stepPercent}
            disabled={linkType !== "step" || layout === "polar"}
          />
        </>
      )}
    </div>
  );
}function getLinkComponent({ layout, linkType, orientation }) {
  let LinkComponent; if (layout === "polar") {
    if (linkType === "step") {
      LinkComponent = LinkRadialStep;
    } else if (linkType === "curve") {
      LinkComponent = LinkRadialCurve;
    } else if (linkType === "line") {
      LinkComponent = LinkRadialLine;
    } else {
      LinkComponent = LinkRadial;
    }
  } else if (orientation === "vertical") {
    if (linkType === "step") {
      LinkComponent = LinkVerticalStep;
    } else if (linkType === "curve") {
      LinkComponent = LinkVerticalCurve;
    } else if (linkType === "line") {
      LinkComponent = LinkVerticalLine;
    } else {
      LinkComponent = LinkVertical;
    }
  } else if (linkType === "step") {
    LinkComponent = LinkHorizontalStep;
  } else if (linkType === "curve") {
    LinkComponent = LinkHorizontalCurve;
  } else if (linkType === "line") {
    LinkComponent = LinkHorizontalLine;
  } else {
    LinkComponent = LinkHorizontal;
  }
  return LinkComponent;
}function Example({
  width: totalWidth,
  height: totalHeight,
  margin = defaultMargin
}) {
  const [layout, setLayout] = useState("cartesian");
  const [orientation, setOrientation] = useState("horizontal");
  const [linkType, setLinkType] = useState("diagonal");
  const [stepPercent, setStepPercent] = useState(0.5);
  const forceUpdate = useForceUpdate(); const innerWidth = totalWidth - margin.left - margin.right;
  const innerHeight = totalHeight - margin.top - margin.bottom; let origin;
  let sizeWidth;
  let sizeHeight; if (layout === "polar") {
    origin = {
      x: innerWidth / 2,
      y: innerHeight / 2
    };
    sizeWidth = 2 * Math.PI;
    sizeHeight = Math.min(innerWidth, innerHeight) / 2;
  } else {
    origin = { x: 0, y: 0 };
    if (orientation === "vertical") {
      sizeWidth = innerWidth;
      sizeHeight = innerHeight;
    } else {
      sizeWidth = innerHeight;
      sizeHeight = innerWidth;
    }
  } const LinkComponent = getLinkComponent({ layout, linkType, orientation }); return totalWidth < 10 ? null : (
    <div>
      <LinkControls
        layout={layout}
        orientation={orientation}
        linkType={linkType}
        stepPercent={stepPercent}
        setLayout={setLayout}
        setOrientation={setOrientation}
        setLinkType={setLinkType}
        setStepPercent={setStepPercent}
      />
      <svg width={totalWidth} height={totalHeight}>
        <LinearGradient id="links-gradient" from="#fd9b93" to="#fe6e9e" />
        <rect width={totalWidth} height={totalHeight} rx={14} fill="#272b4d" />
        <Group top={margin.top} left={margin.left}>
          <Tree
            root={hierarchy(data, (d) => (d.isExpanded ? null : d.children))}
            size={[sizeWidth, sizeHeight]}
            separation={(a, b) => (a.parent === b.parent ? 1 : 0.5) / a.depth}
          >
            {(tree) => (
              <Group top={origin.y} left={origin.x}>
                {tree.links().map((link, i) => (
                  <LinkComponent
                    key={i}
                    data={link}
                    percent={stepPercent}
                    stroke="rgb(254,110,158,0.6)"
                    strokeWidth="1"
                    fill="none"
                  />
                ))}{tree.descendants().map((node, key) => {
                  const width = 40;
                  const height = 20;let top;
                  let left;
                  if (layout === "polar") {
                    const [radialX, radialY] = pointRadial(node.x, node.y);
                    top = radialY;
                    left = radialX;
                  } else if (orientation === "vertical") {
                    top = node.y;
                    left = node.x;
                  } else {
                    top = node.x;
                    left = node.y;
                  } return (
                    <Group top={top} left={left} key={key}>
                      {node.depth === 0 && (
                        <circle
                          r={12}
                          fill="url('#links-gradient')"
                          onClick={() => {
                            node.data.isExpanded = !node.data.isExpanded;
                            console.log(node);
                            forceUpdate();
                          }}
                        />
                      )}
                      {node.depth !== 0 && (
                        <rect
                          height={height}
                          width={width}
                          y={-height / 2}
                          x={-width / 2}
                          fill="#272b4d"
                          stroke={node.data.children ? "#03c0dc" : "#26deb0"}
                          strokeWidth={1}
                          strokeDasharray={node.data.children ? "0" : "2,2"}
                          strokeOpacity={node.data.children ? 1 : 0.6}
                          rx={node.data.children ? 0 : 10}
                          onClick={() => {
                            node.data.isExpanded = !node.data.isExpanded;
                            forceUpdate();
                          }}
                        />
                      )}
                      <text
                        dy=".33em"
                        fontSize={9}
                        fontFamily="Arial"
                        textAnchor="middle"
                        style={{ pointerEvents: "none" }}
                        fill={
                          node.depth === 0
                            ? "#71248e"
                            : node.children
                            ? "white"
                            : "#26deb0"
                        }
                      >
                        {node.data.name}
                      </text>
                    </Group>
                  );
                })}
              </Group>
            )}
          </Tree>
        </Group>
      </svg>
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

我们有了用来渲染树的`data`对象。

当我们改变布局、方向或链接类型时,`useForceUpdate`钩子用于返回强制树更新的函数。

然后我们有了带有下拉菜单的`LinkControls`组件，让我们设置布局、方向或链接类型。

接下来，我们用`getLinkComponent`函数返回给定`layoyt`、`linkType`或`orientation`的组件。

在`Example`组件中，我们得到了`LinkControls`组件中的`layout`、`orientation`和`linkType`集合。

`Group`组件包含树的所有部分。

然后我们添加带有`Tree`组件的树形视图。

它有一个带`tree`参数的渲染属性，用`tree.links()`方法获取父节点和子节点之间的链接，我们用`LinkComponent`函数组件渲染这些链接。

`tree.descendants()`返回后代。

然后我们在`tree.descendants().map()`回调中呈现节点。

我们用`if`语句设置节点的位置。

然后，我们根据节点的深度级别为节点渲染`circle` s 或`rect` s。

无论哪种情况，我们都在节点内部呈现`text`，它要么在`circle`内部，要么在`rect`内部。

# 结论

我们可以使用 Visx 库在 React 应用程序中渲染带有笛卡尔或极坐标的树视图。