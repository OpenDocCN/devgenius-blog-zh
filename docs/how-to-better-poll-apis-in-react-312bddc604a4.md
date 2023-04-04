# 如何在 React 中更好的轮询 API？

> 原文：<https://blog.devgenius.io/how-to-better-poll-apis-in-react-312bddc604a4?source=collection_archive---------0----------------------->

## setInterval 的替代方案，是一个更好的间隔调用异步方法的解决方案

![](img/8115cbfa2c5cbed0e2c80b6063b8ab7a.png)

[阿格巴洛斯](https://unsplash.com/@agebarros?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在 web 开发中，我们可能需要不断地轮询后端 API，以获取最新的数据来更新页面。虽然 WebSocket 是一个更好的选择，但是在某些情况下轮询也可以。

那么在 React 里怎么做呢？

# 设置间隔

我们可以使用 setInterval 连续执行异步方法，这可能是最简单的解决方案。

```
const App = () => {
  const [origin, setOrigin] = useState(''); const updateState = useCallback(async () => {
    const response = await fetch('[https://httpbin.org/get'](https://httpbin.org/get'));
    const data = await response.json();
    setOrigin(data?.origin ?? '');
  }, []); useEffect(() => {
    setInterval(updateState, 3000);
  }, [updateState]); return <main>{`Your origin is: ${origin}`}</main>;
};
```

但是这种解决方案存在一些问题。首先`setInterval`不准确，原因你可以查一下我的这篇文章:

[](https://blog.bitsrc.io/how-to-get-an-accurate-setinterval-in-javascript-ca7623d1d26a) [## 如何在 JavaScript 中获得准确的倒计时

### 为什么 setInterval 不准确？

blog.bitsrc.io](https://blog.bitsrc.io/how-to-get-an-accurate-setinterval-in-javascript-ca7623d1d26a) 

第二，它的粒度不好控制，而且造成浪费。例如，对于一个响应时间很长的 API 请求，页面上最后一个响应的内容还没有更新，下一个请求会再次发送。

这并不理想。

# 设置超时+异步…等待

我们可以使用`setTimeout`和`async...await`来实现一个定制的钩子:

```
const useIntervalAsync = (fn: () => Promise<unknown>, ms: number) => {
  const timeout = useRef<number>(); const run = useCallback(async () => {
    await fn();
    timeout.current = window.setTimeout(run, ms);
  }, [fn, ms]); useEffect(() => {
    run();
    return () => {
      window.clearTimeout(timeout.current);
    };
  }, [run]);
};
```

接下来，它是这样使用的:

```
const App = () => {
  const [origin, setOrigin] = useState(''); const updateState = useCallback(async () => {
    const response = await fetch('[https://httpbin.org/get'](https://httpbin.org/get'));
    const data = await response.json();
    setOrigin(data?.origin ?? '');
  }, []); useIntervalAsync(updateState, 3000); return <main>{`Your origin is: ${origin}`}</main>;
};
```

这个解决方案使用`async...await`和`setTimeout`来确保异步任务在下一个调度任务之前完成。并在`useEffect`的清理阶段清理下一个调度任务。

但是异步任务总是很棘手。设想一种情况:如果使用这个钩子的组件在等待异步响应时被卸载，那么这个定时任务将总是在后台运行。这是一个严重的错误，我们可以通过记录挂载状态来避免它。

```
const useIntervalAsync = (fn: () => Promise<unknown>, ms: number) => {
  const timeout = useRef<number>();
  const mountedRef = useRef(false); const run = useCallback(async () => {
    await fn();
    if (mountedRef.current) {
      timeout.current = window.setTimeout(run, ms);
    }
  }, [fn, ms]); useEffect(() => {
    mountedRef.current = true;
    run(); return () => {
      mountedRef.current = false;
      window.clearTimeout(timeout.current);
    };
  }, [run]);
};
```

这个 bug 可以通过记录`mountedRef`的挂载状态来避免。很简单，没错，但其实很实用，当然你也可以把挂载状态做成自定义钩子重用。例如下面的`useMountedState`:

```
import { useCallback, useEffect, useRef } from 'react';const useMountedState = () => {
  const mountedRef = useRef(false);
  const getState = useCallback(() => mountedRef.current, []); useEffect(() => {
    mountedRef.current = true; return () => {
      mountedRef.current = false;
    };
  }, []); return getState;
};export default useMountedState;
```

# 复杂情况

在一些更复杂的情况下，我们可能需要主动更新页面信息。例如，在一些交互之后，我期望立即调用`updateState`来更新页面上的最新数据。

当然我可以直接调用`updateState`，但是下一个预定的任务可能会执行的很快，很浪费。所以我们可以给`useIntervalAsync`添加特性，让它支持 flushing。

您可以看到我们添加了`flush`方法。其内部逻辑是取消下一个调度任务，直接执行`run`方法。

但是我们加了一个`runningCount`，这是为了什么？

设想一种情况:当钩子内部以正常的逻辑间隔执行`run`函数，而异步响应正在等待解决时，外部调用`flush`期望立即执行，然后`run`会再次执行。这是因为上一个`run`还没有解决，最新的调度任务还没有创建，所以不能取消。

也就是说，此时有两个`run`函数正在被执行，虽然这并不关键，但是如果我们还是用之前的逻辑，那么这两个`run`在被解析后会创建两个定时任务。这导致了更大的浪费。

所以我们可以使用`runningCount`来记录当前的执行次数，并在`next`函数中确保只有当`runningCount`为`0`时才创建一个新的调度任务。

此外，通过 TypeScript 的泛型，我们可以很容易地包装原始函数，以适应更多的情况。一个简单的例子:

```
const App = () => {
  const [origin, setOrigin] = useState(''); const updateState = useCallback(async () => {
    const response = await fetch('[https://httpbin.org/get'](https://httpbin.org/get'));
    const data = await response.json();
    setOrigin(data?.origin ?? '');
  }, []); const update = useIntervalAsync(updateState, 3000); return (
    <main>
      <div>{`Your origin is: ${origin}`}</div>
      <button onClick={update}>update</button>
    </main>
  );
};
```

# 结论

处理异步任务总是很棘手，我们可能需要小心不要出错。希望这篇文章对你有帮助。

如果你对今天的内容有什么想法，请留下你的评论。

*感谢阅读。如果你喜欢这样的故事，想支持我，请考虑成为* [*中会员*](https://medium.com/@islizeqiang/membership) *。每月 5 美元，你可以无限制地访问媒体内容。如果你通过* [*我的链接*](https://medium.com/@islizeqiang/membership) *报名，我会得到一点佣金。*

你的支持对我来说很重要——谢谢。