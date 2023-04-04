# NodeJS 中的并行执行

> 原文：<https://blog.devgenius.io/parallel-execution-in-nodejs-3fd40f30ee14?source=collection_archive---------3----------------------->

![](img/b31021cd2a7267495f593815f3ec4393.png)

NodeJS 是 JavaScript 的运行时环境。它是服务器端和单线程的。也就是说，我们希望异步并行地做事。

现在，Node 使用了几个线程，只有一个执行线程，并且加入了很多东西来使它异步，比如队列和 libuv 库。

使节点程序异步的最基本的方法是使用回调。然而，有很多地方可能出错，我想讨论的是使用承诺来获得更流畅的结果。

回调可以很容易地转换为承诺，就像内置的 promisify 一样，使用承诺可以使您的代码更易于管理，并有助于避免回调地狱。

# 什么是异步？

异步基本上意味着我们可以让事件和函数独立于主执行线程运行，这样我们的线程就不需要等待事件完成才能继续运行。然后，我们有回调或处理程序来处理独立于主线程运行的事件的响应

*即*

```
fetch(file).then(console.log)
```

在这个例子中，我使用 fetch() API(一个基于 Promise 的 XMLHttpRequest 版本)来获取一些数据，当收到来自调用的响应时，我将它记录到控制台。

当 fetch() API 运行时，主执行线程的其余部分也将继续运行，直到它完成，我们才不会停留在这一行。

# 让我们编码

我们的设置将非常简单。显然，您需要安装节点；我会等…

明白了吗？接下来创建您的项目目录，在您最喜欢的编辑器中打开它(提示:VSCode)并打开一个新文件，我们将他命名为 index.js。

我们唯一剩下要做的事是。

想知道进度条是如何在终端中出现的吗？log-update 是一个很棒的包，它给了我们这种能力，我们可以“更新”一个进度条，而不用在终端中看到它的每一次迭代。

所以，让我们创造我们的第一个承诺。

```
const delay = (seconds, fn) => {
    setTimeout(fn, seconds*1000);
}

delay(1, () => {
    console.log('one second');
})
```

不，这不是一个承诺，但我们必须首先明白我们想要实现什么。上面的代码创建了一个延迟函数，它接受两个参数，一个数字和一个回调函数。delay 函数然后设置一个计时器，使用我们传递给它的数字作为计时器延迟的秒数，并将我们的回调传递给所述计时器。

*注意:setTimout 本身是一种异步编程的方法，考虑到 setTimout 函数只是浏览器定时器功能的一个标签，它设置了一个在我们的主执行线程或全局调用堆栈完成运行后运行的回调。*

在我们定义了 delay 之后，我们调用它，将秒数设置为 1，并传递一个要赋给 fn 的函数定义。这个定义将运行`console.log('one second')`

如果我们想要创建更多的延迟，或者按顺序运行更多的函数，我们可以像这样嵌套它们:

```
console.log('starting delay')
delay(2, ()=>{
    console.log('two seconds');
    delay(1, ()=>{
        console.log('three seconds');
        delay(1, ()=>{
            console.log('four seconds')
        })
    })
})
```

我们称这种回调为地狱，这是有道理的。那么我们如何避免这种情况呢？承诺。

在 ES6 中，我们被提供了承诺，当被执行时，会做两件事。

让我们重新看看 fetch() API:

```
const data = fetch('api/uri');
data.then(...)
```

当我们运行 fetch()时，它返回一个 Promise 对象，这是一个允许我们知道我们有一些事情悬而未决的响应，而不是像 ES6 之前那样被蒙在鼓里，它还通过 web 浏览器发出网络请求。一旦请求得到响应，它就会用数据填充 Promise 对象。

因此，简而言之，它在运行其核心功能的同时返回一个响应。

记住这一点，让我们修正我们的延迟函数:

```
const delay = (sec) => new Promise((resolve, reject) => {
    setTimeout(resolve('me last'), sec*1000);
}) 
​
delay(1).then(console.log)
console.log('me first')
```

我们将 delay 定义为一个箭头函数，它运行`new Promise`创建一个 promise 对象，现在分配给标签 delay。Delay 将接受秒数的参数，正如您所看到的，函数定义正在被传递给`new Promise`。

这个函数就是 executor，它基本上是自动运行的，一旦有结果，就调用 resolve 或 reject。Resolve 和 reject 是 JavaScript 提供的回调函数，它将返回一条消息，无论是错误还是成功。

在我们的例子中，我们所做的只是设置一个计时器并记录一个成功消息，所以我们并不真正需要 reject 回调。当我们调用`.then()`时，我们会自动传递返回的消息“我在最后”，然后将其记录到控制台。在控制台中，您将看到:

