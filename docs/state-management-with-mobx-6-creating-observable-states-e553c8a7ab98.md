# MobX 6 的状态管理——创建可观察的状态

> 原文：<https://blog.devgenius.io/state-management-with-mobx-6-creating-observable-states-e553c8a7ab98?source=collection_archive---------3----------------------->

![](img/03c359c720995d674bd940ba0c58927c.png)

基特·苏曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

MobX 是一个简单的 JavaScript 应用程序状态管理解决方案。

在本文中，我们将了解如何使用 MobX 6 将状态管理添加到我们的 JavaScript 应用程序中。

# 创建可观察状态

我们可以通过创建存储来创建一个或多个可观察的状态。

为此，我们写道:

```
import { makeObservable, observable, computed, action, autorun } from "mobx";class Count {
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
  } increment() {
    this.count++;
  }
}const store = new Count(1);autorun(() => {
  console.log(store.count);
});store.increment();
```

我们有包含`count`和`doubleCount`状态的`Count`类。

`count`是一种我们可以操纵的可观察状态。

我们通过在`makeObservable`调用中将`count`属性设置为`observable`来添加这个属性。

`doubleCount` getter 用于创建一个`computed`状态，这是一个从其他可观测状态派生出来的状态。

`increment`方法是通过将`increment`设置为`action`来指示的动作。

我们在行动中操纵可观察的状态值。

在构造函数中，我们将`this.count`设置为`count`来初始化它。

接下来，我们创建`Count`实例，它是商店。

然后我们用回调来调用`autorun`以获得`store.count`值。

只要状态更新，回调就会运行。

所以当调用`store.increment`时，回调将运行以记录`store.count`的最新值。

这也适用于计算状态，因此我们可以写:

```
import { makeObservable, observable, computed, action, autorun } from "mobx";class Count {
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
  } increment() {
    this.count++;
  }
}const store = new Count(1);autorun(() => {
  console.log(store.count);
  console.log(store.doubleCount);
});store.increment();
```

在`autorun`回调中观察两种状态的值。

# 结论

我们可以使用 MobX 为任何客户端 JavaScript 应用程序创建数据存储。