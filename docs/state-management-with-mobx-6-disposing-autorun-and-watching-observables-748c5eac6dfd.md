# 用 MobX 6 进行状态管理——处理自动运行和观察可观察对象

> 原文：<https://blog.devgenius.io/state-management-with-mobx-6-disposing-autorun-and-watching-observables-748c5eac6dfd?source=collection_archive---------6----------------------->

![](img/170783cad7b03e44754bedc1d4d586ca.png)

法比安·海曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

MobX 是一个简单的 JavaScript 应用程序状态管理解决方案。

在本文中，我们将了解如何使用 MobX 6 将状态管理添加到我们的 JavaScript 应用程序中。

# 处置自动运行

当我们不再需要观察商店的变化时，我们应该处理掉我们创建的任何观察器。

要做到这一点，我们可以调用由`autorun`返回的函数，该函数返回一个函数，让我们在用完它后将其从内存中删除。

例如，我们可以写:

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
}const store = new Count(1);const dispose = autorun(() => {
  console.log(store.count);
});store.increment();
dispose();
```

我们用`count`可观察值、`doubleCount`计算状态和`increment`动作创建一个商店。

然后我们实例化它，并将返回的对象分配给`store`。

接下来，我们调用`autorun`来观看我们想要的值。

我们将返回的函数赋给`dispose`变量。

然后我们调用`store.increment`动作来增加`count`状态值，这将触发`autorun`回调运行。

然后我们调用`dispose`来移除`autorun`创建的观察器。

# 观看可观察的事物

除了使用`autorun`函数来观察 MobX 存储状态，我们还可以使用`reaction`函数。

它接受一个函数，该函数返回我们想要观察的值作为第一个参数。

第一个参数是另一个函数，它将当前和以前的值作为参数进行观察。

然后我们可以在回调的时候用它们做点什么。

例如，我们可以写:

```
import {
  makeObservable,
  observable,
  computed,
  action,
  autorun,
  reaction
} from "mobx";class Count {
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
}const store = new Count(1);reaction(
  () => store.count,
  (currCount, prevCount) => {
    console.log(currCount, prevCount);
  }
);store.increment();
```

我们有和以前一样的商店。

但是我们用两个回调函数调用了`reaction`函数。

第一次回调返回`store.count`状态的值。

第二次回调分别具有`store.count`的当前值和先前值。

第二次回调将在我们调用`store.increment`时运行。

这让我们可以更精细地控制我们想在商店里看什么。

# 结论

我们可以通过 MobX 提供的`reaction`函数，用更细粒度的控制来观察值。

此外，当我们不再需要时，我们应该处理掉由`autorun`函数创建的观察器。