# Reactstrap 列出组内容

> 原文：<https://blog.devgenius.io/reactstrap-list-group-content-e7b0f7ecf7e7?source=collection_archive---------4----------------------->

![](img/b0e3545961715a21a78bcb3562091ece.png)

照片由[亚历山大·索夫罗尼](https://unsplash.com/@alexsofronie?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Reactstrap 是为 React 制作的一个版本引导程序。

这是一组具有 Boostrap 陷阱样式的 React 组件。

在本文中，我们将研究如何使用 Reactstrap 添加自定义列表组内容。

# 自定义内容

我们可以将自定义内容添加到列表组项目中。

例如，我们可以写:

```
import React from "react";
import {
  ListGroup,
  ListGroupItem,
  ListGroupItemHeading,
  ListGroupItemText
} from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <div className="App">
      <ListGroup>
        <ListGroupItem active>
          <ListGroupItemHeading>List group item heading</ListGroupItemHeading>
          <ListGroupItemText>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit.
          </ListGroupItemText>
        </ListGroupItem>
        <ListGroupItem>
          <ListGroupItemHeading>List group item heading</ListGroupItemHeading>
          <ListGroupItemText>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit.
          </ListGroupItemText>
        </ListGroupItem>
        <ListGroupItem>
          <ListGroupItemHeading>List group item heading</ListGroupItemHeading>
          <ListGroupItemText>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit.
          </ListGroupItemText>
        </ListGroupItem>
      </ListGroup>
    </div>
  );
}
```

我们使用`ListGroupItemHeading` 和`ListGroupItemText`分别为列表组项目添加内容和主要文本。

# 脸红

要移除列表组的边框，我们可以给列表组添加`flush`道具。

例如，我们可以写:

```
import React from "react";
import { ListGroup, ListGroupItem } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <div className="App">
      <ListGroup flush>
        <ListGroupItem disabled tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
      </ListGroup>
    </div>
  );
}
```

我们有`ListGroup`的`flush`道具来移除边界。

# 水平列表组

我们可以让这些物品水平放置，而不是垂直放置。

我们只需添加一个`horizontal`道具就可以做到这一点。

此外，我们可以将断点设置为仅在给定断点的情况下变为水平的值。

例如，我们可以写:

```
import React from "react";
import { ListGroup, ListGroupItem } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <div className="App">
      <ListGroup horizontal>
        <ListGroupItem disabled tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
      </ListGroup>
    </div>
  );
}
```

使其始终保持水平。

我们可以写:

```
import React from "react";
import { ListGroup, ListGroupItem } from "reactstrap";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <div className="App">
      <ListGroup horizontal="lg">
        <ListGroupItem disabled tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
        <ListGroupItem tag="a" href="#">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        </ListGroupItem>
      </ListGroup>
    </div>
  );
}
```

仅当屏幕达到`lg`断点或更宽时，使其水平。

![](img/6215247f9df103c791ababe32c46a7c6.png)

[Bruce Galpin](https://unsplash.com/@star2dev?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以添加自定义内容并更改列表组的布局。