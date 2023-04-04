# 面向对象的 JavaScript —异步编程

> 原文：<https://blog.devgenius.io/object-oriented-javascript-async-programming-975250f63ba8?source=collection_archive---------5----------------------->

![](img/d4dc90aa1181cde9e0103f96a3af3df8.png)

由 [Berkay Gumustekin](https://unsplash.com/@berkaygumustekin?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究 JavaScript 异步编程。

# 异步编程

异步编程是 JavaScript 应用程序的重要组成部分。

它不同于替代模式，后者是同步编程。

同步编程是我们逐行运行代码的地方。

异步编程是我们运行一些代码，然后等待结果。

当我们等待结果时，我们让队列中的其他代码段运行。

一旦得到结果，我们就运行等待结果的代码。

这是不同的，因为我们不是一行一行地运行任何东西。

我们将代码排队并初始化它们。

一旦计算出结果，我们就返回运行初始化的代码。

当代码等待时，延迟是不可预测的。

因此，我们不能只运行代码，然后等待结果，而不让其他代码运行。

# JavaScript 调用堆栈

调用堆栈是从最近到最早调用的函数的记录。

例如，如果我们有:

```
function c(val) {
  console.log(new Error().stack);
}function b(val) {
  c(val);
}function a(val) {
  b(val);
}
a(1);
```

然后我们得到:

```
Error
    at c ((index):37)
    at b ((index):41)
    at a ((index):45)
    at (index):47
```

从控制台日志中。

我们看到`a`首先被调用，然后是`b`，然后是`c`。

当`c`返回时，栈顶弹出。

然后返回时弹出`b`。

最后弹出`a`。

# 事件循环

浏览器选项卡在单个线程中运行。

它在事件循环中运行。

循环不断地从消息队列中挑选消息，并运行与之相关的回调。

当其他流程将任务添加到消息队列时，事件循环不断地从消息队列中挑选任务。

像计时器和事件处理程序这样的进程并行运行，并将任务添加到队列中。

# 定时器

`setTimeout`函数创建一个定时器并等待它触发。

当计时器运行时，一个任务被添加到消息队列中。

它需要一个回调和以毫秒为单位的持续时间。

一旦达到持续时间，就会运行回调。

当回调运行时，浏览器中的其他交互将被阻止。

# 复试

Node 推广了自己的回调风格。

回调采用一个`err`和`data`参数。

`err`有错误对象。

`data`有结果数据。

例如，我们有:

```
fs.readFile('/foo.txt', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

它有一个带有`err`和`data`的回调。

`fs.readFile`异步读取文件并获取内容。

![](img/fef3bf50c1c5601cbbe614353185a88b.png)

[摄于](https://unsplash.com/@marliesebrandsma?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 的马莉斯·斯特里兰

# 结论

JavaScript 需要大量的异步代码，因为它只在一个线程上运行。

它需要将任务排队，然后异步运行。

这样就不会耽误主线了。