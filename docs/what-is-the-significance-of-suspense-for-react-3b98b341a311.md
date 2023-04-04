# 悬疑对于 React 的意义是什么？

> 原文：<https://blog.devgenius.io/what-is-the-significance-of-suspense-for-react-3b98b341a311?source=collection_archive---------3----------------------->

![](img/8933e4fa3854e49902a096a7f2866351.png)

你们很多人可能在项目中没有用过**悬疑**，但是悬疑是 React 未来发展中非常重要的一部分。

本文将解释悬念对于 React 的意义。

# 反应是一个反复的过程

**React** 从 v16 到 v18 主要特性经历了三大变化:

*   v16:异步模式
*   v17:并发模式
*   v18:并发渲染

React 从 v16 到 v18 的主要特性经历了三大变化:要理解这三大变化的意义，首先需要理解 React 中一个非常容易混淆的概念——**渲染**。

ClassComponent 的 render 函数在执行时被称为 render:

```
class App extends Component {
  render() {
    // 
  }
}
```

将渲染结果渲染到页面的过程称为提交。

异步模式的目的是使呈现异步和可中断。

并发模式的目的是让提交在用户感知中是并发的。

由于并发模式包括中断更改，v18 提出了并发渲染，以降低开发人员的迁移成本。

那么“让提交在用户感知中并发”是什么意思呢？

# “并发”的含义

说到“并发”，就不得不提悬念。考虑以下代码:

```
const App = () => {
  const [count, setCount] = useState(0);
  useEffect(() => {
    setInterval(() => {
      setCount(count => count + 1);
    }, 1000);
  }, []);
  return (
    <>
      <Suspense fallback={<div>loading...</div>}>
        <Sub count={count} />
      </Suspense>
      <div>count is {count}</div>
    </>
  );
};
```

*   每秒将触发一次更新，将状态计数更新为 count => count + 1
*   在 Sub 中，启动一个异步请求，在请求返回之前，暂停包装 Sub 呈现回退

假设请求在三秒钟内返回，理想情况下，请求发起前后的页面将按如下顺序显示:

```
// Before the request is initiated within Sub
<div class=“sub”>I am sub, count is 0</div>
<div>count is 0</div>// The first second of request initiation within Sub
<div>loading...</div>
<div>count is 1</div>// Sub request initiated in the 2nd second
<div>loading...</div>
<div>count is 2</div>// Request initiated in Sub at 3 seconds
<div>loading...</div>
<div>count is 3</div>// After a successful request within Sub
<div class=“sub”>I am sub, request success, count is 4</div>
<div>count is 4</div>
```

从用户的角度来看，页面上有两个任务在“同时”执行。

1.  请求 Sub 的任务(观察第一个 div 的变化)

2.改变计数的任务(观察第二个 div 的变化)

悬疑带来的是“一个页面中多个任务并发执行”的感觉，这就是 React 中并发的意思。

事实上，在异步模式中已经支持悬念，但是上面的代码在异步模式页面中的行为如下:

```
// Before the request is initiated within Sub
<div class=“sub”>I am sub, count is 0</div>
<div>count is 0</div>// The first second of request initiation within Sub
<div>loading...</div>
<div>count is 0</div>// Sub request initiated in the 2nd second
<div>loading...</div>
<div>count is 0</div>// Request initiated in Sub at 3 seconds
<div>loading...</div>
<div>count is 0</div>

// After a successful request within Sub
<div class=“sub”>I am sub, request success, count is 4</div>
<div>count is 4</div>
```

从用户的角度来看，当执行“要请求 Sub 的任务”时，“要更改计数的任务”被冻结。

这就是为什么它被称为异步而不是并发。

# **悬念的含义**

可以看到，对于并发来说，悬念是必不可少的一部分。

悬疑可以认为是“将页面中需要并发渲染的部分进行划分”。

例如，在上面的示例中，悬念将“请求 Sub 的任务”与“更改计数的任务”分开，并在视觉上并发执行。

一旦明确了悬念的含义，你就会看到 React 接下来要做的事情就是不断扩大悬念场景(也就是将更多场景纳入并发渲染的范围)。

例如，当前可用的:

*   反应.懒惰
*   由 React 提供的 fetch 库转换的异步请求
*   使用过渡
*   useDeferredvalue

未来将添加:

*   服务器组件
*   选择性水合

# 摘要

React 从“同步”发展到“异步”，再发展到“并发”。

一旦实现了并发，下一步就是扩展可以使用并发的场景数量，悬疑的作用就是“划分页面中需要并发渲染的部分”。悬念的作用是“划分页面中需要并发渲染的部分”。

这条开发路径从 React 一开始就决定了，因为从架构上来说，React 非常依赖运行时，为了优化性能，“并发”是这种架构下的最优开发方向。