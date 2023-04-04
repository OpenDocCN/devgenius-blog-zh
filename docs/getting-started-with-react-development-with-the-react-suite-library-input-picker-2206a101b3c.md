# 使用 React 套件库—输入选择器开始 React 开发

> 原文：<https://blog.devgenius.io/getting-started-with-react-development-with-the-react-suite-library-input-picker-2206a101b3c?source=collection_archive---------5----------------------->

![](img/90278420ec3355ae48da177c0ccb2d34.png)

[MAYANK D](https://unsplash.com/@mayank_dimri?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Suite 是一个有用的 UI 库，让我们可以轻松地将许多组件添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加组件。

# 输入选择器

我们可以添加`InputPicker`组件让用户选择一个选项。

例如，我们可以写:

```
import React from "react";
import { InputPicker } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";const data = [
  {
    label: "Apple",
    value: "apple",
    role: "fruit"
  },
  {
    label: "Orange",
    value: "orange",
    role: "fruit"
  }
];export default function App() {
  return (
    <div className="App">
      <InputPicker data={data} style={{ width: 225 }} />
    </div>
  );
}
```

向用户显示`label`属性。

`value`是选择时设置的值。

我们可以用`size`道具改变尺寸:

```
import React from "react";
import { InputPicker } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";const data = [
  {
    label: "Apple",
    value: "apple",
    role: "fruit"
  },
  {
    label: "Orange",
    value: "orange",
    role: "fruit"
  }
];export default function App() {
  return (
    <div className="App">
      <InputPicker data={data} size="lg" />
    </div>
  );
}
```

`lg`使其变大。

我们也可以将其设置为`md`表示中号，`sm`表示小号，或者`xs`表示超小号。

同样，我们可以用`block`属性使它成为块级元素:

```
import React from "react";
import { InputPicker } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";const data = [
  {
    label: "Apple",
    value: "apple",
    role: "fruit"
  },
  {
    label: "Orange",
    value: "orange",
    role: "fruit"
  }
];export default function App() {
  return (
    <div className="App">
      <InputPicker data={data} block />
    </div>
  );
}
```

我们可以使用`groupBy`属性添加选项组:

```
import React from "react";
import { InputPicker } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";const data = [
  {
    label: "Apple",
    value: "apple",
    role: "Fruit"
  },
  {
    label: "Orange",
    value: "orange",
    role: "Fruit"
  },
  {
    label: "Lettuce",
    value: "lettuce",
    role: "Vegetable"
  }
];export default function App() {
  return (
    <div className="App">
      <InputPicker data={data} groupBy="role" />
    </div>
  );
}
```

我们将它设置为我们想要分组的属性名。

`creatable`道具让我们在下拉菜单中输入自己的选择:

```
import React from "react";
import { InputPicker } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";const data = [
  {
    label: "Apple",
    value: "apple",
    role: "Fruit"
  },
  {
    label: "Orange",
    value: "orange",
    role: "Fruit"
  },
  {
    label: "Lettuce",
    value: "lettuce",
    role: "Vegetable"
  }
];export default function App() {
  return (
    <div className="App">
      <InputPicker data={data} creatable />
    </div>
  );
}
```

我们可以使用`renderMenuItem`道具来呈现自定义选项:

```
import React from "react";
import { InputPicker } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";const data = [
  {
    label: "Apple",
    value: "apple",
    role: "Fruit"
  },
  {
    label: "Orange",
    value: "orange",
    role: "Fruit"
  },
  {
    label: "Lettuce",
    value: "lettuce",
    role: "Vegetable"
  }
];export default function App() {
  return (
    <div className="App">
      <InputPicker
        data={data}
        renderMenuItem={(label, item) => {
          return (
            <div>
              <i className="rs-icon rs-icon-user" /> {label}
            </div>
          );
        }}
        renderMenuGroup={(label, item) => {
          return (
            <div>
              <i className="rs-icon rs-icon-group" /> {label} - (
              {item.children.length})
            </div>
          );
        }}
        renderValue={(value, item, selectedElement) => {
          return (
            <div>
              <span style={{ color: "#575757" }}>
                <i className="rs-icon rs-icon-user" /> Choice:
              </span>
              {value}
            </div>
          );
        }}
      />
    </div>
  );
}
```

`renderMenuGroup`按照我们的方式呈现选项组。

`renderValue`渲染所选值。

我们只是返回指定如何呈现项目的 JSX。

# 结论

我们可以添加可使用 React Suite 定制的下拉菜单。