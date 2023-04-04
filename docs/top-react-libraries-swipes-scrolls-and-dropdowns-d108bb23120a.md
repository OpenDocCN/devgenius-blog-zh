# 顶级反应库—滑动、滚动和下拉

> 原文：<https://blog.devgenius.io/top-react-libraries-swipes-scrolls-and-dropdowns-d108bb23120a?source=collection_archive---------8----------------------->

![](img/29b3a403ea60b39b509200e2193bb62b.png)

照片由[思想目录](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# 反应灵活

React Swipeable 包允许我们向组件添加滑动处理程序。

要安装它，我们运行:

```
npm i react-swipeable
```

然后我们可以通过写来使用它:

```
import React from "react";
import { useSwipeable } from "react-swipeable";export default function App() {
  const handlers = useSwipeable({
    onSwiped: eventData => console.log("swiped")
  });
  return (
    <div className="App">
      <div {...handlers}> You can swipe here </div>
    </div>
  );
}
```

我们有`handlers`对象，它有滑动的处理程序。

对象有`onSwiped`、`onSwipedLeft`、`onSwipedRight`、`onSwipedUp`、`onSwipedDown`和`onSwiping`等方法。

`useSwipeable`钩子会自动返回这些。

我们所要做的就是为滑动传递一个事件处理器。

我们也可以使用`Swipeable`组件。

例如，我们可以写:

```
import React from "react";
import { Swipeable } from "react-swipeable";export default function App() {
  return (
    <div className="App">
      <Swipeable onSwiped={eventData => console.log("swiped")}>
        You can swipe here!
      </Swipeable>
    </div>
  );
}
```

做同样的事情。

`eventData`有许多数据，如 x 和 y 坐标变化、速度和方向。

# RC-下拉菜单

rc-dropdown 是一个提供菜单组件的包。

要安装它，我们可以运行:

```
npm i rc-dropdown
```

然后我们可以写:

```
import React from "react";
import Dropdown from "rc-dropdown";
import Menu, { Item as MenuItem, Divider } from "rc-menu";
import "rc-dropdown/assets/index.css";function onSelect({ key }) {
  console.log(`${key} selected`);
}function onVisibleChange(visible) {
  console.log(visible);
}const menu = (
  <Menu onSelect={onSelect}>
    <MenuItem disabled>disabled</MenuItem>
    <MenuItem key="1">foo</MenuItem>
    <Divider />
    <MenuItem key="2">bar</MenuItem>
  </Menu>
);export default function App() {
  return (
    <div>
      <div style={{ margin: 20 }}>
        <div style={{ height: 100 }} />
        <div>
          <Dropdown
            trigger={["click"]}
            overlay={menu}
            animation="slide-up"
            onVisibleChange={onVisibleChange}
          >
            <button style={{ width: 100 }}>open</button>
          </Dropdown>
        </div>
      </div>
    </div>
  );
}
```

去使用它。

我们有带有下拉菜单的`Dropdown`组件。

它有`trigger`属性来指定如何触发菜单。

`overlay`有菜单覆盖。

`onVisibleChange`是一个事件处理函数，当菜单可见性改变时运行。

菜单在`menu`组件中，我们使用`Menu`组件来创建菜单。

`MenuItem`有菜单项。

`Divider`有分频器。

`Menu`获取一个`onSelect`道具，当一个物品被选中时运行。

这个包还提供了自己的 CSS 来设计菜单。

其他功能包括过渡、点击等。

# 反应航路点

React Waypoint 允许我们添加一个组件来监视滚动到给定的位置。

要安装它，我们运行:

```
npm i react-waypoint
```

然后我们可以通过写来使用它:

```
import React from "react";
import { Waypoint } from "react-waypoint";export default function App() {
  const handleWaypointEnter = e => console.log(e);
  const handleWaypointLeave = e => console.log(e); return (
    <div>
      <div>
        {Array(1000)
          .fill()
          .map((_, i) => {
            if (i !== 500) {
              return <p key={i}>item {i + 1}</p>;
            }
            return (
              <Waypoint
                key={500}
                onEnter={handleWaypointEnter}
                onLeave={handleWaypointLeave}
              />
            );
          })}
      </div>
    </div>
  );
}
```

我们创建了一个包含`p`元素的数组。

在数组的中间，我们返回`Waypoint`组件来监视滚动到那个位置。

当我们滚动到`Waypoint`时，`onEnter`处理程序运行，当我们滚动离开它时，`onLeave`运行。

它需要`onPositionChange`的处理程序，是否检查水平滚动等等。

![](img/fb4bf61e2991854744badb33b6957cf9.png)

照片由 [Alvan Nee](https://unsplash.com/@alvannee?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

React Swipeable 允许我们处理组件的滑动。

rc-dropdown 是一个简单的下拉库，可以让我们添加它们。

React Waypoint 是一个让我们观察滚动到给定位置的包。