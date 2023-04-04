# 使用 React 套件库开始 React 开发—文本区域和数字输入

> 原文：<https://blog.devgenius.io/getting-started-with-react-development-with-the-react-suite-library-text-areas-and-numeric-inputs-7c276048e583?source=collection_archive---------7----------------------->

![](img/39237157249f66b543186d517d872bb9.png)

查尔斯·波斯蒂奥在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

React Suite 是一个有用的 UI 库，让我们可以轻松地将许多组件添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加组件。

# 文本区域

我们可以用`Input`组件添加一个文本区域。

例如，我们可以写:

```
import React from "react";
import { Input } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Input componentClass="textarea" rows={3} placeholder="Textarea" />
    </div>
  );
}
```

我们将`componentClass`设置为`'textarea'`，这样它将被呈现为一个文本区域。

`rows`有文本区的行数。

我们可以用`disabled`道具禁用输入:

```
import React from "react";
import { Input } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Input disabled />
    </div>
  );
}
```

我们还可以将`disabled`添加到`InputGroup`中:

```
import React from "react";
import { Input, InputGroup, Icon } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <InputGroup disabled>
        <Input />
        <InputGroup.Addon>
          <Icon icon="search" />
        </InputGroup.Addon>
      </InputGroup>
    </div>
  );
}
```

此外，我们可以添加一个带有工具提示的按钮，带有`speaker`属性:

```
import React from "react";
import { Input, Whisper, Tooltip } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Whisper trigger="focus" speaker={<Tooltip>Required</Tooltip>}>
        <Input style={{ width: 300 }} placeholder="Default Input" />
      </Whisper>
    </div>
  );
}
```

# 数字输入

组件让我们可以在 React 应用程序中添加数字输入。

例如，我们可以写:

```
import React from "react";
import { InputNumber } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <InputNumber />
    </div>
  );
}
```

来补充一下。

我们可以用`size`道具改变它的大小。

`xs`是特大号。`sm`小。`md`为中型，`lg`为大型。

此外，我们可以使用`defaultValue`和`snap`道具更改数值输入的默认值和捕捉步骤:

```
import React from "react";
import { InputNumber } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <InputNumber defaultValue={0.01} step={0.01} />
    </div>
  );
}
```

我们可以限制使用`min`和`max`道具选择的数字:

```
import React from "react";
import { InputNumber } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <InputNumber defaultValue={10} max={100} min={10} />
    </div>
  );
}
```

`disabled`道具禁用输入。

`prefix`道具让我们在输入的左边添加内容:

```
import React from "react";
import { InputNumber } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <InputNumber prefix="$" />
    </div>
  );
}
```

`postfix` prop 做同样的事情，但是将内容添加到输入的右边。

此外，我们可以使用`value`和`onChange`道具使其成为受控输入:

```
import React, { useState } from "react";
import { InputNumber } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  const [value, setValue] = useState(); const handleChange = (val) => {
    setValue(val);
  }; return (
    <div className="App">
      <InputNumber value={value} onChange={handleChange} />
    </div>
  );
}
```

`val`参数有输入值。

`value`设置输入的值。

# 结论

我们可以使用 React Suite 添加文本区域和数字输入。