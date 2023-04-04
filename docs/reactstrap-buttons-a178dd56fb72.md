# 反应陷阱—按钮

> 原文：<https://blog.devgenius.io/reactstrap-buttons-a178dd56fb72?source=collection_archive---------5----------------------->

![](img/cbae10e6df7fa40a56b8b45e078c5ada.png)

由[邦妮·凯特](https://unsplash.com/@bonniekdesign?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Reactstrap 是为 React 制作的一个版本引导程序。

这是一组具有 Boostrap 陷阱样式的 React 组件。

在本文中，我们将看看如何添加带有 Reactstrap 的按钮。

# 小跟班

Reactstrap 自带按钮组件。

为了添加它，我们使用了`Button`组件:

```
import React from "react";
import { Button } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Button color="primary" className="mr-2">
        primary
      </Button>
      <Button color="secondary" className="mr-2">
        secondary
      </Button>
      <Button color="success" className="mr-2">
        success
      </Button>
      <Button color="info" className="mr-2">
        info
      </Button>
      <Button color="warning" className="mr-2">
        warning
      </Button>
      <Button color="danger" className="mr-2">
        danger
      </Button>
      <Button color="link" className="mr-2">
        link
      </Button>
    </>
  );
}
```

我们添加了带有`color`道具的`Button`组件来改变按钮的颜色。

`mr-2`类增加了一些右边距。

# 大纲按钮

按钮也可以有轮廓样式。

为了添加它们，我们添加了`outline`道具:

```
import React from "react";
import { Button } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Button outline color="primary" className="mr-2">
        primary
      </Button>
      <Button outline color="secondary" className="mr-2">
        secondary
      </Button>
      <Button outline color="success" className="mr-2">
        success
      </Button>
      <Button outline color="info" className="mr-2">
        info
      </Button>
      <Button outline color="warning" className="mr-2">
        warning
      </Button>
      <Button outline color="danger" className="mr-2">
        danger
      </Button>
      <Button outline color="link" className="mr-2">
        link
      </Button>
    </>
  );
}
```

# 大小

我们可以添加`size`道具来改变大小。

`lg`使其变大，`sm`使其变小。

我们可以这样补充:

```
import React from "react";
import { Button } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Button color="primary" size="lg">
        Large Button
      </Button>
      <Button color="primary" size="sm">
        Small Button
      </Button>
    </>
  );
}
```

我们用`size`道具加了一个大大小小的按钮。

要使按钮成为块元素，我们可以使用`block`道具:

```
import React from "react";
import { Button } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Button color="primary" size="lg" block>
        Large Button
      </Button>
    </>
  );
}
```

这将跨越整行。

# 活动状态

要让一个按钮显示活动状态，我们可以添加`active`道具。

例如，我们可以写:

```
import React from "react";
import { Button } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Button color="primary" size="lg" active>
        Large Button
      </Button>
    </>
  );
}
```

活动状态将比非活动状态更暗。

# 禁用状态

我们可以用`disabled`道具禁用一个按钮。

例如，我们可以写:

```
import React from "react";
import { Button } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Button color="primary" size="lg" disabled>
        Large Button
      </Button>
    </>
  );
}
```

# 复选框和单选按钮

我们可以用`Button`组件制作单选按钮。

为此，我们将它们放在一个按钮组中。

例如，我们可以写:

```
import React from "react";
import { Button, ButtonGroup } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  const [rSelected, setRSelected] = React.useState(null); return (
    <>
      <ButtonGroup>
        <Button
          color="primary"
          onClick={() => setRSelected(1)}
          active={rSelected === 1}
        >
          apple
        </Button>
        <Button
          color="primary"
          onClick={() => setRSelected(2)}
          active={rSelected === 2}
        >
          orange
        </Button>
        <Button
          color="primary"
          onClick={() => setRSelected(3)}
          active={rSelected === 3}
        >
          grape
        </Button>
      </ButtonGroup>
    </>
  );
}
```

我们有一个`ButtonGroup`来将所有的按钮组合在一起。

然后我们为每个按钮准备了`onClick`道具来设置`rSelected`状态。

`active`道具让我们设置激活它的条件。

我们也可以使用按钮作为复选框按钮。

例如，我们可以写:

```
import React from "react";
import { Button, ButtonGroup } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  const [cSelected, setCSelected] = React.useState([]); const onCheckboxBtnClick = selected => {
    const index = cSelected.indexOf(selected);
    if (index < 0) {
      cSelected.push(selected);
    } else {
      cSelected.splice(index, 1);
    }
    setCSelected([...cSelected]);
  }; return (
    <>
      <ButtonGroup>
        <Button
          color="primary"
          onClick={() => onCheckboxBtnClick(1)}
          active={cSelected.includes(1)}
        >
          apple
        </Button>
        <Button
          color="primary"
          onClick={() => onCheckboxBtnClick(2)}
          active={cSelected.includes(2)}
        >
          orange
        </Button>
        <Button
          color="primary"
          onClick={() => onCheckboxBtnClick(3)}
          active={cSelected.includes(3)}
        >
          grape
        </Button>
      </ButtonGroup>
    </>
  );
}
```

我们有一个`onCheckboxBtnClick`函数，如果选择已经存在，它就会运行来删除选择。

否则，将其添加到`cSelected`数组中。

`active`属性的条件现在检查选择是否在`cSelected`数组中，而不是检查是否相等。

这是因为复选框可以保存多个选择。

![](img/53749a594005aebd6c3387da43ae8389.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Aswathy N](https://unsplash.com/@abnair?utm_source=medium&utm_medium=referral) 拍摄

# 结论

按钮有许多用途。它们可以单独使用，也可以作为复选框或单选按钮使用。