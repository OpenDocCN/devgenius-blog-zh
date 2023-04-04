# 使用 nodeJS 中的多处理优化您的 AWS Lambda 成本

> 原文：<https://blog.devgenius.io/optimize-your-aws-lambda-cost-with-multi-threading-in-nodejs-73039406db98?source=collection_archive---------1----------------------->

![](img/f719dca6bfef5cbc4b6ec820e8cdafb3.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Lambda 在无服务器世界中非常流行。大多数用例通常涉及短期运行的任务，以满足用户请求或实现某种自动化。

Lambda 最初的设计并不适合长时间运行的流程。我指的是“长时间运行的流程”，需要几分钟执行的任务，具有低到中等的 CPU/内存，并且是无状态的。我不建议在 Lambda 中运行视频转码:)

事实上，在其发布时，最大运行时间是 5mn，此后增加到了 15mn。
通常，长时间运行的任务更适合持续运行或按需运行的 ECS 任务。但是对于如上所述的长时间运行的流程，使用 Lambda 仍然有一些优势:

*   内置旋转公共 IP(对我的决定最重要的一个，稍后你会明白为什么)
*   内置的可伸缩性，以获得更大的吞吐量(对我来说第二个重要的点是，当我编写这段代码时，Fargate 还不存在)
*   无需提供互联网网关或 NAT 网关
*   无服务器功能非常易于管理和部署
*   频繁的 Lambda 调用没有冷启动

# 使用 Lambda 扫描 HTML 网页

一个很好的用例是对需要频繁扫描的一长串已知页面(比如几十万个)进行有界限的 web 抓取。假设您在扫描之前知道要扫描的页面列表，并且将它们存储在数据库中。

因为要扫描的网站不提供 webhook 来推送给定记录的更新到我们的系统。它们不支持任何搜索功能来检索给定范围内的多条记录，因此您最终需要每天或每小时一个接一个地获取每个页面。

lambda 函数必须是无状态的，但是为了跟踪你已经扫描了哪一页，你需要保存最后一页 ID 的日志(假设你的记录是按 ID 排序的)

## 预测使用和成本

发出 HTTP fetch 请求并不消耗资源，但更耗费时间。对于一个高性能的网站来说，互联网上的每个请求需要 300 毫秒到 1 秒的时间。

假设响应时间恒定为 1s，使用单线程应用程序，您将达到每秒 1 个请求的速率。

对于 Lambda billing 来说，重要的是`duration`、`execution`和`max allocated memory`
，所以让我们来计算一下:假设你需要在一个月内扫描 3 个不同网站上的 20 万个页面，平均响应时间为 1 秒，每次执行使用 360 个批次(因此平均执行时间为 360 秒):

```
Total compute duration: 
1s x 200K records x 3 websites x 30d = 9M seconds / monthsTotal execution count (assuming batches of 360):
9M / 360 = 25K / months
```

技巧一:为什么一批只有 360？在一次 Lambda 执行中要扫描的记录越多，意味着消耗的内存就越多。您需要在批量大小和内存使用之间找到一个最佳平衡点。

技巧 2:使用批处理获取一组页面，避免 Lambda 和您的 DB/API 之间过多的来回往返

