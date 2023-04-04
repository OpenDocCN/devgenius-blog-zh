# 使用 Anychart 向我们的 JavaScript 应用程序添加图表—气泡图、项目符号图和柱形图

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-bubble-bullet-and-column-charts-aa2d6751f7ab?source=collection_archive---------8----------------------->

![](img/01c6e78cf91c0646c7d32227261c9141.png)

由[paweczerwi ski](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 气泡图

我们可以使用 Anychart 轻松创建气泡图。

例如，我们可以编写以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script>
<div id="container" style="width: 500px; height: 400px;"></div>
```

然后我们可以编写下面的 JavaScript 代码:

```
const chart = anychart.cartesian();
const data = [
  ["2000", 1100, 1],
  ["2001", 880, 2],
  ["2002", 1100, 5],
  ["2003", 1500, 3],
  ["2004", 921, 3],
];chart.bubble(data);
chart.title("Bubble Chart");
chart.xAxis().title("Years");
chart.yAxis().title("Sales");
chart.container("container");
chart.draw();
```

脚本标签用于添加 Anychart 库。

div 是呈现图表的容器。

让我们创建一个图表。

`data` 在`data` 数组中。

每个条目的第一个值是 x 轴值。

第二个值是 y 轴值。

第三个是气泡的半径。

`chart.bubble`获取我们正在渲染的数据。

`chart.title`设置标题。

`chart.xAxis().title`设置 x 轴标签。

`chart.yAxis().title`设置 y 轴标签。

`chart.container`设置要在其中呈现图表的容器元素的 ID。

`chart.draw`绘制图表。

# 项目符号图

为了创建项目符号图，我们编写以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script>
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-bullet.min.js"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

我们需要添加一个库，通过第二个脚本标签创建项目符号图。

然后，我们可以通过编写以下内容来创建图表:

```
const chart = anychart.bullet([{
  value: 630
}]);
chart.range().from(0).to(750);
chart.layout('vertical');
chart.container("container");
chart.draw();
```

我们调用`anychart.bullet`来创建图表。

`chart.range`有图表的取值范围。

`chart.layout`具有酒吧的方位。

`chart.container`设置要在其中呈现图表的容器元素的 ID。

`chart.draw`绘制图表。

# 柱形图

我们可以通过编写以下内容来创建柱形图:

```
const data = [
  ["apple", 100],
  ["orange", 120],
  ["grape", 130],
];const chart = anychart.column();
const series = chart.column(data);
chart.container("container");
chart.draw();
```

`anychart.column`创建柱形图。

`chart.column`获取图表数据。

代码的其余部分与前面的示例相同。

我们可以设置列的填充颜色。

例如，我们可以写:

```
const data = [
  ["apple", 100],
  ["orange", 120],
  ["grape", 130],
];const chart = anychart.column();
const series = chart.column(data);
series.normal().fill("#0066cc");
series.hovered().fill("#0066cc", 2);
series.selected().fill("#0066cc", 4);
chart.container("container");
chart.draw();
```

我们用`fill`方法设置条形的填充颜色，以响应各种动作。

此外，我们可以使用`stroke`方法设置轮廓颜色以响应各种动作:

```
const data = [
  ["apple", 100],
  ["orange", 120],
  ["grape", 130],
];const chart = anychart.column();
const series = chart.column(data);
series.normal().stroke("#0066cc");
series.hovered().stroke("#0066cc", 2);
series.selected().stroke("#0066cc", 4);
chart.container("container");
chart.draw();
```

`hatchFill`方法让我们给列添加阴影背景:

```
const data = [
  ["apple", 100],
  ["orange", 120],
  ["grape", 130],
];const chart = anychart.column();
const series = chart.column(data);
series.normal().hatchFill("forward-diagonal", "#0066cc", 1, 15);
series.hovered().hatchFill("forward-diagonal", "#0066cc", 1, 15);
series.selected().hatchFill("forward-diagonal", "#0066cc", 1, 15);
chart.container("container");
chart.draw();
```

# 结论

我们可以用任何图表添加气泡图、项目符号图和柱形图。