```
me first
me last
```

尽管 delay 是先运行的，但我们不必等待它完成，我们继续执行线程并记录“me first”。

好吧，我已经说了很多关于异步的事情。让我们看看代码，对吗？

index.js:

```
const log_ = require('log-update');
​
const toX = () => "X"
​
const delay = (sec) => new Promise((resolve) => {
    setTimeout(resolve, sec*1000);
});
​
let promises = [
  delay(4),
  delay(6),
  delay(4),
  delay(3),
  delay(5),
  delay(7),
  delay(9),
  delay(10),
  delay(3),
  delay(5)
];
​
class PromiseQueue{
  constructor(promises=[], concurrent=1){
    this.concurrent = concurrent;
    this.total = promises.length;
    this.todo = promises;
    this.running = [];
    this.complete = [];
  }
​
  get runAnother(){
    return (this.running.length < this.concurrent) && this.todo.length;
  }
​
  graphTasks(){
    let {todo, running, complete} = this;
    log_(`
    todo: [${todo.map(toX)}]
    running: [${running.map(toX)}]
    complete: [${complete.map(toX)}]
    `)
  }
​
  runTasks(){
    while(this.runAnother){
      let promise = this.todo.shift();
      promise.then(()=>{
        this.complete.push(this.running.shift());
        this.graphTasks()
        this.runTasks()
      })
      this.running.push(promise);
      this.graphTasks()
    }
  }
}
​
​
const queue = new PromiseQueue(promises, 2);
queue.runTasks();
```

控制台:

```
todo: [X]
running: [X,X]
complete: [X,X,X,X]
```

那么…发生了什么事？

嗯，我们首先导入我们方便的`log-update`包。完成了。

然后，我们有一个简单的单行箭头函数，基本上只是返回一个 x。

我们的延期承诺产生了。太棒了。

在那之后，事情开始变得新。我们分手吧。

第一部分并不可怕，我们只是列出了延迟调用的列表。我们把这个列表贴在一个叫做承诺的标签上。

```
let promises = [
  delay(1),
  delay(3),
  delay(2),
  delay(5),
  delay(3),
  delay(9),
  delay(11)
];
```

现在，ES6 中也引入了下一位，class 关键字。但是，如果你熟悉 ES6 之前的面向对象编程，那么你应该使用“伪类”。

```
function Car(make, color){
    this.name = "car";
    this.make = make;
    this.color = color
}
let toyotaSedan = new Car('toyota', 'red');
```

这种伪类结构允许我们在 ES6 之前使用函数构建类对象。现在，我们使用 class 关键字和一个构造函数，但是我相信它只是一个幌子，在引擎盖下工作是一样的🤷‍♂️.

我们的 PromiseQueue 类:

```
class PromiseQueue{
  constructor(promises=[], concurrent=1){
    this.concurrent = concurrent;
    this.total = promises.length;
    this.todo = promises;
    this.running = [];
    this.complete = [];
  }
​
  get runAnother(){
    return (this.running.length < this.concurrent) && this.todo.length;
  }
​
  graphTasks(){
    let {todo, running, complete} = this;
    log_(`
    todo: [${todo.map(toX)}]
    running: [${running.map(toX)}]
    complete: [${complete.map(toX)}]
    `)
  }
​
  runTasks(){
    while(this.runAnother){
      let promise = this.todo.shift();
      promise.then(()=>{
        this.complete.push(this.running.shift());
        this.graphTasks()
        this.runTasks()
      })
      this.running.push(promise);
      this.graphTasks()
    }
  }
}
​
```

我们使用构造函数来分配属性。我们需要，并发，总计，待办事项，运行和完成。由参数分配的唯一所有者是 todo、total 和 concurrent。Running 和 complete 将在我们类中的方法运行过程中更新。

承诺默认为空数组和并发

`runAnother()`如果我们在 todo 中还有剩余内容，并且运行列表的长度小于指定的并发值，则返回一个布尔结果。

使用日志更新来记录每个列表的进度、我们的待办事项、我们运行了多少以及完成了多少。

只要有任务要做，它就会继续运行，它遍历承诺列表，运行每一个承诺，并推动它们运行，然后完成，一次两个。

最后，我们创建类并调用 runTasks:

```
const queue = new PromiseQueue(promises, 2);
queue.runTasks();
```

希望你跟着做了，当你运行它的时候，你会在你的终端上看到一个很酷的小进度图，它会随着任务的完成而实时更新。

我希望你喜欢这篇文章，它甚至有一点点帮助。我是教编程的，所以请原谅我做了这么多后续解释，这些解释可能与异步编程没有太多关系。