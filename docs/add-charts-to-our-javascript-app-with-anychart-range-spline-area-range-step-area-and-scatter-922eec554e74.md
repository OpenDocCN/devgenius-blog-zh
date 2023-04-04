# 使用 Anychart 将图表添加到我们的 JavaScript 应用程序中——范围样条区域、范围步长区域和散点图

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-range-spline-area-range-step-area-and-scatter-922eec554e74?source=collection_archive---------4----------------------->

![](img/2ed06d9db11aca15ceb4e02afc144c50.png)

米切尔·奥尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 范围样条面积图

我们可以很容易地用 Anychart 添加一个范围样条面积图。

例如，我们可以编写以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

然后我们可以添加以下 JavaScript 代码:

```
const data = [
  ["January", 0.7, 6.1],
  ["February", 0.6, 6.3],
  ["March", 1.9, 8.5],
  ["April", 3.1, 10.8],
  ["May", 5.7, 14.4]
];const chart = anychart.area();
const series = chart.rangeSplineArea(data);
chart.container("container");
chart.draw();
```

我们添加脚本标签来添加 Anychart 基础包。

div 允许我们添加容器元素，在其中呈现图表。

然后，我们用具有 x 轴值、低 y 轴值和高 y 轴值的数组来定义`data`数组。

`anychart.area`让我们创建面积图。

然后用`chart.rangeSplineArea`方法为区间样条图设置数据。

`chart.container`设置要在其中呈现图表的容器元素的 ID。

`chart.draw`绘制海图。

# 距离步长面积图

我们可以用 Anychart 添加一个范围步长面积图。

例如，我们可以写:

```
const data = [{
    x: "1995",
    low: 0.10,
    high: 0.15
  },
  {
    x: "1996",
    low: 0.10,
    high: 0.15
  },
  {
    x: "1997",
    low: 0.12,
    high: 0.17
  },
  {
    x: "1998",
    low: 0.13,
    high: 0.20
  },
  {
    x: "1999",
    low: 0.15,
    high: 0.20
  },
];const chart = anychart.area();
const series = chart.rangeStepArea(data);
series.stepDirection("forward");
chart.container("container");
chart.draw();
```

我们用属性`x`创建了`data`数组来添加 x 轴值。

`low`具有低 y 轴值。

并且`high`具有高 y 轴值。

`anychart.area`创建面积图。

`change.rangeStepArea`以`data`为图表数据创建范围步长面积图。

`series.stepDirection`设置步进方向。

其余的代码与前面的例子相同。

# 散布气泡图

我们可以用`chart.bubble`方法创建一个散点图。

例如，我们可以写:

```
const data = [{
    x: 1.3,
    value: 39,
    size: 7
  },
  {
    x: 2.5,
    value: 42,
    size: 24
  },
  {
    x: 3.2,
    value: 54,
    size: 39
  },
  {
    x: 3.9,
    value: 38,
    size: 18
  },
  {
    x: 5.1,
    value: 58,
    size: 2
  }
];const chart = anychart.scatter();
const series = chart.bubble(data);
chart.container("container");
chart.draw();
```

我们有一个包含具有`x`、`value`和`size`属性的对象的`data`数组。

`x`有 x 轴数值。

`value` jas 的 y 轴值。

`size`有气泡的半径。

`anychart.scatter`创建散点图。

让我们添加气泡。

代码的其余部分与前面的示例相同。

# 结论

我们可以用 Anychart 创建范围样条面积图、范围步长面积图和散点图。