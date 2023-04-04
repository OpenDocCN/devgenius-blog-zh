# 使用 React 套件库开始 React 开发—选择下拉菜单

> 原文：<https://blog.devgenius.io/developing-vue-apps-with-the-quasar-library-select-dropdown-aa3d391a2128?source=collection_archive---------7----------------------->

![](img/ecbe0f920963a120bc95b4f8fda5974a.png)

[kazuend](https://unsplash.com/@kazuend?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

React Suite 是一个有用的 UI 库，让我们可以轻松地将许多组件添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加组件。

# 选择下拉菜单

我们可以用`SelectPicker`组件在 React 应用程序中添加一个选择下拉菜单。

例如，我们可以写:

```
import React from "react";
import { SelectPicker } from "rsuite";
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
      <SelectPicker data={data} />
    </div>
  );
}
```

添加一个简单的下拉列表。

`label`显示给用户。

当我们选择一个项目时，`value`被用作选择值。

`appearance`道具改变下拉菜单的外观:

```
import React from "react";
import { SelectPicker } from "rsuite";
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
      <SelectPicker data={data} appearance="subtle" />
    </div>
  );
}
```

将`appearance`设置为`'subtle'`会使其变成灰色。

`size`道具改变下拉菜单的大小:

```
import React from "react";
import { SelectPicker } from "rsuite";
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
      <SelectPicker data={data} size="lg" />
    </div>
  );
}
```

`lg`使它变大。使它成为中等大小。`sm`使其变小，`xs`使其变得特别小。

`block`属性使其显示为块级元素:

```
import React from "react";
import { SelectPicker } from "rsuite";
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
      <SelectPicker data={data} block />
    </div>
  );
}
```

`groupBy`属性允许我们从`data`数组中的对象设置属性名，以便按以下方式对选项进行分组:

```
import React from "react";
import { SelectPicker } from "rsuite";
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
      <SelectPicker data={data} groupBy="role" />
    </div>
  );
}
```

我们可以用`placement`道具改变位置:

```
import React from "react";
import { SelectPicker } from "rsuite";
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
      <SelectPicker data={data} placement="rightStart" />
    </div>
  );
}
```

`placement`可能值的完整列表在[https://rsuitejs.com/components/select-picker/](https://rsuitejs.com/components/select-picker/)上。

# 结论

我们可以使用 React Suite 在 React 应用程序中添加一个选择下拉菜单。