# 使用 Anychart 将图表添加到我们的 JavaScript 应用程序中——极坐标图、折线图和金字塔图

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-polar-polyline-and-pyramid-charts-2dba264424c2?source=collection_archive---------3----------------------->

![](img/8370e19d97f2b867fa73fd8acf07dca8.png)

杰里米·毕晓普在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 极区图

我们可以用 Anychart 添加一个极坐标图。

例如，我们可以写:

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
    <script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-polar.min.js"></script>
    <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const data = [
        ["Strength", 8],
        ["Dexterity", 14],
        ["Concentration", 14],
        ["Intelligence", 15],
        ["Wisdom", 12]
      ];
      const chart = anychart.polar();
      const series = chart.polygon(data);
      chart.xScale("ordinal");
      chart.sortPointsByX(true);
      chart.innerRadius(50);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

我们为基于 Anychart 的包和极坐标图包添加脚本标签。

然后我们用描述和它们的值来定义`data`数组。

`anychart.polar`创建极坐标图。

`chart.polygon`为购物车设置数据。

`chart.xScale`设置 x 轴的刻度。

`chart.innerRadius`设置图表的内径。

`chart.container`设置用于呈现图表的容器的 ID。

`chart.draw`绘制图表。

# 折线图表

折线图表类似于极坐标图表，只是多边形内部是空心的。

为了补充它，我们写道:

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
    <script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-polar.min.js"></script>
    <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const data = [
        ["Strength", 8],
        ["Dexterity", 14],
        ["Concentration", 14],
        ["Intelligence", 15],
        ["Wisdom", 12]
      ];
      const chart = anychart.polar();
      const series = chart.polyline(data);
      chart.xScale("ordinal");
      chart.sortPointsByX(true);
      chart.innerRadius(50);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

与前面例子的不同之处在于，我们调用`chart.polyline`而不是`chart.polygon`。

并且我们调用`sortPointsByX`按照 x 轴的值对点进行排序。

# 金字塔图表

我们可以用 Anychart 轻松添加一个金字塔图。

例如，我们可以写:

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
    <script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-pyramid-funnel.min.js"></script>
    <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const chart = anychart.pyramid([
        { name: "apple", value: 490 },
        { name: "orange", value: 435 },
        { name: "grape", value: 566 },
        { name: "banana", value: 324 },
        { name: "pear", value: 455 }
      ]);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

我们添加了带有第二个脚本标签的`pyramid`包。

然后我们用图表数据调用`anychart.pyramid`。

`value`属性用于确定金字塔的大小。

`name`是标签。

然后剩下的和其他图表一样。

# 结论

我们可以使用 Anychart 轻松添加极坐标图、折线图和金字塔图。