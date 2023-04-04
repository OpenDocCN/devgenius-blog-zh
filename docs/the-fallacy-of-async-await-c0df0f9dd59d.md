# 使用异步/等待时的代码优化

> 原文：<https://blog.devgenius.io/the-fallacy-of-async-await-c0df0f9dd59d?source=collection_archive---------4----------------------->

Async/Await 已经被引入到 JavaScript 中，以应对称为回调地狱的问题。在 async/await 之前，有几个“中间”解决方案试图解决这个问题:

1.  使用[承诺](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
2.  使用[发电机](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)和[公司](https://www.npmjs.com/package/co)库
3.  使用 async/await 作为第二个解决方案的语法糖

当我们应用解决方案 2 或 3 时，有一个风险**是我们没有利用 node.js 的最大卖点。至少在 node.js 获得牵引力时，它是最大的卖点。也就是异步的性质和 [**无阻塞 I/O**](https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/) 。**

```
**# Inefficient**
let result1 = await fetch('/request1');
let result2 = await fetch('/request2');
doSomeCalculation(result1);
```

正如您在上面的代码中看到的，第二个请求只有在第一个请求得到响应后才会执行，然后程序等待第二个请求完成。只有在第二个请求完成后，才会调用**dosomecocalculation**函数。在这两个请求期间，cpu 处于空闲状态。

老实说，在大多数情况下这并不重要。谨记:“*过早优化是万恶之源*”(参见[01])。无论如何，为了让思维更快，我们可以这样写代码:

```
**# Efficient**
let request1 = fetch('/request1');
let request2 = fetch('/request2');let result1 = await request1;
doSomeCalculation(result1);let result2 = await request2;
```

这段代码有两个性能优势:

1.  我们同时发送两个请求，而不等待第一个请求完成。
2.  假设第一个请求完成得比第二个请求快得多，我们已经可以在等待第二个请求完成时利用 CPU 和 doSomeCalculation。在这种情况下，显示的代码优于 *Promise.all([request1，request2])* 。

最诚挚的问候，

奥利弗，正在制作[怪兽写手](https://www.monsterwriter.app/)

**【01】【https://ubiquity.acm.org/article.cfm?】[id = 1513451 #:~:text = Trying % 20 to % 20 do % 20 the % 20 optimization，best % 20 practice % 20 in % 20 software % 20 engineers](https://ubiquity.acm.org/article.cfm?id=1513451#:~:text=Trying%20to%20do%20the%20optimization,best%20practice%20among%20software%20engineers)。**