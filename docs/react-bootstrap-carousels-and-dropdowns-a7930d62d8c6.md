# 反应引导—旋转木马和下拉菜单

> 原文：<https://blog.devgenius.io/react-bootstrap-carousels-and-dropdowns-a7930d62d8c6?source=collection_archive---------4----------------------->

![](img/cb934aea77c7d5c435733b44d276c303.png)

照片由 [Ricky Kharawala](https://unsplash.com/@sweetmangostudios?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何定制 React 引导转盘和下拉菜单。

# 旋转木马

旋转木马是让我们在元素间循环的幻灯片组件。

幻灯片可以有图像或文本。

例如，我们可以通过书写来使用它们:

```
import React from "react";
import Card from "react-bootstrap/Card";
import Carousel from "react-bootstrap/Carousel";
import "bootstrap/dist/css/bootstrap.min.css";
export default function App() {
  return (
    <>
      <Carousel>
        <Carousel.Item>
          <img
            className="d-block w-100"
            src="http://placekitten.com/500/500"
            alt="First slide"
          />
          <Carousel.Caption>
            <h3>First slide</h3>
            <p>
              Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent
              ultrices ac dolor nec vestibulum.{" "}
            </p>
          </Carousel.Caption>
        </Carousel.Item>
        <Carousel.Item>
          <img
            className="d-block w-100"
            src="http://placekitten.com/500/500"
            alt="Third slide"
          /> <Carousel.Caption>
            <h3>Second slide</h3>
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
          </Carousel.Caption>
        </Carousel.Item>
        <Carousel.Item>
          <img
            className="d-block w-100"
            src="http://placekitten.com/500/500"
            alt="Third slide"
          /> <Carousel.Caption>
            <h3>Third slide</h3>
            <p>
              Integer non ante ut arcu imperdiet maximus. Pellentesque id metus
              porttitor,
            </p>
          </Carousel.Caption>
        </Carousel.Item>
      </Carousel>
    </>
  );
}
```

我们导入了`Carousel`组件，该组件具有显示项目的`Carousel.Item`。

在里面，我们可以显示一个图像。

此外，我们可以用`Carousel.Caption`组件为图像添加标题。

我们可以在标题中包含任何内容。

# 受控幻灯片

我们可以使用`activeIndex`和`onSelect`处理器来控制传送带的状态。

例如，我们可以写:

```
import React from "react";
import Carousel from "react-bootstrap/Carousel";
import "bootstrap/dist/css/bootstrap.min.css";
export default function App() {
  const [index, setIndex] = React.useState(0); const handleSelect = (selectedIndex, e) => {
    setIndex(selectedIndex);
  };return (
    <>
      <Carousel activeIndex={index} onSelect={handleSelect}>
        <Carousel.Item>
          <img
            className="d-block w-100"
            src="http://placekitten.com/500/500"
            alt="First slide"
          />
          <Carousel.Caption>
            <h3>First slide</h3>
            <p>
              Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent
              ultrices ac dolor nec vestibulum.{" "}
            </p>
          </Carousel.Caption>
        </Carousel.Item>
        <Carousel.Item>
          <img
            className="d-block w-100"
            src="http://placekitten.com/500/500"
            alt="Third slide"
          /><Carousel.Caption>
            <h3>Second slide</h3>
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
          </Carousel.Caption>
        </Carousel.Item>
        <Carousel.Item>
          <img
            className="d-block w-100"
            src="http://placekitten.com/500/500"
            alt="Third slide"
          /><Carousel.Caption>
            <h3>Third slide</h3>
            <p>
              Integer non ante ut arcu imperdiet maximus. Pellentesque id metus
              porttitor,
            </p>
          </Carousel.Caption>
        </Carousel.Item>
      </Carousel>
      <p>{index}</p>
    </>
  );
}
```

我们有`index`状态和`setIndex`功能来改变它。

然后，我们传递`index`作为`activeIndex`属性的值，让我们设置要转到的幻灯片。

`onSelect`有`handleSelect`函数作为值，让我们在转盘导航完成时设置索引。

然后，我们在转盘下方显示当前幻灯片的`index`。

# 下拉菜单

我们可以使用`Dropdown`组件向 React 应用程序添加下拉菜单。

例如，我们可以写:

```
import React from "react";
import Dropdown from "react-bootstrap/Dropdown";
import "bootstrap/dist/css/bootstrap.min.css";
export default function App() {
  return (
    <>
      <Dropdown>
        <Dropdown.Toggle variant="primary">Dropdown Button</Dropdown.Toggle> <Dropdown.Menu>
          <Dropdown.Item href="#/foo">foo</Dropdown.Item>
          <Dropdown.Item href="#/bar">bar</Dropdown.Item>
          <Dropdown.Item href="#/baz">baz</Dropdown.Item>
        </Dropdown.Menu>
      </Dropdown>
    </>
  );
}
```

我们有`Dropdown`组件，它有`Dropdown.Toggle`组件来显示切换。

`Dropdown.Menu`显示点击切换按钮时的菜单。

`href`可以有任何我们想去的网址。

`variant`是造型变型。

为了使它更短，我们可以使用`DropdownButton`组件将`Dropdown`和`Dropdown.Toggle`组件合二为一。

例如，我们可以写:

```
import React from "react";
import Dropdown from "react-bootstrap/Dropdown";
import DropdownButton from "react-bootstrap/DropdownButton";
import "bootstrap/dist/css/bootstrap.min.css";
export default function App() {
  return (
    <>
      <DropdownButton title="Dropdown button">
        <Dropdown.Item href="#/foo">foo</Dropdown.Item>
        <Dropdown.Item href="#/bar">bar</Dropdown.Item>
        <Dropdown.Item href="#/baz">baz</Dropdown.Item>
      </DropdownButton>
    </>
  );
}
```

`title`道具有切换的文本内容。

取消了`Dropdown.Toggle`和`Dropdown.Menu`包装器，因为我们有了`Dropdown.button`组件。

# 风格变体

我们可以设置`variant`属性来改变下拉菜单的样式。

例如，我们可以写:

```
import React from "react";
import Dropdown from "react-bootstrap/Dropdown";
import DropdownButton from "react-bootstrap/DropdownButton";
import ButtonGroup from "react-bootstrap/ButtonGroup";
import "bootstrap/dist/css/bootstrap.min.css";
export default function App() {
  return (
    <>
      {["Primary", "Secondary", "Success", "Info", "Warning", "Danger"].map(
        variant => (
          <DropdownButton
            as={ButtonGroup}
            key={variant}
            variant={variant.toLowerCase()}
            title={variant}
          >
            <Dropdown.Item eventKey="1">foo</Dropdown.Item>
            <Dropdown.Item eventKey="2">bar</Dropdown.Item>
            <Dropdown.Item eventKey="3" active>
              Active Item
            </Dropdown.Item>
            <Dropdown.Divider />
            <Dropdown.Item eventKey="4">baz</Dropdown.Item>
          </DropdownButton>
        )
      )}
    </>
  );
}
```

显示所有颜色的下拉列表。

![](img/3fc7e6924b7f95faa0500822a1dcdb6b.png)

由[切尔西·福彻](https://unsplash.com/@chalex95?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

旋转木马是幻灯片中带有文本和图像的幻灯片。

下拉菜单可以有很多种。