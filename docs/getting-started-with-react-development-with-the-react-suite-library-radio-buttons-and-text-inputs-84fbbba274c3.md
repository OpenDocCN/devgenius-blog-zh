# 使用 React 套件库开始 React 开发—单选按钮和文本输入

> 原文：<https://blog.devgenius.io/getting-started-with-react-development-with-the-react-suite-library-radio-buttons-and-text-inputs-84fbbba274c3?source=collection_archive---------4----------------------->

![](img/9304eb8418631bdd9a54c8a3ac95f37a.png)

乔纳森·贝拉斯克斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Suite 是一个有用的 UI 库，让我们可以轻松地将许多组件添加到 React 应用程序中。

在本文中，我们将了解如何使用它向 React 应用程序添加组件。

# 单选按钮

我们可以用 React Suite 的`Radio`和`RadioGroup`组件添加单选按钮组。

例如，我们可以写:

```
import React from "react";
import { Radio } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Radio> Radio</Radio>
      <Radio checked> Checked Radio</Radio>
    </div>
  );
}
```

`checked`道具让我们选择单选按钮。

`disabled`按钮禁用单选按钮。

我们可以用`RadioGroup`组件添加单选按钮组。

例如，我们可以写:

```
import React from "react";
import { Radio, RadioGroup } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <RadioGroup name="radioList">
        <p>Group 1</p>
        <Radio value="A">Item A</Radio>
        <Radio value="B">Item B</Radio>
        <p>Group 2</p>
        <Radio value="C">Item C</Radio>
        <Radio value="D" disabled>
          Item D
        </Radio>
      </RadioGroup>
    </div>
  );
}
```

将 4 个单选按钮添加到一个组中。

`inline`道具使单选按钮内联:

```
import React from "react";
import { Radio, RadioGroup } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <RadioGroup name="radioList" inline>
        <p>Group 1</p>
        <Radio value="A">Item A</Radio>
        <Radio value="B">Item B</Radio>
        <p>Group 2</p>
        <Radio value="C">Item C</Radio>
        <Radio value="D" disabled>
          Item D
        </Radio>
      </RadioGroup>
    </div>
  );
}
```

我们可以用`defaultValue`属性设置默认选择值:

```
import React from "react";
import { Radio, RadioGroup } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <RadioGroup name="radioList" inline appearance="picker" defaultValue="A">
        <p>Group 1</p>
        <Radio value="A">Item A</Radio>
        <Radio value="B">Item B</Radio>
        <p>Group 2</p>
        <Radio value="C">Item C</Radio>
        <Radio value="D" disabled>
          Item D
        </Radio>
      </RadioGroup>
    </div>
  );
}
```

`appearance`改变单选按钮的外观。

`'picker'`将它们变成按钮。

# 文本输入

我们可以使用`Input`和`InputGroup`组件在我们的应用程序中添加文本输入。

例如，我们可以写:

```
import React from "react";
import { Input } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <Input placeholder="Input" />
    </div>
  );
}
```

我们设置`placeholder`道具来显示占位符。

要添加一个末尾有图标的输入，我们写:

```
import React from "react";
import { Input, InputGroup, Icon } from "rsuite";
import "rsuite/dist/styles/rsuite-default.css";export default function App() {
  return (
    <div className="App">
      <InputGroup inside>
        <Input placeholder="Input" />
        <InputGroup.Button>
          <Icon icon="search" />
        </InputGroup.Button>
      </InputGroup>
    </div>
  );
}
```

我们添加`InputGroup`组件来添加输入组。

`inside`道具让我们将内容添加到我们的`Input`中。

`InputGroup.Button`让我们在输入的末尾添加一个按钮。

我们在输入中有一个`Icon`，它将被呈现在右侧。

我们可以通过`InputGroup`的`size`支柱来改变输入大小。

`xs`是特大号，`sm`是小号，`md`是中号，`lg`是大号。

# 结论

我们可以用 React Suite 提供的组件添加一个输入字段和单选按钮。