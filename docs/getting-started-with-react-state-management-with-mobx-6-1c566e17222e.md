# MobX 6 React 状态管理入门

> 原文：<https://blog.devgenius.io/getting-started-with-react-state-management-with-mobx-6-1c566e17222e?source=collection_archive---------5----------------------->

![](img/8dbd6ab1cf2e3933f54499399c2e7e50.png)

照片由[维达尔·诺德里-马西森](https://unsplash.com/@vidarnm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

MobX 是 React 应用程序的简单状态管理解决方案。

在本文中，我们将了解如何使用 MobX 6 将状态管理添加到 React 应用程序中。

# 装置

首先，我们可以通过运行以下命令来安装 MobX 包:

```
yarn add mobx
```

用纱线或:

```
npm install --save mobx
```

和 NPM 一起。

为了和 React 一起使用，我们还需要`mobx-react-lite`包。

要安装它，我们运行:

```
npm i mobx-react-lite
```

我们可以添加一个脚本标签来从 CDN 加载 MobX 库。

该脚本的 URL 是:

```
https://cdnjs.com/libraries/mobx
```

或者:

```
https://unpkg.com/mobx/dist/mobx.umd.production.min.js
```

# 简单用法

在我们安装了这些包之后，我们可以使用它们来存储 React 应用程序的全局状态。

例如，我们可以写:

```
import React from "react";
import { makeObservable, observable, computed, action } from "mobx";
import { observer } from "mobx-react-lite";class Count {
  count = 0;
  get doubleCount() {
    return this.count * 2;
  }
  constructor(count) {
    makeObservable(this, {
      count: observable,
      doubleCount: computed,
      increment: action
    });
    this.count = count;
  }increment() {
    this.count++;
  }
}const store = new Count(1);const Counter = observer(({ store: { count } }) => {
  return <div>{count}</div>;
});const DoubleCounter = observer(({ store: { doubleCount } }) => (
  <div>{doubleCount}</div>
));const Incrementer = observer(({ store }) => {
  return (
    <div>
      <button onClick={() => store.increment()}>increment</button>
    </div>
  );
});export default function App() {
  return (
    <div>
      <Incrementer store={store} />
      <Counter store={store} />
      <DoubleCounter store={store} />
    </div>
  );
}
```

创建`Count`商店并在我们的 React 应用程序中使用它。

我们在`Count`类中有`count`状态。

`doubleCount`是从`count`状态的值计算出来的状态。

在构造函数中，我们有`makeObservable`函数来使`count`和`doubleCount`属性处于反应状态。

使其成为可观察的状态。

而`computed`使它成为一个计算状态，这意味着它是一个从一个或多个可观测状态派生出来的反应状态。

然后我们将`this.count`类属性初始化为`count`参数的值。

接下来，我们创建`increment`方法来增加`count`值。

然后我们从`Count`类创建一个`store`，并将`count`状态初始化为 1。

接下来，我们创建获取`store.count`值的`Counter`组件并呈现它。

我们还有`DoubleCounter`组件来呈现`store.doubleCount`值。

接下来，我们有了从 prop 获取`store`的`Incrementer`组件，当我们单击按钮时，调用它的`increment`来增加`count`的值。

我们用 MobX `observer`函数包装所有 3 个组件，让所有 3 个组件都监视存储状态值。

最后，在`App`中，我们添加所有 3 个组件并传入商店，这样当我们单击 increment 时，我们会看到`count`和`doubleCount`都更新了。

# 结论

我们可以使用 MobX 轻松地将状态管理添加到 React 应用程序中。