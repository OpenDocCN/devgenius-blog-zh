# 反应模式—合成

> 原文：<https://blog.devgenius.io/react-patterns-composition-29e4f04a23df?source=collection_archive---------14----------------------->

![](img/1fda8ef09cd11f758ceecd6fed512f9e.png)

Jacques Bopp 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将研究如何组合 React 组件。

# 组件之间的通信

重用功能始终是开发人员努力追求的目标。

React 使可重用组件变得容易。

具有清晰界面的小组件可以很容易地组合在一起，以创建强大且可维护的复杂应用程序。

例如，我们可以通过编写以下代码来创建组件:

```
const Item = ({ name }) => <p>{name}</p>;
```

这是一个简单的组件，它只接受`name`道具并显示它。

这是我们可以努力创造的理想组件。

`name`是道具。它是一个从父组件到子组件的值。

当一个组件把一些道具传递给另一个组件时，就叫做拥有者。

# 儿童

有一个特殊的道具可以从所有者传递给被称为子组件的组件。

孩子们的财产并不能说明它的价值。

它包含标签之间的组件。

例如，我们可以写:

```
<button>click me</button>
```

那么`click me`就是`children`的值。

为了让我们自己的按钮做同样的事情，我们可以写:

```
const Button = ({ children }) => <button>{children}</button>;
```

我们有一个带着 T5 道具的 T4。

然后我们可以用它来写:

```
import React from "react";const Button = ({ children }) => <button>{children}</button>;export default function App() {
  return <Button>click me</Button>;
}
```

好的一面是我们可以在组件标签之间传递任何东西。

所以我们可以写:

```
import React from "react";const Button = ({ children }) => <button>{children}</button>;export default function App() {
  return (
    <Button>
      <b>click me</b>
    </Button>
  );
}
```

然后我们得到一个粗体文本的按钮。

我们可以通过编写以下代码来验证子道具类型:

```
Button.propTypes = {
  children: PropTypes.oneOfType([PropTypes.array, PropTypes.element])
};
```

现在我们不会在将道具传入`Button`组件时犯任何错误。

# 容器和表示模式

React 组件通常将逻辑与表示混合在一起。

逻辑是任何与 UI 无关的东西，比如 API 调用、数据操作等等。

表示是我们在创建组件来创建 UI 时返回的东西。

为了使我们的代码简洁，我们应该将逻辑和表示分开。

分离逻辑和表示使它们更加可重用。

例如，我们可以将逻辑从演示中分离出来，写为:

```
import React, { useState, useEffect } from "react";const fetchPerson = async name => {
  const res = await fetch(`https://api.agify.io/?name=${name}`);
  return await res.json();
};export default function App() {
  const [data, setData] = useState({}); const onLoad = async name => {
    const personData = await fetchPerson(name);
    setData(personData);
  }; useEffect(() => {
    onLoad("michael");
  }, []);
  return <p>{data.name}</p>;
}
```

`fetchPerson`有逻辑。它用于从 API 获取数据。

`App`已经演示完毕。

我们通过调用`fetchPerson`获取数据，并调用`setData`更新数据，以便可以显示。

责任是明确的。它们轮廓分明，每个部分都很小。

表示组件可以有状态。我们只有显示数据所需的状态。

我们可以通过将表示代码移动到更小的组件中来进一步分离它。

例如，我们可以创建一个哑组件:

```
import React, { useState, useEffect } from "react";
const fetchPerson = async name => {
  const res = await fetch(`https://api.agify.io/?name=${name}`);
  return await res.json();
};const Person = ({ data: { name } }) => <p>{name}</p>;export default function App() {
  const [data, setData] = useState({});
  const onLoad = async name => {
    const personData = await fetchPerson(name);
    setData(personData);
  };
  useEffect(() => {
    onLoad("michael");
  }, []); return <Person data={data} />;
}
```

我们将 p 元素移动到它自己的组件中，这样在`App`组件中就没有任何逻辑了。

`App`只是充当连接表示和逻辑的容器。

组件越小，越多的人可以在代码的同一部分工作。

这是因为如果他们很小，那么多人改变同一个组件的可能性就很小。

如果我们使用迭代过程来构建应用程序，那么这将使应用程序的构建过程更快。

然而，它确实涉及到创建更多的文件和组件。

因此，我们不应该总是应用这个。当这种模式有意义时，我们应该采用它。

当我们需要再利用的时候，我们就分开。

![](img/45ef4130a3a90aa5688086a013f1d324.png)

照片由[ярославалексеенко](https://unsplash.com/@webaliser?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用不同的组件轻松地创建可重用的组件。

很容易做到这一点，通过将逻辑分成函数，将表示代码分成组件。