# 使用 D3 向 React 应用添加图形—CSV 和 tsv

> 原文：<https://blog.devgenius.io/adding-graphics-to-a-react-app-with-d3-csvs-and-tsvs-16094a9d157f?source=collection_archive---------7----------------------->

![](img/018f9040796065402223fcd8c80d06ea.png)

Rami Al-zayat 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

D3 让我们可以轻松地向前端 web 应用添加图形。

Vue 是一个流行的前端 web 框架。

他们合作得很好。在本文中，我们将了解如何使用 D3 为 Vue 应用添加图形。

# csvParseRows

我们可以使用`csvParseRows`方法将 CSV 字符串解析成一个对象数组。

例如，我们可以写:

```
import React, { useEffect } from "react";
import * as d3 from "d3";const string = `2011,10\n2012,20\n2013,30\n`;export default function App() {
  useEffect(() => {
    const data = d3.csvParseRows(string, function ([year, population]) {
      return {
        year: new Date(+year, 0, 1),
        population
      };
    });
    console.log(data);
  }, []); return <div className="App"></div>;
}
```

我们用 CSV 字符串作为第一个参数调用`csvParseRows`。

回调函数有经过解析的条目，我们可以从中返回我们想要的东西。

# CSV 格式

`csvFormat`方法格式化 CSV 的行和列。

例如，我们可以写:

```
import React, { useEffect } from "react";
import * as d3 from "d3";const data = [
  { year: 2011, population: 10 },
  { year: 2012, population: 20 },
  { year: 2013, population: 30 }
];
export default function App() {
  useEffect(() => {
    const string = d3.csvFormat(data, ["year", "population"]);
    console.log(string);
  }, []); return <div className="App"></div>;
}
```

我们传入一个对象数组作为第一个参数。

然后我们传入一个包含列名的字符串数组。

然后我们得到`string`的值，也就是 CSV 字符串。它的值是:

```
year,population
2011,10
2012,20
2013,30
```

# tsvParse

`tsvParse`方法让我们解析制表符分隔的字符串。

例如，我们可以写:

```
import React, { useEffect } from "react";
import * as d3 from "d3";const string = `year\tpopulation\n2011\t10\n2012\t20\n2013\t30\n`;export default function App() {
  useEffect(() => {
    const data = d3.tsvParse(string, function ({ year, population }) {
      return {
        year: new Date(+year, 0, 1),
        population
      };
    });
    console.log(data);
  }, []); return <div className="App"></div>;
}
```

我们有一个制表符分隔的字符串，并用它调用`tsvParse`。

然后在回调时，我们从参数中解析的条目中析构`year`和`population`属性。

我们返回对象来返回我们想要的。

# tsvParseRows

`tsvParseRows`方法让我们解析不带标题的制表符分隔的字符串行。

例如，我们可以写:

```
import React, { useEffect } from "react";
import * as d3 from "d3";const string = `2011\t10\n2012\t20\n2013\t30\n`;export default function App() {
  useEffect(() => {
    const data = d3.tsvParseRows(string, function ([year, population]) {
      return {
        year: new Date(+year, 0, 1),
        population
      };
    });
    console.log(data);
  }, []);return <div className="App"></div>;
}
```

我们传入一个不带标题的字符串。

然后我们用字符串和回调函数调用`tsvParseRows`，回调函数具有被解析行的属性。

我们为每个条目返回我们想要返回的对象。

# 结论

我们可以用 D3 解析和创建 CSV 和 tsv，并在 React 应用程序中使用它们。