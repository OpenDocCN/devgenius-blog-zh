# 通过代码系列学习合并(第 2 部分)

> 原文：<https://blog.devgenius.io/learn-combine-by-code-series-part-2-aa7083bad99e?source=collection_archive---------2----------------------->

![](img/bd4f109524c3e438e07d695a35a18350.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) 拍照

在[第 1 部分](https://medium.com/dev-genius/learn-combine-by-code-55e661a5256c)中，我们讨论了什么是联合收割机，以及如何理解和使用它，在这一部分，我们将继续举例说明它的用法。

## 16-比较

*   当我们需要重新发布来自另一个发布者的项目时，只有当每个新项目从以前发布的项目开始按升序排列时。

```
a is: 1 b is: 4
a is: 2 b is: 4
a is: 0 b is: 4
a is: 3 b is: 4Value is: 4
```

## 17-连接

*   当我们需要在另一个发布者的元素之前发出一个发布者的所有元素时。

```
Value is: A1
Value is: A2
Value is: B1
Value is: B2
```

## 18-包含

*   当我们需要在从上游发布者接收到指定元素时发出一个布尔值。

```
Does numbers contains A1?  true
```

## 19-包含 Where

*   当我们需要在接收到满足谓词闭包的元素时发出一个布尔值。

```
Does numbers contains A?:  true
```

## 20-计数

*   当我们需要发布从上游发布者收到的元素数量时。

```
Count of numbers publisher is: 3
```

## 21-去抖

*   当我们需要在事件之间经过指定的时间间隔后才发布元素时。

```
Time now is:  **2020-10-25 19:20:25 +0000**
value is: **AA** at time:  **2020-10-25 19:20:27 +0000**
```

## 22-延迟

*   当我们需要延迟向下游接收方交付元素和完成时。

```
Time now is:  2020-10-26 **22:43:26** +0000
value: is 1 at time:  2020-10-26 **22:43:36** +0000
value: is 2 at time:  2020-10-26 **22:43:36** +0000
value: is 3 at time:  2020-10-26 **22:43:36** +0000
Finished at time:  2020-10-26 **22:43:36** +0000
```

## 23-下降

*   当我们需要在重新发布后面的元素之前省略指定数量的元素时。

## 24- DropUntilOutput

*   当我们需要忽略来自上游发布者的元素，直到它接收到来自第二个发布者的元素。

```
value is:  A3
value is:  A4
```

## 25-下降时间

*   当我们需要忽略上游发布者的元素，直到给定的闭包返回 false。

```
value is:  B1
value is:  B2
```

## 26-过滤器

*   当我们需要重新发布所有匹配闭包的元素时。

```
value is:  A1
value is:  A2
```

## 27-第一

*   当我们需要发布一个流的第一个元素时，那么完成。

```
value is:  A1
```

## 28-第一次在哪里

*   当我们只需要发布流的第一个元素来满足谓词闭包时。

```
value is:  A1
```

## 29-平面地图

*   当我们需要将元素从上游发布者转换到新的发布者时。

```
value is:  2
value is:  4
value is:  6
```

## 30-处理事件

*   当发布者事件发生时，我们需要执行指定的闭包。

```
receiveSubscription [1, 2, 3]
receiveRequest demand unlimited
receiveOutput 1
value is:  1
receiveOutput 2
value is:  2
receiveOutput 3
value is:  3
receiveCompletion finished
```

## 31-忽略输出

*   当我们需要忽略所有上游元素，但传递一个完成状态(完成或失败)时。

```
receiveCompletion finished
```

## 32-最后一名

*   当我们只需要在流结束后发布流的最后一个元素时。

```
value is:  3
```

## 33- LastWhere

*   当我们只需要发布满足谓词闭包的流的最后一个元素时，一旦流结束。

```
value is:  3
```

## 34- MakeConnectable

*   当我们需要向另一个发布者提供显式的可连接性时。

```
value is:  B1
```

## 35-地图

*   当我们需要用提供的闭包转换来自上游发布者的所有元素时。

```
value is:  A1
value is:  A2
value is:  A3
```

## 36-合并

*   当我们需要对两个上游发布者应用合并功能时

```
value is:  1
value is:  2
value is:  3
value is:  4
value is:  5
value is:  6
```

## 37-多播

*   当我们需要使用一个主题向多个订阅者交付元素时。
*   它返回一个 ConnectablePublisher，该 Publisher 仅在调用 connect()后开始发布。

```
Stream 1 received: First
Stream 1 received: Second
Stream 1 received: Third
Stream 2 received: First
Stream 2 received: Second
Stream 2 received: Third
```

## 38-接收

*   当我们需要将元素交付给特定调度程序上的下游订户时。

```
value is:  First
value is:  Second
value is:  Third
```

## 39-删除重复项

*   当我们只需要发布与前一个元素不匹配的元素时。

```
value is:  First
value is:  Third
```

## 40-减少

*   当我们需要对所有收到的元素应用闭包，并在上游发布者完成时产生一个累积值时。

```
55 finished
```

## 41-扫描

*   当我们需要将所有以前发布的值累加到一个值中，然后将这个值与每个新发布的值合并时。

```
0 1 3 6 10 15
```

## 42 股

*   当我们需要与多个订阅者共享上游发布者的输出时。

```
Random: receive subscription: ([55, 69, 38])
Random: request unlimited
Random: receive value: (55)
Stream 1 received: 55
Random: receive value: (69)
Stream 1 received: 69
Random: receive value: (38)
Stream 1 received: 38
Random: receive finished
```

## 43-订阅

*   当我们需要在特定的调度程序上接收来自上游发布者的元素时。

```
Stream received: 1
Stream received: 2
Stream received: 3
```

## 44-开关测试

*   当我们需要从最近的事件中产生一系列事件时。

```
URL: https://example.org/get?index=5
```

## 45 节气门

*   当我们需要在您指定的时间间隔内选择性地重新发布来自上游发布者的元素时。

```
**Received Timestamp 2020-10-28 20:24:17 +0000**
receive value: (2020-10-28 20:24:18 +0000)
receive value: (2020-10-28 20:24:19 +0000)
receive value: (2020-10-28 20:24:20 +0000)
**Received Timestamp 2020-10-28 20:24:20 +0000**
receive value: (2020-10-28 20:24:21 +0000)
receive value: (2020-10-28 20:24:22 +0000)
receive value: (2020-10-28 20:24:23 +0000)
**Received Timestamp 2020-10-28 20:24:23 +0000**
```

## 46- Zip

*   当我们需要组合来自两个发布者的最新元素并向下游发出一个元组时。

```
(1, "A")
(2, "B")
```

## 47-试抓

*   当我们需要决定如何处理来自上游发布者的消息时，要么用新的发布者替换该发布者，要么抛出新的错误。

```
Received 5.
Received 4.
Received 3.
Received 2.
Received 1.
Received 0.
Completion: finished.
```

## 48-超时

*   当我们需要终止一个发布者，如果一个元素没有在你指定的超时间隔内交付。

```
completion: finished at 2020-10-28 20:34:54 +0000
```

我真的很喜欢反应式编程，也很享受学习它的过程。希望你和我一样喜欢😃🎉如果是这样，请随意鼓掌，如果你喜欢更多，请随意鼓掌😃与你的朋友分享，与人分享总是令人愉快的。

不要忘记检查该系列的其他部分🚀

*   [通过代码系列学习联合收割机(第一部分)](https://medium.com/dev-genius/learn-combine-by-code-55e661a5256c)
*   [通过代码系列学习联合收割机(第三部分)](https://deda9.medium.com/learn-combine-by-code-series-part-3-c9b857c837eb)