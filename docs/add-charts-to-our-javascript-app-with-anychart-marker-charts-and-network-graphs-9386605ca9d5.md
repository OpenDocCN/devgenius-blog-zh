# 使用 Anychart 将图表添加到我们的 JavaScript 应用程序中——标记图和网络图

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-marker-charts-and-network-graphs-9386605ca9d5?source=collection_archive---------12----------------------->

![](img/b7f06831262c66033293cd45fd264ed5.png)

照片由[你好我是尼克🎞](https://unsplash.com/@helloimnik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 标记图表

我们可以通过编写以下代码来创建标记图:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>chart</title>
  </head>
  <body>
    <script
      src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js"
      type="text/javascript"
    ></script>
    <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const data = [
        ["2000", 1100],
        ["2001", 880],
        ["2002", 1100],
        ["2003", 1500],
        ["2004", 921]
      ];
      const chart = anychart.cartesian();
      chart.marker(data);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

我们在`data`数组中有数据。

每个条目的第一个值是 x 轴值，第二个值是 y 轴值。

`anychart.cartesian`创建标记图。

`chart.marker`设置数据。

`chart.container`设置要在其中呈现图表的容器元素的 ID。

`chart.draw`绘制图表。

我们可以用`size`方法设置标记的大小:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>chart</title>
  </head>
  <body>
    <script
      src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js"
      type="text/javascript"
    ></script>
    <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const data = [
        ["2000", 1100],
        ["2001", 880],
        ["2002", 1100],
        ["2003", 1500],
        ["2004", 921]
      ];
      const chart = anychart.cartesian();
      const series = chart.marker(data);
      series.normal().size(10);
      series.hovered().size(15);
      series.selected().size(15);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

我们可以用`type`方法将市场设置成不同的形状:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>chart</title>
  </head>
  <body>
    <script
      src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js"
      type="text/javascript"
    ></script>
    <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const data = [
        ["2000", 1100],
        ["2001", 880],
        ["2002", 1100],
        ["2003", 1500],
        ["2004", 921]
      ];
      const chart = anychart.cartesian();
      const series = chart.marker(data);
      series.normal().type("star4");
      series.hovered().type("star5");
      series.selected().type("star6");
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

# 网络图

我们可以很容易地用 Anychart 添加网络图。

例如，我们可以写

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>chart</title>
  </head>
  <body>
    <script
      src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js"
      type="text/javascript"
    ></script>
    <script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-graph.min.js"></script>
    <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const data = {
        nodes: [
          { id: "Richard" },
          { id: "Larry" },
          { id: "Marta" },
          { id: "Jane" },
          { id: "Norma" }
        ],
        edges: [
          { from: "Richard", to: "Larry" },
          { from: "Richard", to: "Marta" },
          { from: "Larry", to: "Marta" },
          { from: "Marta", to: "Jane" },
          { from: "Jane", to: "Norma" },
          { from: "Brett", to: "Frank" }
        ]
      };
      const chart = anychart.graph(data);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

我们添加了带有第二个脚本标签的 Anychart 图形模块。

然后我们添加`data`数组来添加数据。

我们定义了`nodes`数组中的节点。

我们定义它们如何与`edges`数组连接。

`from`是要连接的起始节点，`to`是要连接的目标节点。

然后，我们只需将数据传递给`anychart.graph`，并像绘制任何其他类型的图表一样绘制它。

# 结论

我们可以用 Anychart 添加标记图和网络图。