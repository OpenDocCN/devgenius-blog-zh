# 用我们自己定制的 React 钩子去抖

> 原文：<https://blog.devgenius.io/debouncing-in-react-with-our-own-custom-react-hook-f9032bcfb3de?source=collection_archive---------13----------------------->

simple 软件开发中的去抖是一种用于节省计算资源的技术，当输入变化非常频繁时，只对最新输入进行昂贵的计算。这里只有最新输入的输出是相关的。 [**这里直接跳转到代码**](https://codesandbox.io/s/debounce-example-48dzj?fontsize=14&hidenavigation=1&theme=dark)

![](img/d3fed6dce245b940ebf8ff6a725dc3c3.png)

因此，典型且非常常见的去抖用例是在搜索页面的前端开发中。比方说，去其他星系旅行是现实。现在，你是一个崭露头角的企业家，想开发一个星际旅行计划应用程序。人们可以搜索地点(目前我们只关心这个)，也可以做其他典型的事情。你建造了一切。

你的网站获得了真正的吸引力，有成千上万的人在寻找他们的下一次星际冒险。你的后端经常崩溃。您需要扩展您的搜索服务后端，这将随着用户呈指数级增长。天气晴朗的一天，你意识到人们打字真的很快，平均每分钟约 175 个字符。当他们打出“开普勒”(某些开普勒的班加罗尔)时，你每次按键都会向后端发出请求。但是只有“kepalore”的结果与用户相关。此外，你的用户分析显示，如果用户希望看到搜索结果，那么在按键过程中会有间隙。所以，你找到了它，一个在不影响用户体验的情况下优化或最小化后端调用数量的地方。

只有当用户停止输入一段时间后，你才可以进行后端调用，在这个例子中是 300 毫秒。

您正在使用 Reactjs 作为您的前端框架，并且碰巧使用了 react 钩子。最重要的是，你有一个严格的第三方一揽子规则。你打电话给我寻求一些初步的指导。

[理解需求后]

**我:**浏览器里已经有一些东西可以用来在一定时间后触发呼叫。我们可以将后端调用函数作为参数传递给 setTimeout，并将超时设置为 300。我们可以使用下面的代码。

```
// inside the search change event (or the input onchange handler)
setTimeout(() => getResultsFromBackend(searchTerm), 300)
```

你:这太棒了，但是我发现上面的代码有问题。即使调用是在 300 毫秒后进行的，后端仍然会为每个字符调用。

**我:**是的，我现在看到问题了。让我想一想。哦，对了，setTimeout 返回 **timerId** ，如果在 300 毫秒内按下一个新字符，我们可以用它来清除旧的超时。

```
// timerId is declared in a upper scope say.
if (timerId) {
// if there is a timerId they we know there was a key press and the getResultsFromBackend is scheduled to be called in sometime but as there is a new key press we only want getResultsFromBackend to be called after 300ms with the new search term.clearTimeout(timerId);// this will clear the timer and not call getResultsFromBackend for the old value}
timerId = setTimeout(() => getResultsFromBackend(searchTerm), 300);// this sets the getResultsFromBackend to be called in 300ms with the new search term // Example Keypress:  'k'Backend is set to be called in 300ms with search value 'k'Keypress (within 300ms) : 'e'Clears the scheduled called of backend with 'k'
Schedules new call with value 'ke' in 300msKeypress (within 300ms) : 'p'Clears the scheduled called of backend with 'ke'
Schedules new call with value 'kep' in 300msUser stops for 300msSends the backend call for Kep.
```

你:这正是我们想要的。现在让我们用 react hook 实现它。

**React 钩子去抖实现**

我们开始吧

您可以通过多接受一个函数作为参数来使钩子更加通用，该函数将获取事件作为参数并返回格式化的值。这将允许您处理所有类型的输入。

随时重用和指出任何问题。