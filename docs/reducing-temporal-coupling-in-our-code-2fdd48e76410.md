# 减少代码中的时间耦合

> 原文：<https://blog.devgenius.io/reducing-temporal-coupling-in-our-code-2fdd48e76410?source=collection_archive---------33----------------------->

![](img/c5ae31769944e20571ca51a0d46742fb.png)

由 [Alex Iby](https://unsplash.com/@alexiby?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

时间通常是软件架构中被忽略东西。我们唯一关心的是我们的日程安排。

然而，在我们自己的代码中，我们也要关心时间。我们必须关心并发性和排序。

在本文中，我们将看看如何减少代码中的时间耦合。

# 允许并发

我们必须在代码中考虑到并发性。这样，如果没有必要，我们就不必强迫一个过程在另一个过程之前运行。

这通过允许不相互依赖的事情并发运行来提高性能，节省每个人的时间。

# 工作流程

我们必须分析我们程序的工作流，看看哪些部分在排序方面可以相互解耦。

例如，如果我们发出 HTTP 请求来获取不相关的数据，那么我们可以通过让它们并行运行来分离这些工作流。

如果他们不必以串行方式运行，那么串行运行和浪费时间是没有意义的。

例如，如果不相关的数据来自不同的来源，不需要编写下面的代码来获得这两个数据:

```
async () => {
  const michael = await fetch('https://api.agify.io?name=michael')
  const joe = await fetch('https://api.agify.io?name=joe')
}
```

我们可以改为编写以下代码来并行获取它们:

```
async () => {
  const responses = await Promise.all([
    fetch('https://api.agify.io?name=michael'),
    fetch('https://api.agify.io?name=joe')
  ])
}
```

我们可以看到第一个片段的运行时间比第二个长得多。

因此，如果我们允许它们并发运行，那么我们就可以确保这一个请求不会被不必要的延迟，因为一开始它们是不相关的，所以它们可以并发运行。

# 并行设计

线性代码导致编程草率。对于并发性，我们必须考虑基于时间的依赖性。

我们必须确保一种资源在另一种资源之前可用。若要调用对象，我们必须确保状态是我们想要的，然后才能调用下一个过程，该过程取决于上一个过程产生的值。

因此，我们必须确保检查有效值，如果它们不存在，就等待它们。

不同语言获取相关数据的方式不同。例如，在 JavaScript 中，我们通过使用 promises 来确保在调用下一个异步进程之前获得来自异步进程的依赖数据。

有了承诺，我们可以将它们链接起来，这样在运行下一个承诺之前，我们可以确保上一个承诺返回我们想要的值。

例如，如果我们有以下 JavaScript 代码:

```
const foo = async () => {
  const res = await fetch('https://yesno.wtf/api')
  const {
    answer
  } = res.json();
  if (answer === 'yes') {
    const res = await fetch('https://api.agify.io?name=joe')
    const result = res.json();
    console.log(result)
  }
}
```

然后，在调用下一个 API 获取更多数据之前，我们确保从响应体获得了属性`answer`，并且它的值是`'yes'`。

这是我们在 JavaScript 中经常遇到的事情，所以在我们做任何事情之前，都必须检查 promise 代码是否符合我们的要求。

![](img/f84cf2700ebb651a8d50e6422c76ac12.png)

卡莉·雷·霍布斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 部署

我们还可以灵活部署我们的应用程序。我们可以一次部署整个系统，也可以只部署其中的一部分。

如果我们将它们划分成更小的服务，那么我们就不必每次对系统的某个部分做一些小的改变就部署整个系统。

将它们解耦可以让我们同时部署不同的部分，而不是一次性部署所有的部分。

向非并发系统添加并发性要困难得多，所以我们在设计系统时必须考虑到这一点。

# 结论

允许并发是我们在设计系统时必须考虑的事情。

我们不希望不相关的东西串行运行，这样它们就不会不必要地耦合在一起，并且通过不必等待他们不必等待的东西来节省用户的时间。

并发也适用于部署。如果我们将我们的系统划分成更小的服务，那么我们只需要在需要部署的时候部署那部分服务。

这比每次我们有一个小的变化就部署整个系统要好得多。