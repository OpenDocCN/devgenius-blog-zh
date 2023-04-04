# 使用 Anychart 将图表添加到我们的 JavaScript 应用程序中——迷你图、样条图和样条面积图

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-sparklines-spline-charts-and-spline-area-charts-e202ccb7865e?source=collection_archive---------6----------------------->

![](img/86993d26cb25c6855ca196bda1a697bd.png)

照片由 [Jez Timms](https://unsplash.com/@jeztimms?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 迷你图

我们可以用任何图表轻松创建一个闪光点。

要添加一个，我们编写以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script>
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-sparkline.min.js"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

然后，我们添加以下 JavaScript 代码:

```
const stage = anychart.graphics.create('container');
const chart = anychart.sparkline();
chart.container(stage);
chart.data([1.1371, 1.1341, 1.1132, 1.1381, 1.1371]);
chart.bounds(0, 0, 90, 20);
chart.draw();
```

我们为 Anychart 基础包和迷你图包添加脚本标记。

然后，我们添加 div，它将用作容器元素，用于在中呈现迷你图。

接下来，我们用`anychart.graphics.create`方法创建`stage`对象。

我们传入容器元素的 ID 来呈现迷你图。

然后我们调用`anychart.sparkline`来创建迷你图。

我们通过调用`chart.container`将它添加到`stage`中。

`chart.data`为图表设置数据。

`chart.bounds`分别设置迷你图的左上角和右下角。

数字以像素为单位。

`chart.draw`绘制迷你图。

# 样条面积图

我们可以用 Anychart 添加一个样条面积图。

例如，我们可以编写以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

和下面的 JavaScript 代码:

```
const data = [{
    x: "January",
    value: 43
  },
  {
    x: "February",
    value: 46
  },
  {
    x: "March",
    value: 12
  },
  {
    x: "April",
    value: 65
  },
  {
    x: "May",
    value: 71
  }
];const chart = anychart.area();
const series = chart.splineArea(data);
chart.container("container");
chart.draw();
```

我们只需要 Anychart 基础包来创建样条面积图。

容器 div 与前面的示例相同。

接下来，我们用图表的数据创建`data`数组。

`x`有 x 轴数值。

`value`有 y 轴数值。

`anychart.area`创建面积图。

`chart.splineArea`添加带数据的样条面积图。

代码的其余部分与前面的示例相同。

# 样条图

我们可以用`anychart.line`方法创建一个样条图。

例如，我们可以写:

```
const data = [{
    x: "January",
    value: 43
  },
  {
    x: "February",
    value: 46
  },
  {
    x: "March",
    value: 12
  },
  {
    x: "April",
    value: 65
  },
  {
    x: "May",
    value: 71
  }
];const chart = anychart.line();
const series = chart.spline(data);
chart.container("container");
chart.draw();
```

我们调用`anychart.line`来创建折线图。

然后我们用`chart.spline`创建样条图。

代码的其余部分与其他示例相同。

# 结论

我们可以用 Anychart 创建迷你图、样条图和样条面积图。