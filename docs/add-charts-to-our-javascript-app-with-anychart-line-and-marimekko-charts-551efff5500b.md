# 使用 any chart-Line 和 Marimekko 图表向我们的 JavaScript 应用程序添加图表

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-line-and-marimekko-charts-551efff5500b?source=collection_archive---------7----------------------->

![](img/68997cf1fc84ed0c9ced2b09f5c09c49.png)

Joel Mbugua 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 折线图

我们可以用 Anychart 添加折线图。

要添加一个，我们编写以下内容:

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
        ["January", 100],
        ["February", 120],
        ["March", 180]
      ]; const chart = anychart.line();
      const series = chart.line(data);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

我们添加第一个脚本标记来添加 Anychart 库。

在此之下，我们为图表容器添加 div。

在此之下，我们为折线图添加脚本标记。

`data`有一个数组，数组条目中有 x 轴和 y 轴的值。

`anychart.line`创建折线图。

`chart.line`为折线图添加数据。

`chart.container`用里面的图表容器元素的 ID 调用，在容器中呈现图表。

`chart.draw`绘制图表。

# Marimekko 图表

我们可以用任何图表创建 Marimekko 图表。

为此，我们写道:

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
    <script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-mekko.min.js"></script> <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const data = anychart.data.set([
        ["QTR1", 10000, 12500],
        ["QTR2", 12000, 15000],
        ["QTR3", 13000, 16500],
        ["QTR4", 10000, 13000]
      ]);
      const series1 = data.mapAs({ x: 0, value: 1 });
      const series2 = data.mapAs({ x: 0, value: 2 });
      chart = anychart.mekko();
      chart.mekko(series1);
      chart.mekko(series2);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

我们添加了带有第二个脚本标签的`mekko`包。

然后我们用图表的数据调用`anychart.darta.set`。

然后我们用一个对象调用`data.mapAs`，将 x 轴映射到数组中值的索引。

`x`设置为 0，所以它从它上面的数据数组中取值。

`value`分别设置为 1 和 2，因此取 1 和 2 的值作为数据。

然后我们调用`anychart.mekko`来创建图表。

我们用`series1`和`series2`数据调用`chart.mekko`来设置图表数据。

代码的其余部分与前面的示例相同。

# 条形图

我们可以使用`barmekko`方法创建条形图。

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
    <script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-mekko.min.js"></script> <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const data = [
        { x: "Spring", value: 20 },
        { x: "Summer", value: 30 },
        { x: "Autumn", value: 80 },
        { x: "Winter", value: 40 }
      ];
      const chart = anychart.barmekko();
      const series = chart.mekko(data);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

我们有一个`data`数组，其中的对象分别具有 x 轴和 y 轴数据。

然后我们调用`anychart.barmekko`来创建条形图。

接下来，我们调用`chart.mekko`来设置数据。

代码的其余部分与前面的示例相同。

# 结论

我们可以使用 Anychart 轻松创建折线图、Marimekko 图和条形图。