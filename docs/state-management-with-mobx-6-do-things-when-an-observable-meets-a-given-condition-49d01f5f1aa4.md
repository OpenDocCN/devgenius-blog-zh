# MobX 6 的状态管理——当一个可观察对象满足给定的条件时做一些事情

> 原文：<https://blog.devgenius.io/state-management-with-mobx-6-do-things-when-an-observable-meets-a-given-condition-49d01f5f1aa4?source=collection_archive---------3----------------------->

![](img/5e27327aa1bc4231ebd3253dacf2badb.png)

[Julian Hochgesang](https://unsplash.com/@julianhochgesang?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

MobX 是一个简单的 JavaScript 应用程序状态管理解决方案。

在本文中，我们将了解如何使用 MobX 6 将状态管理添加到我们的 JavaScript 应用程序中。

# 当一个可观察对象满足给定的条件时做事情

我们可以使用 MobX 6 提供的`when`函数，在商店的状态处于给定条件时做一些事情。

例如，我们可以写:

```
import { makeObservable, observable, computed, action, when } from "mobx";class Count {
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
}const store = new Count(1);when(
  () => store.count === 3,
  () => console.log(`It's 3`)
);store.increment();
store.increment();
```

我们用`count`可观察值、`doubleCount`计算状态和`increment`动作创建一个商店。

然后我们实例化它，并将返回的对象分配给`store`。

接下来，我们用一个函数调用`when`，该函数返回我们想要在商店中查找的条件。

当我们作为第一个参数传入的函数中返回的条件满足时，第二个回调就运行。

由于我们调用了两次`increment`并且存储的`count`状态被初始化为 1，我们应该会看到控制台日志运行。

如果我们不向`when`传递第二个参数，它将返回一个承诺。

所以我们可以用它来等待一个给定的条件，然后运行代码。

例如，我们可以写:

```
import { makeObservable, observable, computed, action, when } from "mobx";class Count {
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
}const store = new Count(1);const when3 = async () => {
  await when(() => store.count === 3);
  console.log(`It's 3`);
};when3();
store.increment();
store.increment();
```

我们拥有与上一个示例相同的商店。

但在那下面，我们有调用`when`的`when3`函数，函数和之前一样，作为第一个参数。

省略了第二个功能。

之后我们称之为`console.log`。

然后我们运行`when3`函数。

接下来，我们像以前一样调用`increment`两次。

因此，我们应该看到控制台日志在`count`达到 3 后运行，正如我们在`when3`函数中指定的那样。

# 结论

我们可以使用 MobX 6 提供的`when`函数，在一个商店的状态满足我们正在寻找的条件后做一些事情。