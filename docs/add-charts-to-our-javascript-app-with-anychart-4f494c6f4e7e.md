# 使用 Anychart 向我们的 JavaScript 应用程序添加图表

> 原文：<https://blog.devgenius.io/add-charts-to-our-javascript-app-with-anychart-4f494c6f4e7e?source=collection_archive---------6----------------------->

![](img/50413be6d13785558a225e5fab5f8397.png)

照片由[艾萨克·史密斯](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Anychart 是一个易于使用的库，允许我们将图表添加到 JavaScript web 应用程序中。

在本文中，我们将了解如何使用 Anychart 创建基本图表。

# 创建我们的第一个图表

我们可以通过添加脚本标记来添加 Anychart 库，从而创建我们的第一个图表:

```
<script src="https://cdn.anychart.com/releases/8.9.0/js/anychart-base.min.js" type="text/javascript"></script>
```

然后，我们为图表添加一个容器元素:

```
<div id="container" style="width: 500px; height: 400px;"></div>
```

我们设置了稍后将会用到的`id`。

然后要创建一个饼图，我们写:

```
anychart.onDocumentLoad(() => {
   const chart = anychart.pie();
   chart.data([
     ["apple", 5],
     ["orange", 2],
     ["grape", 2],     
   ]);   
   chart.title("Most popular fruits");   
   chart.container("container");   
   chart.draw();
 });
```

我们用`anychart.pie()`创建一个饼图。

然后我们调用`chart.data`将我们的数据添加到一个数组中。

`chart.title`设置图表标题。

`chart.container`设置用于呈现图表的容器的 ID。

`chart.draw`绘制图表。

# 从 XML 加载数据

我们可以从 XML 加载数据。

例如，我们可以编写以下 HTML:

```
<div id="container" style="width: 500px; height: 400px;"></div>
```

和下面的 JavaScript 代码:

```
const xmlString = `
<xml>
 <chart type="pie" >
  <data>
   <point name="apple" value="1222"/>
   <point name="orange" value="2431"/>
   <point name="grape" value="3624"/>
  </data>
 </chart>
</xml>
`anychart.onDocumentLoad(() => {
  const chart = anychart.fromXml(xmlString);
  chart.title("Most popular fruits");
  chart.container("container");
  chart.draw();
});
```

添加呈现给定 XML 字符串的饼图。

我们将`chart`标签的`type`属性设置为`'pie'`。

`data`标签包含`point`元素，这些元素包含数据。

`name`有键，`value`有值。

# 从 JSON 加载数据

要从 JSON 对象加载数据，我们可以使用`anychart.fromJSON`方法。

我们保持和以前一样的 HTML，并编写以下 JavaScript:

```
const json = {
  "chart": {
    "type": "pie",
    "data": [
      ["apple", 1222],
      ["orange", 2431],
      ["grape", 3624],
    ]
  }
};anychart.onDocumentLoad(() => {
  const chart = anychart.fromJson(json);
  chart.title("Most popular fruits");
  chart.container("container");
  chart.draw();
});
```

我们有一个包含图表数据的属性为`chart`的`json`对象。

`type`有要呈现的图表类型。

`data`有一个包含数据的键值数组。

# 从 CSV 加载数据

我们可以用`chart.title`方法从 CSV 加载图表数据。

例如，我们可以写:

```
const csvString = `
  2009-02-06,6764\n 
  2009-02-07,7056\n 
  2009-02-08,7180\n 
`anychart.onDocumentLoad(() => {
  const chart = anychart.area();
  chart.area(csvString);
  chart.title("Profit growth");
  chart.container("container");
  chart.draw();
});
```

我们有一个包含 CSV 列的字符串的`csvString`变量。

第一个是 x 轴值。

第二列是 y 轴值。

然后我们将`csvString`传递给`chart.area`使用`csvString`的数据来绘制我们的面积图。

# 结论

我们可以使用 Anychart 轻松呈现来自各种数据源的图表。