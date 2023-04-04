# 使用 React 套件库开始 React 开发—自动完成和切换

> 原文：<https://blog.devgenius.io/getting-started-with-react-development-with-the-react-suite-library-autocomplete-and-toggle-169701dbb8c7?source=collection_archive---------4----------------------->

![](img/83d8504d12eb62f11ceaf29414e32f27.png)

亚当·温格在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

React Suite 是一个有用的 UI 库，让我们可以轻松地将许多组件添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加组件。

# 自动完成

我们可以使用`AutoComplete`组件将自动完成输入添加到 React 应用程序中。

例如，我们可以写:

```
import React from "react";
import { AutoComplete } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";const data = ["apple", "orange", "grape"];export default function App() {
  return (
    <div className="App">
      <AutoComplete data={data} />
    </div>
  );
}
```

我们将数据传递给`data` prop 来显示自动完成选项。

我们可以用`renderItem`道具渲染一个自定义物品:

```
import React from "react";
import { AutoComplete, Icon } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";const data = ["apple", "orange", "grape"];export default function App() {
  return (
    <div className="App">
      <AutoComplete
        data={data}
        renderItem={(item) => {
          return (
            <div>
              <Icon icon="star" /> {item.label}
            </div>
          );
        }}
      />
    </div>
  );
}
```

属性禁用自动完成功能。

此外，我们可以将自动完成功能添加到输入组中:

```
import React from "react";
import { AutoComplete, Icon, InputGroup } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";const data = ["apple", "orange", "grape"];export default function App() {
  return (
    <div className="App">
      <InputGroup inside>
        <AutoComplete data={data} />
        <InputGroup.Button>
          <Icon icon="search" />
        </InputGroup.Button>
      </InputGroup>
    </div>
  );
}
```

我们可以通过设置`value`和`onChange`道具使其成为受控输入:

```
import React, { useState } from "react";
import { AutoComplete } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";const data = ["apple", "orange", "grape"];export default function App() {
  const [value, setValue] = useState(); const handleChange = (val) => {
    setValue(val);
  }; return (
    <div className="App">
      <AutoComplete data={data} value={value} onChange={handleChange} />
    </div>
  );
}
```

`value`有输入值，我们用 tyhe `handleChange`函数设置。

`val`有输入的值，所以我们可以调用`setValue`来设置`value`。

# 触发器

我们可以用`Toggle`组件添加一个开关。

例如，我们可以写:

```
import React from "react";
import { Toggle } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Toggle defaultChecked />
    </div>
  );
}
```

`defaultChecked`将使其默认选中。

`size`道具让我们设置开关的大小:

```
import React from "react";
import { Toggle } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Toggle size="lg" />
    </div>
  );
}
```

`lg`使其变大。`sm`使其变小。使它成为中等的。

我们可以将文本添加到切换框中，并根据它是否被选中来显示文本。

例如，我们可以写:

```
import React from "react";
import { Toggle } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Toggle size="lg" checkedChildren="Open" unCheckedChildren="Close" />
    </div>
  );
}
```

勾选时显示`checkedChildren`，不勾选时显示`unCheckedChildren`。

`disabled`按钮禁用切换。

# 结论

我们可以使用 React suite 添加自动完成功能并切换到 React 应用程序中。