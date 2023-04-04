# 用 React 和 JavaScript 创建一个秒表应用

> 原文：<https://blog.devgenius.io/create-a-stopwatch-app-with-react-and-javascript-5e619d4d11b3?source=collection_archive---------2----------------------->

![](img/16b16eaf106ab0667ec54f3dc108338f.png)

照片由[将](https://unsplash.com/@will0629?utm_source=medium&utm_medium=referral)放在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 是一个易于使用的 JavaScript 框架，让我们可以创建前端应用程序。

在本文中，我们将看看如何用 React 和 JavaScript 创建一个秒表应用程序。

# 创建项目

我们可以用 Create React App 创建 React 项目。

要安装它，我们运行:

```
npx create-react-app stopwatch
```

和 NPM 一起创建我们的 React 项目。

此外，我们必须安装矩库，使计算和格式化持续时间更容易。

要安装它，我们运行:

```
npm i moment
```

# 创建秒表应用程序

为了创建秒表，我们编写:

```
import React, { useState } from "react";
import moment from "moment";export default function App() {
  const [startDate, setStartDate] = useState(new Date());
  const [diff, setDiff] = useState("00:00:00");
  const [timer, setTimer] = useState(); return (
    <div className="App">
      <button
        onClick={() => {
          setStartDate(new Date());
          const timer = setInterval(() => {
            let start = moment(startDate);
            let end = moment(new Date());
            let diff = end.diff(start);
            let f = moment.utc(diff).format("HH:mm:ss.SSS");
            setDiff(f);
          }, 1000);
          setTimer(timer);
        }}
      >
        start
      </button>
      <button onClick={() => clearInterval(timer)}>stop</button>
      <p>{diff}</p>
    </div>
  );
}
```

我们用`startDate`状态来存储开始日期时间。

`diff`将经过的时间存储为字符串。

`timer`拥有计时器对象。

然后我们添加一个按钮，该按钮具有调用`setStartDate`来设置开始日期的功能。

然后我们用`setInterval`创建一个每秒运行一次的计时器。

在`setInterval`回调中，我们从`startDate`和当前日期时间创建`moment`对象。

我们调用`moment`来创建那些对象。

然后，我们用`diff`方法得到它们之间经过的时间。

然后我们用`moment.utc`方法和`format`方法将经过的时间格式化成一个字符串。

同样，我们调用`setTimer`来设置`timer`状态到我们的`setInterval`定时器。

接下来，我们使用停止按钮调用`clearInterval`来停止计时器。

并且`p`元素显示格式化的经过时间字符串。

# 结论

我们可以用 React 和 JavaScript 轻松创建秒表。