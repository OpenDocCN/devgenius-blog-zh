# 反应引导—手风琴

> 原文：<https://blog.devgenius.io/react-bootstrap-accordion-be1de3eefd25?source=collection_archive---------2----------------------->

![](img/78288bdc4f6443968bfbde611ad31d7e.png)

[约翰·西门子](https://unsplash.com/@johannsiemens?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何在 React 组件中使用 React Bootstrap 的手风琴。

# 手风琴

手风琴让我们限制卡片组件一次只能打开一个。

要使用它，我们可以写:

```
import React from "react";
import Accordion from "react-bootstrap/Accordion";
import Card from "react-bootstrap/Card";
import Button from "react-bootstrap/Button";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Accordion defaultActiveKey="0">
        <Card>
          <Card.Header>
            <Accordion.Toggle as={Button} variant="link" eventKey="0">
              title 1
            </Accordion.Toggle>
          </Card.Header>
          <Accordion.Collapse eventKey="0">
            <Card.Body>body 1</Card.Body>
          </Accordion.Collapse>
        </Card>
        <Card>
          <Card.Header>
            <Accordion.Toggle as={Button} variant="link" eventKey="1">
              title 2
            </Accordion.Toggle>
          </Card.Header>
          <Accordion.Collapse eventKey="1">
            <Card.Body>body 2</Card.Body>
          </Accordion.Collapse>
        </Card>
      </Accordion>
    </>
  );
}
```

我们导入了`Accordion`和`Card`组件。

上面有`defaultActionKey`为默认卡的钥匙。

`Accordion.Toggle`是打开卡的拨动开关。

`as`是一个道具，让我们指定将切换渲染为哪个组件。

`variant`是我们想要呈现的样式。

`eventKey`是给定卡的密钥。

# 完全折叠状态

如果我们不指定`defaultActiveKey`，那么所有的卡片都将被折叠。

例如，我们可以写:

```
import React from "react";
import Accordion from "react-bootstrap/Accordion";
import Card from "react-bootstrap/Card";
import Button from "react-bootstrap/Button";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Accordion>
        <Card>
          <Card.Header>
            <Accordion.Toggle as={Button} variant="link" eventKey="0">
              title 1
            </Accordion.Toggle>
          </Card.Header>
          <Accordion.Collapse eventKey="0">
            <Card.Body>body 1</Card.Body>
          </Accordion.Collapse>
        </Card>
        <Card>
          <Card.Header>
            <Accordion.Toggle as={Button} variant="link" eventKey="1">
              title 2
            </Accordion.Toggle>
          </Card.Header>
          <Accordion.Collapse eventKey="1">
            <Card.Body>body 2</Card.Body>
          </Accordion.Collapse>
        </Card>
      </Accordion>
    </>
  );
}
```

# 使整个标题可点击

我们可以通过将标题呈现为一个`Card.Header`来使整个标题可点击。

例如，我们可以写:

```
import React from "react";
import Accordion from "react-bootstrap/Accordion";
import Card from "react-bootstrap/Card";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Accordion>
        <Card>
          <Card.Header>
            <Accordion.Toggle as={Card.Header} variant="link" eventKey="0">
              title 1
            </Accordion.Toggle>
          </Card.Header>
          <Accordion.Collapse eventKey="0">
            <Card.Body>body 1</Card.Body>
          </Accordion.Collapse>
        </Card>
        <Card>
          <Card.Header>
            <Accordion.Toggle as={Card.Header} variant="link" eventKey="1">
              title 2
            </Accordion.Toggle>
          </Card.Header>
          <Accordion.Collapse eventKey="1">
            <Card.Body>body 2</Card.Body>
          </Accordion.Collapse>
        </Card>
      </Accordion>
    </>
  );
}
```

我们通过将`as`改为`Card.Header`使文本之外的部分可点击。

# 自定义开关

我们可以用`useAccordionToggle`钩子创建我们的自定义切换组件。

它采用了`eventKey`和一个在 roggle 被点击时运行的回调函数。

例如，我们可以写:

```
import React from "react";
import Accordion from "react-bootstrap/Accordion";
import Card from "react-bootstrap/Card";
import { useAccordionToggle } from "react-bootstrap/AccordionToggle";
import "bootstrap/dist/css/bootstrap.min.css";function CustomToggle({ children, eventKey }) {
  const decoratedOnClick = useAccordionToggle(eventKey, () =>
    console.log("toggle clicked")
  ); return (
    <button
      type="button"
      style={{ backgroundColor: "orange" }}
      onClick={decoratedOnClick}
    >
      {children}
    </button>
  );
}export default function App() {
  return (
    <>
      <Accordion>
        <Card>
          <Card.Header>
            <CustomToggle eventKey="0">title 1</CustomToggle>
          </Card.Header>
          <Accordion.Collapse eventKey="0">
            <Card.Body>body 1</Card.Body>
          </Accordion.Collapse>
        </Card>
        <Card>
          <Card.Header>
            <CustomToggle eventKey="0">title 2</CustomToggle>
          </Card.Header>
          <Accordion.Collapse eventKey="1">
            <Card.Body>body 2</Card.Body>
          </Accordion.Collapse>
        </Card>
      </Accordion>
    </>
  );
}
```

我们有自己创建的`CustomToggle`组件。

它需要`children`和`eventKey`道具。

`children`让我们显示在组件标签中传递的内容。

`useAccordingToggle`返回一个点击处理程序，用给定的`eventKey`打开卡片。

第二个参数是一个回调函数，它允许我们在点击开关时运行任何东西。

然后我们可以使用`CustomToggle`来代替 React Bootstrap 附带的 toggle 来切换我们的卡。

# 具有扩展意识的自定义开关

我们还可以检查当它被点击时，开关是否展开了卡片。

为此，我们使用上下文 API 来监听手风琴到`AccordionContext`的声音。

例如，我们可以写:

```
import React from "react";
import Accordion from "react-bootstrap/Accordion";
import AccordionContext from "react-bootstrap/AccordionContext";
import Card from "react-bootstrap/Card";
import { useAccordionToggle } from "react-bootstrap/AccordionToggle";
import "bootstrap/dist/css/bootstrap.min.css";function CustomToggle({ children, eventKey, callback }) {
  const currentEventKey = React.useContext(AccordionContext);const decoratedOnClick = useAccordionToggle(
    eventKey,
    () => callback && callback(eventKey)
  );const isCurrentEventKey = currentEventKey === eventKey;return (
    <button
      type="button"
      style={{ backgroundColor: isCurrentEventKey ? "orange" : "pink" }}
      onClick={decoratedOnClick}
    >
      {children}
    </button>
  );
}export default function App() {
  return (
    <>
      <Accordion>
        <Card>
          <Card.Header>
            <CustomToggle eventKey="0">title 1</CustomToggle>
          </Card.Header>
          <Accordion.Collapse eventKey="0">
            <Card.Body>body 1</Card.Body>
          </Accordion.Collapse>
        </Card>
        <Card>
          <Card.Header>
            <CustomToggle eventKey="1">title 2</CustomToggle>
          </Card.Header>
          <Accordion.Collapse eventKey="1">
            <Card.Body>body 2</Card.Body>
          </Accordion.Collapse>
        </Card>
      </Accordion>
    </>
  );
}
```

我们用我们的`CustomToggle`得到`eventKey`道具，这样我们就可以和我们上下文中的`currentEventKey`进行比较。

然后我们有了`useAccordionToggle`,就像之前的例子一样。

它返回一个点击处理程序，我们将它传递给`onClick`来扩展卡片。

`isCurrentKey`的结果是将 toggle 的`eventKey`与上下文中打开的 toggle 进行比较。

因此，当卡片打开或关闭时，我们可以用它来设计我们喜欢的切换方式。

![](img/561069a382b39911d09c5b66c6c0dec8.png)

由[马赫凯奥](https://unsplash.com/@mahkeo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

手风琴让我们限制卡片组件一次只能打开一个。

我们可以自定义切换和卡片内容。