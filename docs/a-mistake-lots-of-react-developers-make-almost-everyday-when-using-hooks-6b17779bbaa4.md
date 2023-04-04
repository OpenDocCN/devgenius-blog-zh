# 许多 React 开发人员在使用钩子时几乎每天都会犯的一个错误。

> 原文：<https://blog.devgenius.io/a-mistake-lots-of-react-developers-make-almost-everyday-when-using-hooks-6b17779bbaa4?source=collection_archive---------1----------------------->

![](img/6a0cdcd246d5b2e43edfd370f85fbbcc.png)

贾斯汀·克里斯恩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# **背景**

当用现代的 React(用钩子)编码时，UseEffect 可以非常强大，因为它监视组件中特定的状态/属性变量，并且每当它的一个依赖项改变时触发。

现在，这也可能是棘手和低效的，因为如果该效果改变了状态变量，则该效果所做的更改很可能会重新呈现组件。所以开发人员在使用这些钩子时需要小心，避免不必要的钩子使用。

# 问题

如果我们有一个简单的计数器应用程序，除了 count 等于 0 的情况之外，每点击两次鼠标，count 就增加 1，我们希望在单击一次鼠标时增加 1，我们决定使用一个在每次鼠标单击时触发的 useEffect。

让我们来看看下面的使用效果

```
const [count, setCount] = useState(0);
const [mouseClickCount, setMouseClickCount] = useState(0);useEffect(() => { if(mouseClickCount === 2 && count !== 0) {
     setCount(count + 1 );
 } else if(mouseClickCount === 1 && count === 0) {
     setCount(count + 1 );
 }}, **[count, mouseClickCount]**); ...
```

你看到这里的问题了吗？

你猜对了，每次这个 useEffect 改变 count，它都会再次触发自己，因为 count 在它的 dependancies 数组中。这可能会导致性能问题，并且在某些情况下，如果 mouseClickCount 在挂钩完成之前没有重置为 0，那么 if 语句中的条件可能会再次满足，并且它会增加计数的值，从而导致意外的结果。

此外，如果我们没有在这一点上发现问题，我们就会编写不必要的逻辑来避免在不应该调用 setCount 时调用它。

对于这个非常具体的例子，我们在 useEffect 中有几行代码，但在其他用例中，我们可能有更多的代码。假设我们想将 useEffect 内部的逻辑(或部分逻辑)取出，放入 useEffect 调用的独立函数(或多个函数)中:

```
const [count, setCount] = useState(0);
const [mouseClickCount, setMouseClickCount] = useState(0);const checkCountAndMouseClicks = useCallback(() => {
if(mouseClickCount === 2 && count !== 0) {
     setCount(count + 1);
 } else if(mouseClickCount === 1 && count === 0) {
     setCount(count + 1);
 }
},[]);useEffect(() => {
 checkCountAndMouseClicks();
}, **[mouseClickCount]);**...
```

您在前面的代码片段中发现什么错误了吗？我们的计数将始终等于 1！这是为什么呢？

好吧，useCallback 对函数的包装进行了内存化，包括任何变量引用，所以当内存化计数等于 0 时，每次调用 checkCountAndMouseClicks 时，count 都会是 0，即使它的值可能不是 0。

我们如何解决这个问题？

```
const [count, setCount] = useState(0);
const [mouseClickCount, setMouseClickCount] = useState(0);const checkCountAndMouseClicks = useCallback(() => {
if(mouseClickCount === 2 && count !== 0) {
     setCount(count + 1);
 } else if(mouseClickCount === 1 && count === 0) {
     setCount(count + 1);
 }
},**[count]**);useEffect(() => {
 checkCountAndMouseClicks();
}, **[mouseClickCount]);**...
```

太神奇了！现在，每当我们单击时，将触发 useEffect，当第二次单击时，它将使计数增加 1，这将导致使用最新的计数引用对 check count 和 MouseClicks 进行新的函数记忆。

# 但是我们还没有到那一步

让我们仔细看看我们为解决上述两个问题做了什么，我注意到的一件事是，当我们可以用最少的实现获得这个功能时，我们坚持使用钩子。

```
const [count, setCount] = useState(0);
const [mouseClickCount, setMouseClickCount] = useState(0);const checkCountAndMouseClicks = useCallback(() => {
if(mouseClickCount === 2 && count !== 0) {
     setCount(count + 1);
 } else if(mouseClickCount === 1 && count === 0) {
     setCount(count + 1);
 }
},**[count]**);const DomElementOnClick = (e) => {
 e.preventDefault();
 const updatedClickCount = mouseClickCount + 1;
 setMouseClickCount(updatedClickCount);
 checkCountAndMouseClicks();
}**;**...
```

通过这种设置，我们可以准确地实现我们所需要的，而不会让 React 向我们的应用捆绑包中添加一堆不必要的代码。

# **结论**

陷入这个问题是很常见的，因为开发人员往往会忘记钩子的用途。它们(在很大程度上)应该是捕捉事件发生时触发的逻辑，一旦你遵循这种思维定势，你就可以避免在不应该使用它们的时候使用它们。如果像 onClick 这样的事件侦听器正在做这项工作，我们不需要创建一个 useEffect 来包含属于事件处理程序的逻辑。