# Top React 库—格式化数字和无限滚动

> 原文：<https://blog.devgenius.io/top-react-libraries-formatting-numbers-and-infinite-scrolling-d52a23c205c?source=collection_archive---------9----------------------->

![](img/144d690c20d215ae4f06073e846f2403.png)

由 [Andrew Buchanan](https://unsplash.com/@photoart2018?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# 反应数字格式

react-number-format 包让我们以自己的方式格式化数字。

要安装它，我们运行:

```
npm i react-number-format
```

然后我们可以使用`NumberFormat`组件来显示按照我们的方式格式化的数字。

例如，我们可以写:

```
import React from "react";
import NumberFormat from "react-number-format";export default function App() {
  return (
    <div className="App">
      <NumberFormat
        value={585950}
        displayType={"text"}
        thousandSeparator={true}
        prefix={"$"}
      />
    </div>
  );
}
```

`value`有我们要格式化的值。

`displayType`是我们想要显示的数字。

`thousandSeparator`表示我们想要一个分隔符。

`prefix`在格式化数字前有符号。

我们也可以用`renderText`道具添加我们自己的自定义渲染功能:

```
import React from "react";
import NumberFormat from "react-number-format";export default function App() {
  return (
    <div className="App">
      <NumberFormat
        value={57595995}
        displayType={"text"}
        thousandSeparator={true}
        prefix={"$"}
        renderText={value => <div>{value}</div>}
      />
    </div>
  );
}
```

我们可以输入自己的格式字符串:

```
import React from "react";
import NumberFormat from "react-number-format";export default function App() {
  return (
    <div className="App">
      <NumberFormat
        value={4111111111111111}
        displayType={"text"}
        format="#### #### #### ####"
      />
    </div>
  );
}
```

如果我们没有将`displayType`设置为`'text'`，那么它将显示为输入:

```
import React from "react";
import NumberFormat from "react-number-format";export default function App() {
  return (
    <div className="App">
      <NumberFormat format="#### #### #### ####" />
    </div>
  );
}
```

我们还有`format`属性来指定如何格式化数字。

我们也可以用`mask`道具为面具添加一个角色:

```
import React from "react";
import NumberFormat from "react-number-format";export default function App() {
  return (
    <div className="App">
      <NumberFormat format="#### #### #### ####" mask="_" />
    </div>
  );
}
```

这让我们用`_`字符显示提示。

我们可以用它做更多的事情，比如操作元素本身和添加自定义输入。

# 反应无限滚动器

React Infinite Scroller 是 React 应用程序的一个无限滚动组件。

要安装它，我们可以运行:

```
npm i react-infinite-scroller
```

然后我们可以通过写来使用它:

```
import React from "react";
import InfiniteScroll from "react-infinite-scroller";export default function App() {
  const [arr, setArr] = React.useState(
    Array(50)
      .fill()
      .map((_, i) => i)
  );
  return (
    <div className="App">
      <InfiniteScroll
        pageStart={0}
        loadMore={() =>
          setArr(arr => [
            ...arr,
            ...Array(50)
              .fill()
              .map((_, i) => i)
          ])
        }
        hasMore
        loader={
          <div className="loader" key={0}>
            Loading ...
          </div>
        }
      >
        {arr.map(a => (
          <p>{a}</p>
        ))}
      </InfiniteScroll>
    </div>
  );
}
```

当我们到达页面底部时，我们使用带有`loadMore`属性的`InfiniteScroll`组件来加载更多数据。

`loader` prop 具有显示何时加载数据的组件。

`pageStart`已加载起始页。

`hasMore`表示我们是否有更多项目要加载。

在标签之间，我们显示带有代码的商品。

此外，我们可以将`useWindow`属性设置为`false`来监听 DOM 滚动事件。

例如，我们可以写:

```
import React from "react";
import InfiniteScroll from "react-infinite-scroller";export default function App() {
  const [arr, setArr] = React.useState(
    Array(50)
      .fill()
      .map((_, i) => i)
  );
  return (
    <div className="App">
      <div style={{ height: 300, overflow: "auto" }}>
        <InfiniteScroll
          pageStart={0}
          loadMore={() =>
            setArr(arr => [
              ...arr,
              ...Array(50)
                .fill()
                .map((_, i) => i)
            ])
          }
          hasMore
          useWindow={false}
          loader={
            <div className="loader" key={0}>
              Loading ...
            </div>
          }
        >
          {arr.map(a => (
            <p>{a}</p>
          ))}
        </InfiniteScroll>
      </div>
    </div>
  );
}
```

我们有一个 300 像素高的 div。

此外，我们将`overflow`设置为`'auto'`，这样当条目溢出 div 时，div 就会有一个滚动条。

然后我们用`InfiniteScroll`组件来显示商品。

`useWindow`被设置为`false`,这样我们可以观察我们是否滚动到了 div 的底部，而不是窗口的底部。

代码的其余部分是相同的。

![](img/b5f9cbf8b181572175db73054eff2dce.png)

[费里特·契克](https://unsplash.com/@ferrit?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

react-number-format 让我们格式化数字，并将它们显示为文本或输入框。

React Infinite Scroller 是一个易于使用的无限滚动软件包。