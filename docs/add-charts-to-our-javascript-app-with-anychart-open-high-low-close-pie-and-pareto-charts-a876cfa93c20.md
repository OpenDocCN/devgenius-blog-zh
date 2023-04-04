# 使用 Anychart 向我们的 JavaScript 应用程序添加图表—开盘-盘高-盘低-收盘图、饼图和帕累托图

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-open-high-low-close-pie-and-pareto-charts-a876cfa93c20?source=collection_archive---------18----------------------->

![](img/0e4bd29857cee6b4e1594aa4a108b08c.png)

[sheri silver](https://unsplash.com/@sheri_silver?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 开盘-盘高-盘低-收盘(OHLC)图

我们可以很容易地用任何图表添加 OHLC 图表。

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
    <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const data = [
        [Date.UTC(2020, 07, 23), 23.55, 23.88, 23.38, 23.62],
        [Date.UTC(2020, 07, 24), 22.65, 23.7, 22.65, 23.36],
        [Date.UTC(2020, 07, 25), 22.75, 23.7, 22.69, 23.44],
        [Date.UTC(2020, 07, 26), 23.2, 23.39, 22.87, 22.92],
        [Date.UTC(2020, 07, 27), 23.98, 24.49, 23.47, 23.49]
      ]; const chart = anychart.ohlc();
      const series = chart.ohlc(data);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

我们添加用于添加 Anychart 基础包的脚本标记。

然后我们添加一个 div 来呈现里面的图表。

接下来，我们用日期和开盘、盘高、盘低和收盘数据按此顺序创建`data`数组。

然后我们调用`anychart.ohlc`来创建图表。

接下来，我们调用`chart.ohlc`来设置图表数据。

`chart.container`让我们设置图表容器元素的 ID。

并且`chart.draw`在容器元素中绘制图表。

# 帕累托图

要添加帕累托图，我们写:

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
    <script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-pareto.min.js"></script>
    <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const chart = anychart.pareto([
        { x: "apple", value: 19 },
        { x: "orange", value: 9 },
        { x: "grape", value: 28 }
      ]);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

我们用想要呈现的数据调用`anychart.pareto`。

`x`有 x 轴数值。

`value`有 y 轴数值。

代码的其余部分与其他图表相同。

我们必须添加`pareto`包来呈现帕累托图。

# 圆形分格统计图表

我们可以用任何图表添加一个饼图。

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
    <div id="container" style="width: 500px; height: 400px;"></div>
    <script>
      const data = [
        { x: "A", value: 677 },
        { x: "B", value: 243 },
        { x: "C", value: 431 }
      ];
      const chart = anychart.pie(data);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

我们用饼图值创建了`data`数组。

`x`有饼图标签。

`value`有饼图块值。

我们将`data`数组传递给`anychart.pie`来创建图表。

我们用和前面例子一样的方法画它们。

我们可以分别用`fill`和`stroke`设置饼图块的填充和轮廓颜色:

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
        { x: "A", value: 677 },
        { x: "B", value: 243 },
        { x: "C", value: 431 }
      ];
      const chart = anychart.pie(data);
      chart.normal().fill("#669999", 0.5);
      chart.hovered().fill("#666699", 0.5);
      chart.selected().fill("#666699", 0.7);
      chart.normal().stroke("#669999", 2);
      chart.hovered().stroke("#669999", 2);
      chart.container("container");
      chart.draw();
    </script>
  </body>
</html>
```

# 结论

我们可以创建 OHLC，排列图和饼图很容易与任何图表。