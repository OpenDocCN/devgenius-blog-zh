# 反应引导-网格布局、顺序和偏移

> 原文：<https://blog.devgenius.io/react-bootstrap-grid-layout-c9bea6fd231?source=collection_archive---------3----------------------->

![](img/dfde79f9225c5b68412dbda8c47ffbcd.png)

维克多·塔拉舒克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

React Bootstrap 是为 React 开发的 Bootstrap 的一个版本。

这是一组具有引导风格的 React 组件。

在本文中，我们将了解如何使用 React Bootstrap 的网格系统来创建布局。

# 具有多个断点的响应式网格

我们可以让 React Bootstrap 监视多个断点，并相应地调整列的大小。

例如，我们可以写:

```
import React from "react";
import Container from "react-bootstrap/Container";
import Row from "react-bootstrap/Row";
import Col from "react-bootstrap/Col";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Container>
        <Row>
          <Col xs={12} md={8}>
            foo
          </Col>
          <Col xs={6} md={4}>
            bar
          </Col>
        </Row> <Row>
          <Col xs={6} md={4}>
            foo
          </Col>
          <Col xs={6} md={4}>
            bar
          </Col>
          <Col xs={6} md={4}>
            baz
          </Col>
        </Row> <Row>
          <Col xs={6}>foo</Col>
          <Col xs={6}>bar</Col>
        </Row>
      </Container>
    </>
  );
}
```

将列调整到不同的大小。

我们有`xs`和`md`断点来按照我们喜欢的方式调整它们的大小。

例如，`xs={12} md={8}`意味着当`xs`或额外的小断点被命中时，该列将占据网格的所有 12 列。

如果命中了`md`或中间断点，则该列将为 8 列宽。

# 订单和偏移

除了断点道具，我们还可以传入一个具有`order`和`offset`属性的对象。

`order`指定从左到右的视觉顺序。

`offset`是将一列向右移动的列数。

例如，我们可以写:

```
import React from "react";
import Container from "react-bootstrap/Container";
import Row from "react-bootstrap/Row";
import Col from "react-bootstrap/Col";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Container>
        <Row>
          <Col xs>1</Col>
          <Col xs={{ order: 12 }}>2</Col>
          <Col xs={{ order: 1 }}>3</Col>
        </Row>
      </Container>
    </>
  );
}
```

订购我们的专栏。

数字较小的在左边，数字较大的在右边。

因此，带 2 的列在带 3 的列的右边，因为第一列的序号比另一列高。

我们也可以把最左边的写成`first`，把最右边的写成`last`。

例如，我们可以写:

```
import React from "react";
import Container from "react-bootstrap/Container";
import Row from "react-bootstrap/Row";
import Col from "react-bootstrap/Col";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Container>
        <Row>
          <Col xs={{ order: "last" }}>1</Col>
          <Col xs>2</Col>
          <Col xs={{ order: "first" }}>3</Col>
        </Row>
      </Container>
    </>
  );
}
```

我们看到 3 和 1 被翻转了，尽管首先定义了带有 1 的列。

这是因为`order`属性。

我们还可以添加`offset`属性，将一列向右移动给定的列数。

例如，我们可以写:

```
import React from "react";
import Container from "react-bootstrap/Container";
import Row from "react-bootstrap/Row";
import Col from "react-bootstrap/Col";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Container>
        <Row>
          <Col xs={4}>foo</Col>
          <Col xs={{ span: 4, offset: 4 }}>bar</Col>
        </Row>
        <Row>
          <Col xs={{ span: 3, offset: 3 }}>foo</Col>
          <Col xs={{ span: 3, offset: 3 }}>bar</Col>
        </Row>
        <Row>
          <Col xs={{ span: 6, offset: 3 }}>foo</Col>
        </Row>
      </Container>
    </>
  );
}
```

然后根据`span`和`offset`移动立柱。

`span`是从 1 到 12 的列数。

`offset`相对于相邻列向右移动的列数。

# 在行中设置列宽

我们可以在`Row`组件中设置它内部所有列的宽度。

例如，我们可以写:

```
import React from "react";
import Container from "react-bootstrap/Container";
import Row from "react-bootstrap/Row";
import Col from "react-bootstrap/Col";
import "bootstrap/dist/css/bootstrap.min.css";export default function App() {
  return (
    <>
      <Container>
        <Row xs={2} md={4} lg={6}>
          <Col>foo</Col>
          <Col>bar</Col>
        </Row>
        <Row xs={1} md={2}>
          <Col>foo</Col>
          <Col>bar</Col>
          <Col>baz</Col>
        </Row>
      </Container>
    </>
  );
}
```

我们为行中的所有列设置大小。

所以在第一行，如果屏幕是额外的小，那么大小是 2。

如果大小为中，则大小为 4。

如果规模很大，那么规模就是 6。

![](img/63a94668905578d350246b130e9d8730.png)

豪尔赫·萨帕塔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以调整列的大小，并使用各种道具和价值进行移动。

它们可以根据偏移进行排序和间隔。