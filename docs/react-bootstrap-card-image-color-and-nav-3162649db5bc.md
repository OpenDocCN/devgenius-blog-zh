# 反应引导—卡片图像、颜色和导航

> 原文：<https://blog.devgenius.io/react-bootstrap-card-image-color-and-nav-3162649db5bc?source=collection_archive---------5----------------------->

![](img/b32f43570715381cf96abf4f164d34df.png)

[安德鲁·詹金斯](https://unsplash.com/@anglue18?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何定制 React 引导卡。

# 图像叠加

我们可以添加卡片背景和覆盖层，

为此，我们可以使用`Card.ImgOverlay`组件。

例如，我们可以写:

```
import React from "react";
import Card from "react-bootstrap/Card";
import Button from "react-bootstrap/Button";
import "bootstrap/dist/css/bootstrap.min.css";
export default function App() {
  return (
    <>
      <Card style={{ width: "20rem" }}>
        <Card.Img
          variant="top"
          src="https://dummyimage.com/700/ffffff/000000"
        />
        <Card.ImgOverlay>
          <Card.Title>Special title</Card.Title>
          <Card.Text>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent
            ultrices ac dolor nec vestibulum. Maecenas vulputate diam ut sem
            tempus, id eleifend tortor hendrerit. Sed non orci massa. Aliquam
            eget lectus a ante convallis gravida. Donec fringilla odio ut magna
            gravida aliquam. Cras molestie non ante vel dictum. Cras lacinia
            molestie lacus, in lacinia sapien imperdiet in. Sed eleifend laoreet
            finibus. Integer semper dictum eros nec eleifend. Nunc quam mi,
            finibus lacinia metus vitae, dapibus ultricies diam. Vivamus ante
            nisi, pellentesque at lacus eu, vehicula pretium lorem. Nunc vitae
            ligula laoreet, accumsan diam et, auctor mauris. Fusce vitae posuere
            nibh, vitae eleifend quam. Duis a enim lacus.
          </Card.Text>
          <Button variant="primary">Go</Button>
        </Card.ImgOverlay>
      </Card>
    </>
  );
}
```

让我们的文本显示在图像之上。

我们用`Card.ImgOverlay`组件做到了这一点。

# 航行

我们可以在卡片的头部添加导航。

为此，我们可以写:

```
import React from "react";
import Card from "react-bootstrap/Card";
import Button from "react-bootstrap/Button";
import Nav from "react-bootstrap/Nav";
import "bootstrap/dist/css/bootstrap.min.css";
export default function App() {
  return (
    <>
      <Card>
        <Card.Header>
          <Nav variant="tabs" defaultActiveKey="#first">
            <Nav.Item>
              <Nav.Link href="#first">Active</Nav.Link>
            </Nav.Item>
            <Nav.Item>
              <Nav.Link href="#link">Link</Nav.Link>
            </Nav.Item>
            <Nav.Item>
              <Nav.Link href="#disabled" disabled>
                Disabled
              </Nav.Link>
            </Nav.Item>
          </Nav>
        </Card.Header>
        <Card.Body>
          <Card.Title>Special title</Card.Title>
          <Card.Text>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent
            ultrices ac dolor nec vestibulum. Maecenas vulputate diam ut sem
            tempus, id eleifend tortor hendrerit. Sed non orci massa. Aliquam
            eget lectus a ante convallis gravida. Donec fringilla odio ut magna
            gravida aliquam. Cras molestie non ante vel dictum. Cras lacinia
            molestie lacus, in lacinia sapien imperdiet in. Sed eleifend laoreet
            finibus. Integer semper dictum eros nec eleifend. Nunc quam mi,
            finibus lacinia metus vitae, dapibus ultricies diam. Vivamus ante
            nisi, pellentesque at lacus eu, vehicula pretium lorem. Nunc vitae
            ligula laoreet, accumsan diam et, auctor mauris. Fusce vitae posuere
            nibh, vitae eleifend quam. Duis a enim lacus.
          </Card.Text>
          <Button variant="primary">Go somewhere</Button>
        </Card.Body>
      </Card>
    </>
  );
}
```

我们在卡片的顶部有一个导航条，因为我们将`Nav`组件添加到了`Card.Header`组件中。

`defaultActiveKey`有激活标签页的键。

这可以更改为`pills`变体，将导航链接显示为按钮。

例如，我们可以写:

```
import React from "react";
import Card from "react-bootstrap/Card";
import Button from "react-bootstrap/Button";
import Nav from "react-bootstrap/Nav";
import "bootstrap/dist/css/bootstrap.min.css";
export default function App() {
  return (
    <>
      <Card>
        <Card.Header>
          <Nav variant="pills" defaultActiveKey="#first">
            <Nav.Item>
              <Nav.Link href="#first">Active</Nav.Link>
            </Nav.Item>
            <Nav.Item>
              <Nav.Link href="#link">Link</Nav.Link>
            </Nav.Item>
            <Nav.Item>
              <Nav.Link href="#disabled" disabled>
                Disabled
              </Nav.Link>
            </Nav.Item>
          </Nav>
        </Card.Header>
        <Card.Body>
          <Card.Title>Special title</Card.Title>
          <Card.Text>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent
            ultrices ac dolor nec vestibulum. Maecenas vulputate diam ut sem
            tempus, id eleifend tortor hendrerit. Sed non orci massa. Aliquam
            eget lectus a ante convallis gravida. Donec fringilla odio ut magna
            gravida aliquam. Cras molestie non ante vel dictum. Cras lacinia
            molestie lacus, in lacinia sapien imperdiet in. Sed eleifend laoreet
            finibus. Integer semper dictum eros nec eleifend. Nunc quam mi,
            finibus lacinia metus vitae, dapibus ultricies diam. Vivamus ante
            nisi, pellentesque at lacus eu, vehicula pretium lorem. Nunc vitae
            ligula laoreet, accumsan diam et, auctor mauris. Fusce vitae posuere
            nibh, vitae eleifend quam. Duis a enim lacus.
          </Card.Text>
          <Button variant="primary">Go somewhere</Button>
        </Card.Body>
      </Card>
    </>
  );
}
```

我们将`Nav`上的`variant`道具改为`pills`，这样我们就得到按钮样式的链接。

# 背景颜色

我们可以用`bg`道具设置背景颜色。

可以使用`text`道具改变文本颜色。

例如，我们可以写:

```
import React from "react";
import Card from "react-bootstrap/Card";
import Button from "react-bootstrap/Button";
import Nav from "react-bootstrap/Nav";
import "bootstrap/dist/css/bootstrap.min.css";
export default function App() {
  return (
    <>
      {[
        "Primary",
        "Secondary",
        "Success",
        "Danger",
        "Warning",
        "Info",
        "Light",
        "Dark"
      ].map((variant, index) => (
        <Card
          bg={variant.toLowerCase()}
          key={index}
          text={variant.toLowerCase() === "light" ? "dark" : "white"}
          style={{ width: "18rem" }}
          className="mb-2"
        >
          <Card.Header>Header</Card.Header>
          <Card.Body>
            <Card.Title>{variant} Card Title </Card.Title>
            <Card.Text>
              Lorem ipsum dolor sit amet, consectetur adipiscing elit.
            </Card.Text>
          </Card.Body>
        </Card>
      ))}
    </>
  );
}
```

我们有`text`道具来改变文本的样式变量。

`bg`道具改变卡片背景的样式变体。

![](img/a683e1105bf3899165732e49f9505f4b.png)

照片由 [𝙸𝚐𝚘𝚛](https://unsplash.com/@sakurayon?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以在我们的卡片上添加显示在文本内容和导航链接下方的图像。

此外，我们可以改变卡片的文字和背景颜色。