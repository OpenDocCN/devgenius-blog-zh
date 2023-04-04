# 使用 Anychart 向我们的 JavaScript 应用程序添加图表—散点图、散点图和散点图

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-scatter-line-charts-scatter-marker-charts-and-69eb4cf8facf?source=collection_archive---------8----------------------->

![](img/0129d897db7583ee372fe0c5a5ca8f5e.png)

图片由 [Erol Ahmed](https://unsplash.com/@erol?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 散点图

我们可以用 Anychart 添加散点图。

例如，我们可以添加以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

然后我们可以添加以下 JavaScript 代码:

```
const data = [{
    x: 1.3,
    value: 39,
  },
  {
    x: 2.5,
    value: 42,
  },
  {
    x: 3.2,
    value: 54,
  },
  {
    x: 3.9,
    value: 38,
  },
  {
    x: 5.1,
    value: 58,
  }
];const chart = anychart.scatter();
const series = chart.line(data);
chart.container("container");
chart.draw();
```

我们添加脚本标签来添加 Anychart 基础包。

div 是呈现图表的容器。

然后我们添加`data`数组来将数据添加到我们的图表中。

每个条目都有带有 x 轴值的`x`属性。

`value`属性具有 y 轴值。

然后我们调用`anychart.scatter`来创建散点图。

`chart.line`让我们给散点图添加一条线。

我们还使用方法设置要呈现的数据。

`chart.container`设置要在其中呈现图表的容器的 ID。

`chart.draw`绘制图表。

# 散点图

我们可以使用以下代码创建散点标记图:

```
const data = [{
    x: 1.3,
    value: 39,
  },
  {
    x: 2.5,
    value: 42,
  },
  {
    x: 3.2,
    value: 54,
  },
  {
    x: 3.9,
    value: 38,
  },
  {
    x: 5.1,
    value: 58,
  }
];const chart = anychart.scatter();
const series = chart.marker(data);
chart.container("container");
chart.draw();
```

和前面的例子一样，我们有一个`data`数组。

我们再次调用`anychart.scatter`来创建散点图。

让我们将标记添加到图表中。

我们用这个方法设置要呈现的数据。

代码的其余部分与前面的示例相同。

# 桑基图

我们可以用 Anychart 添加一个 Sankey 图。

为了添加图表，我们编写以下 HTML:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script>
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-sankey.min.js"></script><div id="container" style="width: 500px; height: 400px;"></div>
```

然后，我们添加以下 JavaScript 代码:

```
const data = [{
    from: "Canada",
    to: "France",
    weight: 434
  },
  {
    from: "Canada",
    to: "Germany",
    weight: 343
  },
  {
    from: "Canada",
    to: "Italy",
    weight: 434
  },
  {
    from: "Canada",
    to: "Spain",
    weight: 213
  },
  {
    from: "USA",
    to: "France",
    weight: 341
  },
  {
    from: "USA",
    to: "Germany",
    weight: 343
  },
  {
    from: "USA",
    to: "Spain",
    weight: 232
  }
];const chart = anychart.sankey(data);
chart.nodeWidth("30%");
chart.container("container");
chart.draw();
```

我们添加了 Anychart 基础包和带有脚本标记的 Sankey 包。

div 与前面的示例相同。

然后我们在每个条目中添加带有`from`、`to`和`weight`属性的`data`数组。

`from`和`to`分别具有从哪条线到哪条线的值。

`weight`是线的重量。

接下来，我们调用`anychart.sankey`来创建桑基图表。

`chart.nodeWidth`有节点的宽度。

代码的其余部分与前面的示例相同。

# 结论

我们可以使用 Anychart 轻松添加散点图、散点图和散点图。