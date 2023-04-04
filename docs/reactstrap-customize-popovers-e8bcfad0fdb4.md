# 反应陷阱—自定义弹出窗口

> 原文：<https://blog.devgenius.io/reactstrap-customize-popovers-e8bcfad0fdb4?source=collection_archive---------3----------------------->

![](img/b77bb4ddc2861f24848b48c748a3f7bc.png)

Joshua Sukoff 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Reactstrap 是为 React 制作的一个版本引导程序。

这是一组具有 Boostrap 陷阱样式的 React 组件。

在本文中，我们将看看如何用 Reactstrap 添加 popovers。

# Popovers 投放

弹出式广告可以放在不同的位置。

我们只需要改变`placement`道具来改变位置。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import { Button, Popover, PopoverHeader, PopoverBody } from "reactstrap";export default function App() {
  const [popoverOpen, setPopoverOpen] = React.useState(false); const toggle = () => setPopoverOpen(!popoverOpen); return (
    <div>
      <Button id="Popover" type="button">
        Launch Popover
      </Button>
      <Popover
        placement="bottom"
        isOpen={popoverOpen}
        target="Popover"
        toggle={toggle}
      >
        <PopoverHeader>Popover Title</PopoverHeader>
        <PopoverBody>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla tempus
          fermentum lacus
        </PopoverBody>
      </Popover>
    </div>
  );
}
```

用`placement`支柱将位置设置到底部。

其他可能的值包括`top`、`left`和`right`。

# 不受控制的爆米花

我们不需要控制弹出窗口的状态来打开或关闭它。

这可以通过`UncontroledPopover`组件完成。

如果我们不需要以编程方式控制 popover，这是很有用的。

例如，我们可以这样使用它:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import {
  Button,
  UncontrolledPopover,
  PopoverHeader,
  PopoverBody
} from "reactstrap";export default function App() {
  return (
    <div>
      <Button id="UncontrolledPopover" type="button">
        Launch Popover
      </Button>
      <UncontrolledPopover placement="bottom" target="UncontrolledPopover">
        <PopoverHeader>Popover Title</PopoverHeader>
        <PopoverBody>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla tempus
          fermentum lacus
        </PopoverBody>
      </UncontrolledPopover>
    </div>
  );
}
```

我们有`Button`和`UncontrolledPopover`组件来切换弹出窗口。

触发组件的`id`和`UncontrolledPopover`的`target`必须匹配。

# 重新定位松饼

我们可以在更新时重新定位 popover 内容。

例如，我们可以写:

```
import React from "react";
import "bootstrap/dist/css/bootstrap.min.css";
import {
  Button,
  UncontrolledPopover,
  PopoverHeader,
  PopoverBody,
  Collapse
} from "reactstrap";const PopoverContent = ({ scheduleUpdate }) => {
  const [isOpen, setIsOpen] = React.useState(false); return (
    <>
      <PopoverHeader>Update</PopoverHeader>
      <PopoverBody>
        <Button onClick={() => setIsOpen(!isOpen)}>Click me</Button>
        <Collapse
          isOpen={isOpen}
          onEntered={scheduleUpdate}
          onExited={scheduleUpdate}
        >
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla tempus
          fermentum lacus
        </Collapse>
      </PopoverBody>
    </>
  );
};export default function App() {
  return (
    <div className="text-center">
      <Button id="ScheduleUpdateButton" type="button">
        Open Popover
      </Button>
      <UncontrolledPopover
        trigger="click"
        placement="top"
        target="ScheduleUpdateButton"
      >
        {({ scheduleUpdate }) => (
          <PopoverContent scheduleUpdate={scheduleUpdate} />
        )}
      </UncontrolledPopover>
    </div>
  );
}
```

我们创建了`PopoverContent`组件，它使用一个`scheduleUpdate`道具来让我们在单击按钮时改变位置。

显示或隐藏`Collapse`内容时调用`scheduleUpdate`的`PopoverContent`组件。

内容显示时运行`onEntered`回调，内容隐藏时运行`onExited`回调。

![](img/92d11255f452d4c3852174444bc637f5.png)

照片由[思想目录](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

弹出窗口的位置可以静态或动态更改。

此外，我们可以添加一个`UncontrolledPopover`来让我们显示一个弹出窗口，而不用控制它的状态。