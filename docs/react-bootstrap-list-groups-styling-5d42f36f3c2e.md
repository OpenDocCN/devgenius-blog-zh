# 反应引导-列表组样式

> 原文：<https://blog.devgenius.io/react-bootstrap-list-groups-styling-5d42f36f3c2e?source=collection_archive---------4----------------------->

![](img/280c7113d288eff6a092312e30538e13.png)

由[杜安·斯美塔纳](https://unsplash.com/@veverkolog?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 向 React 应用程序添加列表组。

# 让它齐平

我们可以添加`variant`属性并将其设置为`'flush'`来从列表组中移除外部边框和圆角。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ListGroup from "react-bootstrap/ListGroup";export default function App() {
  return (
    <div>
      <ListGroup variant="flush">
        <ListGroup.Item>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi
          efficitur massa tellus, non ultrices augue sagittis ornare. Sed
          ultrices ligula in est tempus tincidunt. Nam id consequat lacus, et
          mattis turpis.
        </ListGroup.Item>
        <ListGroup.Item>
          In euismod egestas dictum. Curabitur vel ipsum in eros commodo egestas
          sed sed augue. In hac habitasse platea dictumst. In pretium massa in
          urna tincidunt vulputate. Duis eget tristique nisi, auctor pharetra
          ex. Duis rhoncus, lectus vitae consequat blandit, lectus sem rhoncus
          lorem, sit amet viverra tortor est in mi.
        </ListGroup.Item>
        <ListGroup.Item>
          Ut pretium sollicitudin vehicula. In hac habitasse platea dictumst.
          Morbi fringilla mauris sit amet quam tempor, vulputate consequat dolor
          tempor. Duis magna quam, molestie et velit a, cursus porttitor mi.
          Fusce consectetur faucibus elit ut dictum.
        </ListGroup.Item>
      </ListGroup>
    </div>
  );
}
```

# 水平显示项目

如果我们给它加上`horizontal`道具，我们可以横向展示物品。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ListGroup from "react-bootstrap/ListGroup";export default function App() {
  return (
    <div>
      <ListGroup horizontal>
        <ListGroup.Item>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi
          efficitur massa tellus, non ultrices augue sagittis ornare. Sed
          ultrices ligula in est tempus tincidunt. Nam id consequat lacus, et
          mattis turpis.
        </ListGroup.Item>
        <ListGroup.Item>
          In euismod egestas dictum. Curabitur vel ipsum in eros commodo egestas
          sed sed augue. In hac habitasse platea dictumst. In pretium massa in
          urna tincidunt vulputate. Duis eget tristique nisi, auctor pharetra
          ex. Duis rhoncus, lectus vitae consequat blandit, lectus sem rhoncus
          lorem, sit amet viverra tortor est in mi.
        </ListGroup.Item>
        <ListGroup.Item>
          Ut pretium sollicitudin vehicula. In hac habitasse platea dictumst.
          Morbi fringilla mauris sit amet quam tempor, vulputate consequat dolor
          tempor. Duis magna quam, molestie et velit a, cursus porttitor mi.
          Fusce consectetur faucibus elit ut dictum.
        </ListGroup.Item>
      </ListGroup>
    </div>
  );
}
```

现在文本并排显示。

我们也可以将断点设置为水平显示。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ListGroup from "react-bootstrap/ListGroup";export default function App() {
  return (
    <div>
      {["sm", "md", "lg", "xl"].map((breakpoint, idx) => (
        <ListGroup horizontal={breakpoint} className="my-2" key={idx}>
          <ListGroup.Item>Lorem ipsum dolor sit amet</ListGroup.Item>
          <ListGroup.Item>
            consectetur adipiscing elit. Morbi efficitur massa tellus, non
            ultrices augue sagittis ornare
          </ListGroup.Item>
          <ListGroup.Item>
            Sed ultrices ligula in est tempus tincidunt.
          </ListGroup.Item>
        </ListGroup>
      ))}
    </div>
  );
}
```

我们用一个断点字符串来设置`horizontal`属性，这样，如果我们有给定的断点或更高的断点，这些项目将只水平显示。

`sm`是小国。`md`是中等。`lg`大。而且`xl`是特大号。

# 上下文类别

我们可以改变`ListGroup.Item`的变体，让它成为我们想要的风格。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ListGroup from "react-bootstrap/ListGroup";export default function App() {
  return (
    <div>
      <ListGroup>
        <ListGroup.Item>Lorem ipsum </ListGroup.Item>
        <ListGroup.Item variant="primary">Lorem ipsum</ListGroup.Item>
        <ListGroup.Item variant="secondary">Lorem ipsum</ListGroup.Item>
        <ListGroup.Item variant="success">Lorem ipsum</ListGroup.Item>
        <ListGroup.Item variant="danger">Lorem ipsum</ListGroup.Item>
        <ListGroup.Item variant="warning">Lorem ipsum</ListGroup.Item>
        <ListGroup.Item variant="info">Lorem ipsum</ListGroup.Item>
        <ListGroup.Item variant="light">Lorem ipsum</ListGroup.Item>
        <ListGroup.Item variant="dark">Lorem ipsum</ListGroup.Item>
      </ListGroup>
    </div>
  );
}
```

展示 Bootstrap 附带的不同风格。

它们可以与`action`道具配对，然后将应用悬停和活动样式:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import ListGroup from "react-bootstrap/ListGroup";export default function App() {
  return (
    <div>
      <ListGroup>
        <ListGroup.Item action>Lorem ipsum </ListGroup.Item>
        <ListGroup.Item action variant="primary">
          Lorem ipsum
        </ListGroup.Item>
        <ListGroup.Item action variant="secondary">
          Lorem ipsum
        </ListGroup.Item>
        <ListGroup.Item action variant="success">
          Lorem ipsum
        </ListGroup.Item>
        <ListGroup.Item action variant="danger">
          Lorem ipsum
        </ListGroup.Item>
        <ListGroup.Item action variant="warning">
          Lorem ipsum
        </ListGroup.Item>
        <ListGroup.Item action variant="info">
          Lorem ipsum
        </ListGroup.Item>
        <ListGroup.Item action variant="light">
          Lorem ipsum
        </ListGroup.Item>
        <ListGroup.Item action variant="dark">
          Lorem ipsum
        </ListGroup.Item>
      </ListGroup>
    </div>
  );
}
```

使用`action`道具，当我们突出显示一个项目时，我们将看到不同的样式。

![](img/f36230c4df16241715f0088db6f2f919.png)

尤金·希斯蒂科夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以向列表组项目添加各种样式。

它们也可以水平显示。