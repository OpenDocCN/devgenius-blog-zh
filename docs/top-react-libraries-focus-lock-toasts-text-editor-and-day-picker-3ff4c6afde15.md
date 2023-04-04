# 顶级 React 库—焦点锁定、祝酒词、文本编辑器和日期选择器

> 原文：<https://blog.devgenius.io/top-react-libraries-focus-lock-toasts-text-editor-and-day-picker-3ff4c6afde15?source=collection_archive---------4----------------------->

![](img/827d713cf8b641bd0eeb0e59ee820758.png)

由 [iMattSmart](https://unsplash.com/@imattsmart?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# 反应-聚焦-锁定

react-focus-lock 包让我们保持一个元素的焦点。

要安装它，我们可以运行:

```
npm i react-focus-lock
```

然后我们可以通过写来使用它:

```
import React from "react";
import FocusLock from "react-focus-lock";export default function App() {
  const onClose = e => console.log(e); return (
    <FocusLock>
      <button onClick={onClose}>button</button>
    </FocusLock>
  );
}
```

我们有`FocusLock`组件来保持焦点在按钮上。

hos 聚焦的选项和`FocusLock`组件可以渲染成什么组件是可以改变的。

# 反应迟钝

React-Toastify 组件让我们可以轻松地向 React 应用程序添加祝酒词。

为了安装这个包，我们运行:

```
npm i react-toastify
```

然后我们可以通过写来使用它:

```
import React from "react";
import { ToastContainer, toast } from "react-toastify";
import "react-toastify/dist/ReactToastify.css";export default function App() {
  const notify = () => toast("toast!"); return (
    <div>
      <button onClick={notify}>Notify!</button>
      <ToastContainer />
    </div>
  );
}
```

我们导入 CSS 和 JavaScript 模块。

然后我们可以调用`toast`函数来显示祝酒词。

位置、内容、更新、持续时间等。，都可以改。

# 反应-Ace

React-Ace 是一个文本编辑器组件，我们可以将其添加到 React 应用程序中。

我们可以通过运行以下命令来安装它:

```
npm i react-ace
```

然后我们可以通过写来使用它:

```
import React from "react";
import AceEditor from "react-ace";
import "ace-builds/src-noconflict/mode-java";
import "ace-builds/src-noconflict/theme-github";export default function App() {
  function onChange(newValue) {
    console.log("change", newValue);
  }
  return (
    <div>
      <AceEditor
        mode="java"
        theme="github"
        onChange={onChange}
        name="id"
        editorProps={{ $blockScrolling: true }}
      />
    </div>
  );
}
```

我们导入 CSS，然后我们可以使用`AceEditor`组件来添加编辑器。

`editorProps`拥有编辑的选择权。

我们可以为聚焦、模糊、滚动、验证等等添加监听器。

显示的行数也可以改变。

`mode`是用于解析和代码突出显示的语言。

`theme`有用于样式编辑器的主题。

它还附带了`SplitEditor`组件，可以创建 Ace 编辑器的多个链接实例。

每个实例共享一个主题和其他属性，但是它们有自己的值。

例如，我们可以写:

```
import React from "react";
import { split as SplitEditor } from "react-ace";
import "ace-builds/src-noconflict/mode-java";
import "ace-builds/src-noconflict/theme-github";export default function App() {
  return (
    <div>
      <SplitEditor
        mode="java"
        theme="github"
        splits={2}
        orientation="below"
        value={["hi", "hello"]}
        name="id"
        editorProps={{ $blockScrolling: true }}
      />
    </div>
  );
}
```

添加拆分编辑器。

# react-day-picker

react-day-picker 是一个灵活的日期选择器，我们可以在 react 应用程序中使用。

它高度可定制、可本地化，并且不需要任何外部依赖。

要安装它，我们运行:

```
npm i react-day-picker
```

然后我们可以添加带有`DayPicker`组件的日期选择器:

```
import React from "react";
import DayPicker from "react-day-picker";
import "react-day-picker/lib/style.css";export default function App() {
  return (
    <div>
      <DayPicker />
    </div>
  );
}
```

我们可以通过传入`onDayClick`和`selectedDays`道具使其成为受控组件:

```
import React from "react";
import DayPicker from "react-day-picker";
import "react-day-picker/lib/style.css";export default function App() {
  const [selectedDay, setSelectedDay] = React.useState(); const handleDayClick = day => {
    setSelectedDay(day);
  }; return (
    <div>
      <DayPicker onDayClick={handleDayClick} selectedDays={selectedDay} />
    </div>
  );
}
```

`onDayClick`将选择的日期设置为`selectedDay`状态。

`selectedDays`具有从`selectedDay`状态选择的日期的值。

我们可以设计它的样式，而且它支持本地化。

![](img/d17418eebc76bff55b8606df7bac4953.png)

卡尔·詹尼斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

react-focus-lock 让我们将焦点放在应用程序的元素上。

React-Toastify 让我们可以轻松地展示祝酒词。

React-Ace 是一个文本编辑器。

react-day-picker 是一个可样式化并支持本地化的日期选择器。