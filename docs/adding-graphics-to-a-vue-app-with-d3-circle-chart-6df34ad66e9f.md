# 使用 D3 向 Vue 应用添加图形—圆形图

> 原文：<https://blog.devgenius.io/adding-graphics-to-a-vue-app-with-d3-circle-chart-6df34ad66e9f?source=collection_archive---------5----------------------->

![](img/fff32a568b353606ce49915b82446d4a.png)

马丁·雷施在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

D3 让我们可以轻松地向前端 web 应用添加图形。

Vue 是一个流行的前端 web 框架。

他们合作得很好。在本文中，我们将了解如何使用 D3 为 Vue 应用添加图形。

# 圆形图表

我们可以用 D3 在我们的 Vue 应用程序中添加一个圆。

为此，我们写道:

```
<template>
  <div id="app">
    <div id="svgcontainer"></div>
  </div>
</template>
<script>
import * as d3 from "d3";
import Vue from "vue";
export default {
  name: "App",
  mounted() {
    Vue.nextTick(() => {
      const width = 400;
      const height = 400;
      const data = [10, 24, 35];
      const colors = ["green", "lightblue", "yellow"]; const svg = d3
        .select("body")
        .append("svg")
        .attr("width", width)
        .attr("height", height); const g = svg
        .selectAll("g")
        .data(data)
        .enter()
        .append("g")
        .attr("transform", function (d, i) {
          return "translate(0,0)";
        }); g.append("circle")
        .attr("cx", function (d, i) {
          return i * 75 + 50;
        })
        .attr("cy", function (d, i) {
          return 75;
        })
        .attr("r", function (d) {
          return d * 1.5;
        })
        .attr("fill", function (d, i) {
          return colors[i];
        }); g.append("text")
        .attr("x", function (d, i) {
          return i * 75 + 25;
        })
        .attr("y", 80)
        .attr("stroke", "teal")
        .attr("font-size", "10px")
        .attr("font-family", "sans-serif")
        .text(function (d) {
          return d;
        });
    });
  },
};
</script>
```

我们首先用`width`和`height`变量为 SVG 创建常数。

`data`有我们想要显示的数据。

`colors`每个圆圈都有颜色。

我们使用`width`和`height`来创建 SVG:

```
const svg = d3
  .select("body")
  .append("svg")
  .attr("width", width)
  .attr("height", height);
```

然后我们添加 SVG 组`g`,包含:

```
const g = svg
  .selectAll("g")
  .data(data)
  .enter()
  .append("g")
  .attr("transform", function(d, i) {
    return "translate(0,0)";
  });
```

然后，我们添加圆圈:

```
g.append("circle")
  .attr("cx", function(d, i) {
    return i * 75 + 50;
  })
  .attr("cy", function(d, i) {
    return 75;
  })
  .attr("r", function(d) {
    return d * 1.5;
  })
  .attr("fill", function(d, i) {
    return colors[i];
  });
```

我们设置`cx`和`cy`属性来添加圆心的 x 和 y 坐标。

然后我们添加`r`属性来添加半径。半径与`data`中的数字成比例，因此数字越大，半径越大。

那么`fill`是`colors`数组中的一个条目。

最后，我们添加了数字显示:

```
g.append("text")
  .attr("x", function(d, i) {
    return i * 75 + 25;
  })
  .attr("y", 80)
  .attr("stroke", "teal")
  .attr("font-size", "10px")
  .attr("font-family", "sans-serif")
  .text(function(d) {
    return d;
  });
```

文本的`x`和`y`属性是文本位置的 x 和 y 坐标。

`stroke`有文字颜色。

`font-size`是字体的大小。`font-family`是字体类型。

`text`有一个回调函数，它返回要显示的文本。

# 结论

我们可以用 D3 在我们的 Vue 应用程序中添加一个圆形图表。