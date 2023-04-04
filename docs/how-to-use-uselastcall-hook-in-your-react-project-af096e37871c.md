# 如何在 react 项目中使用 useLastCall 钩子？

> 原文：<https://blog.devgenius.io/how-to-use-uselastcall-hook-in-your-react-project-af096e37871c?source=collection_archive---------13----------------------->

![](img/b985391f251e9374d596c921c2b3d731.png)

是的，我们走吧，我们如何在你的项目中使用 useLastCall 钩子。首先，我们需要知道它是什么，有什么用。首先这里是知识库更好理解:[https://github.com/HubertRyanOfficial/react-lastcall-hook](https://github.com/HubertRyanOfficial/react-lastcall-hook)

# 最后一次呼叫是为了什么？

**useLastCall** 创建库是为了避免调用数据库，而不需要缓存本身，通过比较数据库中最后一次调用与执行相同功能的时间之间的时间差来确定应该多长时间调用一次。当然，对于您的 reacts 和 react 本地项目。

由于我们使用 react 自己的钩子，我们建议使用 react 16.8 以后的版本，这样就可以使用 useEffect 和 useState 钩子。

# 如何申请

其中一个例子是使确定的最后一次呼叫之间的时间差超过 1 分钟。因此，检查从执行或调用数据库开始是否已经过了 1 分钟。所以让我们看看它会是什么样子！

让我们模拟一下，我们将进行一个一流的 api 调用来获取我们的帖子，看看有多简单。😍😊

```
$ yarn add react-lastcall-hook

or

$ npm install react-lastcall-hook --save
```

```
import useLastCall from "react-lastcall-hook";

function ExampleComponent() {

  const callPosts = useLastCall(ms,lastCallTime, functionToCall);

  useEffect(() => {
    if(callPosts) {
      callPosts()
    }
  }, [callPosts])

  return (...)
}
```

记住[仓库](https://github.com/HubertRyanOfficial/react-lastcall-hook)有更多的细节

# 例子

现在我们来看一个更真实的例子。

```
...

import useLastCall from "react-lastcall-hook";

function ExampleComponent() {

  const callPosts = useLastCall(1000 * 64, new Date().valueOf, getPosts);

  useEffect(() => {
    if(callPosts) {
      callPosts()
    }
  }, [callPosts]);

  async function getPosts() {
    await fetch(...)
  }

  return (...)
}
```

## Yump，耶！！！

现在，只有从最后一次调用到新调用的时间超过 1 分钟，才会调用获取帖子的功能。在本例中，我将它放在 ms 中，以便于我们的定制。我们看到最后一次调用的时间是我们的**日期**，这意味着函数将被调用为任何 **useEffect** ，这就是为什么这个时间与为当前小时添加的时间没有区别。

存储上次调用该函数的值的一个非常好的方法是使用 Context 或 redux。我将展示一个例子，其中 **redux** 总是用时间戳中最后一次调用的值来更新 reducer。现在获取并总是保存到一个名为 **callPostsLastTime** 的 reducer 或上下文中的属性。如果你有其他改进的方法，请与我们分享。

```
...

import {useDisptach, connect} from "react-redux";

import useLastCall from "react-lastcall-hook";

function ExampleComponent({callPostLastTime = new Date().valueOf}) {
  const disptach = useDisptach();

  const callPosts = useLastCall(1000 * 64, callPostLastTime, updateCallPostLastTime);

  useEffect(() => {
    if(callPosts) {
      callPosts()
    }
  }, [callPosts]);

  function updateCallPostLastTime() {

    disptach({
      type: "UPDATE_CALLPOSTS_LASTTIME",
      payload: new Date().valueOf();
    })
    getPosts()
  }

  async function getPosts() {
    await fetch(...)
  }

  return (...)
}

const mapStateToProps = store => ({
  callPostLastTime: store.postsReducer.lastTime
});

export default connect(mapStateToProps, null)(ExampleComponent);
```

是的，现在使用来自 **react-redux** 的**used dispatch**钩子，这样我们可以在 redux 内更新我们的减速器中的值。如果你对 Redux 架构不太了解，这里有一个链接可以让你了解更多:[https://redux.js.org/introduction/getting-started](https://redux.js.org/introduction/getting-started)

# 你猜对了！👍😁

如果你想在使用上下文的架构中使用它，它也非常有用。记住这是为了我们可以存储最后一次调用函数的时间，这样我们就可以看到开始执行的时间差。

[储存库](https://github.com/HubertRyanOfficial/react-lastcall-hook)

[GitHub](https://github.com/HubertRyanOfficial)

**塔普丁:**@休伯特里安

[推特](https://twitter.com/hubertryanoff)

[Instagram](https://www.instagram.com/hubertryanofficial)

非常感谢，希望对伟大的 React 社区有所帮助。❤🙌

![](img/5449d80e7221e0f8479e9ed9396abdb1.png)

休伯特·瑞恩标志