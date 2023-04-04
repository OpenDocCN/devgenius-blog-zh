# 顶部反应挂钩—cookie、去抖和剪贴板

> 原文：<https://blog.devgenius.io/top-react-hooks-cookies-debounce-and-clipboard-ba9dfb2a75d4?source=collection_archive---------2----------------------->

![](img/36628930d79de50b2e64b695da7ee1c8.png)

米莎·沃克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Hooks 包含了 React 应用程序中的逻辑代码。

我们可以创建自己的钩子，使用别人提供的钩子。

在本文中，我们将看看一些有用的 React 挂钩。

# 反应配方

React Recipes 附带了许多钩子，我们可以用来做各种事情。

我们可以使用`useCopyClipboard`钩子将任何字符串复制到剪贴板。

例如，我们可以写:

```
import React from "react";
import { useCopyClipboard } from "react-recipes";export default function App() {
  const [isCopied, setIsCopied] = useCopyClipboard(); const copy = () => {
    setIsCopied("copy string");
  }; return (
    <button onClick={copy} type="button">
      {isCopied ? "Copied" : "Copy"}
    </button>
  );
}
```

创建将字符串复制到剪贴板的按钮。

`useCopyClipboard`钩子返回一个包含`isCopied`和`setIsCopied`变量的数组。

`isCopied`表示如果是`true`，字符串被复制到剪贴板。

`setIsCopied`将字符串复制到剪贴板。

`useDarkMode`挂钩让我们可以打开和关闭黑暗模式。

该设置将保存在本地存储中。

例如，我们可以写:

```
import React from "react";
import { useDarkMode } from "react-recipes";export default function App() {
  const [darkMode, setDarkMode] = useDarkMode(); return (
    <div>
      <button onClick={() => setDarkMode(!darkMode)}>toggle dark mode</button>
      <p>{darkMode.toString()}</p>
    </div>
  );
}
```

`setDarkMode`功能设置黑暗模式是否开启。

`darkMode`有暗模式的切换状态。

该设置将保存为本地存储器中带有关键字`dark-mode-enabled`的条目。

它没有任何样式，所以我们必须自己设置黑暗模式的样式。

`useDebounce`是一个钩子，让我们谴责任何快速变化的价值。

例如，我们可以这样使用它:

```
import React from "react";
import { useDebounce } from "react-recipes";export default function App() {
  const [searchTerm, setSearchTerm] = React.useState("");
  const [result, setResult] = React.useState({}); const debouncedSearchTerm = useDebounce(searchTerm, 500); const search = async () => {
    const res = await fetch(`https://api.agify.io/?name=${debouncedSearchTerm}`);
    const data = await res.json();
    setResult(data);
  }; React.useEffect(() => {
    if (debouncedSearchTerm) {
      search();
    }
  }, [debouncedSearchTerm]); return (
    <div>
      <input value={searchTerm} onChange={e => setSearchTerm(e.target.value)} />
      <p>{JSON.stringify(result)}</p>
    </div>
  );
}
```

我们在`searchTerm`状态下使用`useDebounce`挂钩。

第二个参数是设置`debouncedSearchTerm`值的延迟，单位为毫秒。

然后我们将它传递到第二个参数`useEffect`的数组中，观察它的值。

如果`debouncedSearchTerm` 置位，它将运行`search`函数获取数据。

`useDimensions`钩子让我们获得任何元素的尺寸。

要使用它，我们可以写:

```
import React from "react";
import { useDimensions } from "react-recipes";export default function App() {
  const [wrapperRef, dimensions] = useDimensions(); return (
    <div ref={wrapperRef}>
      height: {dimensions.height}
      width: {dimensions.width}
    </div>
  );
}
```

钩子返回我们传递的`wrapperRef`,作为我们想要查看其大小的元素的`ref`属性的值。

`dimensions`具有我们传递给它的 div 的维度。

然后，当我们改变视窗的大小时，我们会看到尺寸的更新。

![](img/640a9179add937fe128c1a4d7175ee8c.png)

诺亚·西利曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

React Recipes 有用于反弹、复制到剪贴板、观察元素大小和操作 cookies 的钩子。