# 使用交叉点观察器 API 为 React 应用程序添加无限滚动

> 原文：<https://blog.devgenius.io/add-infinite-scrolling-to-a-react-app-with-the-intersection-observer-api-d3a3381f9a1c?source=collection_archive---------1----------------------->

![](img/c5bbc3532ff894c2432083cc38ee9fdf.png)

照片由[自由使用声音](https://unsplash.com/@freetousesoundscom?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上

无限滚动是我们必须经常添加到 React 应用程序中的东西。

在本文中，我们将了解如何使用交叉点观察器 API 为 React 应用程序添加无限滚动。

# 使用交叉点观察器 API 添加无限滚动

交叉点观察器 API 允许我们在 React 应用程序中轻松添加无限滚动，因为我们可以使用它来检测列表底部的元素。

我们为页面底部的元素分配一个 ref，然后我们可以使用交叉点观察器 API 来检测它何时越过屏幕边缘并显示在页面上。

为此，我们写道:

```
import { useEffect, useRef, useState } from "react";
export default function App() {
  const lastItemRef = useRef();
  const observer = useRef();
  const [arr, setArr] = useState(
    Array(30)
      .fill()
      .map((_, i) => i)
  );
  const [page, setPage] = useState(1);useEffect(() => {
    const options = {
      root: document,
      rootMargin: "20px",
      threshold: 1
    };
    const callback = (entries) => {
      if (entries[0].isIntersecting) {
        const newPage = page + 1;
        setArr((arr) => [
          ...arr,
          ...Array(30)
            .fill()
            .map((_, i) => i + 30 * (newPage - 1))
        ]);
        setPage(newPage);
      }
    };
    observer.current = new IntersectionObserver(callback, options);
    if (lastItemRef.current) {
      observer.current.observe(lastItemRef.current);
    }
    return () => {
      observer.current.disconnect();
    };
  });
  return (
    <div className="App">
      {arr.map((a, i) => {
        if (i === arr.length - 1) {
          return (
            <p key={a} ref={lastItemRef}>
              {a}
            </p>
          );
        }
        return <p key={a}>{a}</p>;
      })}
    </div>
  );
}
```

我们有`lastItemRef`，它是我们分配给页面底部元素的 ref。

`observer`是我们用来存储观察者的 ref。

`arr`就是我们渲染到页面上的东西。

我们有`page`元素来告诉我们现在在哪一页。

然后我们有一个带有回调的`useEffect`钩子，回调有一个`options`对象来存储交叉点观察器的选项。

`root`是滚动容器，也就是`document`对象。这就是`html`元素。

`rootMargin`我们观察的元素离屏幕边缘有多近，才能被认为与屏幕边缘相交。

`threshold`表示对于要运行的回调函数 top，观察到的元素在根选项指定的元素中可见的程度。

然后我们用`callback`来检查`isIntersecting`是否为`true`以将其视为交集。

然后我们调用`setArr`向`arr`添加更多的项目，如果是`true`的话。

然后我们用我们正在观察的元素调用`observe`，它存储在`lastItemRef` current 中。

当组件卸载时，我们返回一个调用`disconnect`的回调来清除交叉点观察器。

最后，我们将`arr`项渲染到`p`元素中，并将`lastItemRef`赋给最后渲染的元素，即屏幕底部的元素。

# 结论

我们可以使用交叉点观察器 API 轻松地将无限滚动添加到 React 应用程序中。