# 面向对象的 JavaScript——观察者和测试

> 原文：<https://blog.devgenius.io/object-oriented-javascript-observers-and-tests-6a441706cb46?source=collection_archive---------6----------------------->

![](img/98b1bf0d2a8f946696ee1de4265b0ee3.png)

照片由 [Kristijan Arsov](https://unsplash.com/@aarsoph?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究一些基本的设计模式和测试。

# 观察者模式

观察者模式是我们通过订阅一个对象来观察来自一个源的变化，并在发生变化时将变化发布给订阅的观察者。

我们可以创建一个对象，通过编写以下代码来获取观察器并向它们发布更改:

```
const observer = {
  subscribers: [],
  addSubscriber(callback) {
    if (typeof callback === "function") {
      this.subscribers.push(callback);
    }
  }, removeSubscriber(callback) {
    const index = this.subscribers.findIndex(s => s === callback);
    this.subscribes.splice(index, 1);
  }, publish(obj) {
    for (const sub of this.subscribers) {
      if (typeof sub === 'function') {
        sub(obj);
      }
    }
  },
};
```

我们创建一个接受回调的`observer`对象。

`callback`是我们添加到`subscribers`数组中的函数。

此外，我们有`removeSubscriber`数组来移除回调。

`publish`获取一个包含要发布的内容的对象，并回调`this.subscribers`数组中的回调函数来发布更改。

然后我们可以通过写来使用它:

```
const callback = (e) => console.log(e);
const callback2 = (e) => console.log(2, e);
observer.addSubscriber(callback);
observer.addSubscriber(callback2);
observer.publish('foo')
```

我们创建了 2 个回调，并用`addSubscriber`方法将它们添加到`subscribers`数组中。

然后我们调用`publish`来调用回调，回调记录了更改。

我们应该看到:

```
foo
2 "foo"
```

记录在控制台日志中。

观察者模式很好，因为回调和`observer`对象之间没有太多的耦合。

当有一些信息要发送给他们时，我们就调用回调函数。

回调对`observer`一无所知。

# 测试

测试是编写 JavaScript 应用程序的重要部分。

我们可以通过编写自动运行的测试来使我们的生活变得更容易。

没有足够的测试是一个坏主意。

通过测试，我们确保现有代码的行为符合我们的规范。

任何新的代码变化都不会破坏我们的规范所定义的现有行为。

有了测试，我们不必亲自动手测试所有的东西。

此外，当我们重构时，我们不必自己再次检查所有的东西。

如果我们的改变使测试失败，那么我们知道有问题。

# 单元测试

单元测试是我们最擅长的。

他们很矮，跑得很快。

他们在给定一些输入的情况下测试输出。

它们应该是自解释的、可维护的和可读的。

它们在任何顺序下都应该以相同的方式工作。

# 测试驱动开发

测试驱动开发最近被大量使用，

方法工作流从编写一些失败的测试开始。

然后我们编写代码使测试通过。

然后我们再次运行测试，看看他们是否通过。

然后我们重构代码，确保我们的测试仍然通过。

# 行为驱动发展

行为驱动开发是一种方法，在这种方法中，我们创建测试来测试代码的行为。

我们用一些输入进行测试，并检查输出。

![](img/9f753e6b7bf3965d5674a2c8f6506f0f.png)

[绿色变色龙](https://unsplash.com/@craftedbygc?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以创建单元测试来测试我们的代码。

此外，观察者模式允许我们创建松散耦合的代码，从观察者到另一个对象进行通信。