# 使用 React 和 JavaScript 创建 PIN Pad

> 原文：<https://blog.devgenius.io/create-a-pin-pad-with-react-and-javascript-de80f22de5f1?source=collection_archive---------3----------------------->

![](img/66ccc14e9ae9ccf7a283366aace43637.png)

照片由 [CHUTTERSNAP](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 是一个易于使用的 JavaScript 框架，让我们可以创建前端应用程序。

在本文中，我们将看看如何用 React 和 JavaScript 创建一个密码板。

# 创建项目

我们可以用 Create React App 创建 React 项目。

要安装它，我们运行:

```
npx create-react-app pin-pad
```

和 NPM 一起创建我们的 React 项目。

# 创建 PIN Pad

为了创建 PIN pad，我们编写:

```
import React, { useState } from "react";export default function App() {
  const [pin, setPin] = useState(""); return (
    <div>
      <div>{pin}</div>
      <div>
        <div>
          <button onClick={() => setPin((pin) => `${pin}1`)}>1</button>
          <button onClick={() => setPin((pin) => `${pin}2`)}>2</button>
          <button onClick={() => setPin((pin) => `${pin}3`)}>3</button>
        </div>
        <div>
          <button onClick={() => setPin((pin) => `${pin}4`)}>4</button>
          <button onClick={() => setPin((pin) => `${pin}5`)}>5</button>
          <button onClick={() => setPin((pin) => `${pin}6`)}>6</button>
        </div>
        <div>
          <button onClick={() => setPin((pin) => `${pin}7`)}>7</button>
          <button onClick={() => setPin((pin) => `${pin}8`)}>8</button>
          <button onClick={() => setPin((pin) => `${pin}9`)}>9</button>
        </div>
        <div>
          <button onClick={() => setPin((pin) => pin.slice(0, pin.length - 1))}>
            &lt;
          </button>
          <button onClick={() => setPin((pin) => `${pin}0`)}>0</button>
          <button onClick={() => setPin("")}>C</button>
        </div>
      </div>
    </div>
  );
}
```

我们创建了`pin`状态来保存单击按钮时创建的字符串。

然后我们显示`pin`值。

然后我们将设置了`onClick`属性的按钮添加到一个函数中，该函数通过一个返回`pin`字符串的回调函数来调用`setPin`。

setPin 回调函数，它返回删除了最后一个字符的`pin`字符串。

而 C 按钮有一个`setPin`回调，将`pin`重置为空字符串。

# 结论

我们可以使用 React 和 JavaScript 轻松创建密码板。