Lambda 在分配的内存上调整了持续时间和执行计费。你可以使用[这个计算器](https://aws.amazon.com/lambda/pricing/#Calculator)得到一个最小 256MB 的估算
它将花费你:30 美元(使用每月免费层)否则 37 美元

但实际上你分配的内存和 CPU 在 Lambda 中几乎没有被使用过。

> 如果我们可以在相同的 lambda 内发送高达 5r/s 的数据，并在允许更高吞吐量的同时降低成本，那不是很好吗？

这可以通过同一个 Lambda 中的 nodeJS 中的分叉过程来实现。

# Lambda 中的多处理节点

注意:在我实现这个的时候，我使用的是 nodeJS 8。
(节点 10 附带了`worker_threads`，它应该使用更少的资源。你可以在这里阅读`child_process`与[的区别)](https://blog.soshace.com/advanced-node-js-a-hands-on-guide-to-event-loop-child-process-and-worker-threads-in-node-js/)

网上关于多处理节点的资源并不多。

这个链接对我的起步帮助很大:[https://dzone . com/articles/understanding-execfile-spawn-exec-and-fork-in-node](https://dzone.com/articles/understanding-execfile-spawn-exec-and-fork-in-node)

为了实现 lambda，我使用了类似的代码:

```
//lambda.js
const { fork }= require('child_process');
const maxThreads = 5;exports.handler = async (event, context, callback) => {
  try {
       for (let proc = 0; proc < Number(maxThreads); proc++) {
         const child = fork('child.js');
         child.on('message', (m) => {
           console.log('PARENT got message:', m);
         });
         // Fetch your batch of record vorm your DB
         // and pass the bacth to the child thread
         child.send({ hello: 'world', batch });
    }
    callback(null, 'Success!');
  } catch (err) {
    console.log(err);
    callback('failure!', err);
  }
};//child.js
process.on('message', (m) => {
console.log('Message received from child:', m.hello);
// could send http req, like axios.post(..., m.batch) ... etc});
process.send({ foo: 'bar' });
```

## Lambda 的测试和调整

nodeJS 中的分叉需要 CPU。Lambda 计算能力与您分配的最大内存成比例。因此，你提供的内存越多，你得到的 CPU 就越多，因此你可以更快地执行线程！

下面是一些测试分叉 5 个伪线程，打印出`Message received from child xxxx ms`，其中`xxxx` 是启动子线程并向其传递消息所花费的总时间。您可以假设当子线程打印该消息时，它已经准备好运行您逻辑或发送 HTTP 请求。

下面是最大内存为 128MB 的 lambda 输出:

```
22:25:12.042Z Time to batch main : 376 ms
22:25:12.043Z Time to fork : 1 ms
22:25:12.503Z Time to add child in DB :460 ms
22:25:12.563Z Time to fork : 60 ms
22:25:12.963Z Time to add child in DB :381 ms
22:25:13.023Z Time to fork : 41 ms
22:25:13.483Z Time to add child in DB :460 ms
22:25:13.603Z Time to fork : 101 ms
22:25:14.043Z Time to add child in DB :440 ms
22:25:14.103Z Time to fork : 40 ms
22:25:14.342Z Message received from child 1839ms
22:25:14.563Z Time to add child in DB :441 ms
22:25:16.302Z Message received from child 3320ms
22:25:16.962Z Message received from child 3460ms
22:25:17.242Z Message received from child 3180ms
22:25:17.362Z Message received from child 2799ms
END RequestId: 16037f02-976c-11e8-a001-1f7e6977215c
REPORT RequestId: 16037f02-976c-11e8-a001-1f7e6977215c
Duration: **5739.83 ms** Billed Duration: 5800 ms
Memory Size: 128 MB
Max Memory Used: 55 MB
```

**总时长:5.7 秒**

256MB 的相同测试:

```
22:28:19.180 Time to batch main : 580 ms
22:28:19.204 Time to fork : 21 ms
22:28:19.603 Time to add child in DB :399 ms
22:28:19.643 Time to fork : 39 ms
22:28:19.804 Message received from child 201 ms
22:28:20.023 Time to add child in DB :380 ms
22:28:20.063 Time to fork : 40 ms
22:28:20.443 Message received from child 419 ms
22:28:20.503 Time to add child in DB :440 ms
22:28:20.523 Time to fork : 20 ms
22:28:20.923 Time to add child in DB :381 ms
22:28:20.963 Time to fork : 39 ms
22:28:21.005 Message received from child 502 ms
22:28:21.423 Time to add child in DB :441 ms
22:28:21.563 Message received from child 640 ms
22:28:21.763 Message received from child 321 ms
END RequestId: 8552d382-976c-11e8-bac9-55b202547050
REPORT RequestId: 8552d382-976c-11e8-bac9-55b202547050
Duration: **3184.13 ms**
Billed Duration: 3200 ms
Memory Size: 256 MB
Max Memory Used: 43 MB
```

**总时长:3.1 秒**

Lambda 允许的内存越多，分叉这些进程花费的时间就越少。

如您所见，除非您将大对象传递给分叉的子对象，否则内存不会被大量使用。

对于 5 个线程来说，总持续时间仍然很长，因为每个子线程都有一个初始开始时间。但是一旦你的孩子启动了，你可以让他们一直运行，直到他们发送完一批请求。这就是开始保存工期的方法。

## 对主流程有哪些影响？

Lambda 主进程将一直挂起，直到所有子进程都完成或者执行超时。但是要注意，如果从 AWS CLI 调用它们，通常的`callback(null,'Success')`
或`callback('error',err)`不会返回消息。

# 多重处理的结果

即使内存需求稍高，持续时间和执行时间的减少也会影响您的账单，并帮助您节省一些美元。

“理想的”增益是持续时间减少 5。但是因为分叉有自己的持续时间，每个请求的响应时间可能不同，所以您可能会看到减少了 4。执行次数的提高取决于单次执行中可以处理的批处理的大小，因此一次执行中可以处理的越多，成本就越低。

在这里，我向您展示了如何使用相同的运行 Lambda 发送 5r/s(甚至更多)。一旦你达到了 lambda 上下文中所能做的极限，那么你就可以跨越更多运行 5r/s 的并发 lambda 来增加更多的总吞吐量。最后，你的总吞吐量是`(lambda throughput)x(total concurrent lambda).`记住这一点，因为目标网站可能不支持高负载，或者一旦你达到一个给定的阈值，可能会通过 IP 节流你，但由于旋转的公共 IP，你应该会受到部分影响，更少被检测到。