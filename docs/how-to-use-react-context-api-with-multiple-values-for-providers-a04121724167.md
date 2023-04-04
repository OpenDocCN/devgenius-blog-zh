# 如何对提供者使用带有多个值的 React 上下文 API？

> 原文：<https://blog.devgenius.io/how-to-use-react-context-api-with-multiple-values-for-providers-a04121724167?source=collection_archive---------1----------------------->

![](img/a157a4e73d9582fa40c393f11d12a2df.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Fabrice Villard](https://unsplash.com/@fabulu75?utm_source=medium&utm_medium=referral) 拍摄的照片

React Context API 让我们可以方便地在组件之间共享数据。

有时，我们可能希望在组件之间共享多个状态。

在本文中，我们将研究如何使用一个上下文提供者在组件之间共享多个状态。

# 对提供者使用具有多个值的 React 上下文 API

我们可以将任何想要的东西传递给上下文提供者组件的`value` prop。

因此，与一个提供者共享多个状态没有问题。

例如，我们可以写:

```
import React, { useContext, useState } from "react";
const CountContext = React.createContext("count");const DescendantA = () => {
  const { count, setCount, count2, setCount2 } = useContext(CountContext); return (
    <>
      <button onClick={() => setCount((c) => c + 1)}>Click me {count}</button>
      <button onClick={() => setCount2((c) => c + 1)}>Click me {count2}</button>
    </>
  );
};
const DescendantB = () => {
  const { count, setCount, count2, setCount2 } = useContext(CountContext);return (
    <>
      <button onClick={() => setCount((c) => c + 1)}>Click me {count}</button>
      <button onClick={() => setCount2((c) => c + 1)}>Click me {count2}</button>
    </>
  );
};
export default function App() {
  const [count, setCount] = useState(0);
  const [count2, setCount2] = useState(0);
  return (
    <CountContext.Provider value={{ count, setCount, count2, setCount2 }}>
      <DescendantA />
      <DescendantB />
    </CountContext.Provider>
  );
}
```

我们用`React.createContext`方法创建了`CountContext`。

然后我们将`CountContext.Provider`组件包装在`DescendantA`和`DescendantB`组件上。

我们将在`App`中定义的状态 getter 和 setter 全部传递给`value` prop 中的一个对象。

然后在`DescendantA`和`DescendantB`组件中，我们用`CountContext`调用`useConext`来返回我们传递到`value` prop 中的对象。

我们析构了所有的属性并使用它们。

我们在按钮中显示`count`和`count2`。

我们在按钮点击处理程序中调用`setCount`和`setCount2`。

现在，当我们点击按钮时，我们看到计数上升。

# 结论

我们可以向 React 中的上下文提供者组件的`value` prop 中传递任何我们想要的东西。

所以我们可以在组件之间共享任何东西，只要我们把它们传递给`value` prop。