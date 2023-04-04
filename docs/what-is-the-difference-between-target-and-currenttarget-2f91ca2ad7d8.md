# target 和 currentTarget 有什么区别？

> 原文：<https://blog.devgenius.io/what-is-the-difference-between-target-and-currenttarget-2f91ca2ad7d8?source=collection_archive---------2----------------------->

## 为什么不能在 React 的异步处理程序中访问 currentTarget？

![](img/28a817f7ef80ae207c9b72543bfbdbab.png)

照片由 [Silvan Arnet](https://unsplash.com/@silvanarnet?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

`Event.target`和`Event.currentTarget`是 [DOM 事件接口](https://www.w3.org/TR/DOM-Level-2/events.html#Events-interface)上的属性。简单来说，它们之间的区别在于:

`Event.currentTarget`是事件附加到的元素。永远不会变。

`Event.target`是触发事件的元素。这取决于用户点击的确切位置。

让我们首先用一个模态来演示他们的用例:

这是一个简单的演示。当你点击模态内部的时候，你可以看到`currentTarget`指向覆盖图而`target`指向模态。这也印证了我上面说的。

但是当你这样做的时候，你会发现它关闭了，这不是我们所期望的。那么有两种解决方案:

*   将事件绑定到模式，以防止事件冒泡到父元素。代码是这样的:

```
modal.addEventListener('click', (event) => {
  event.stopPropagation();
});
```

*   在`overlay`附带的事件中，检查`currentTarget`和`target`是否相同。如果它们相同，就证明用户点击了覆盖区域，而不是模态内部。

```
overlay.addEventListener('click', (event) => {
  if (event.currentTarget === event.target) {
    console.log('Close the modal');
    overlay.style.display = 'none';
  }
});
```

下面是集成的代码，您可以取消对它的注释来验证:

通过上面的例子，相信你已经明白了。`target`多用于事件委托。例如，如果您有大量需要绑定事件的子元素，那么您可以在其父元素上只绑定一个事件，并在子元素触发时使用`target`来标识子元素。这提高了应用程序的性能。

React 中也使用了事件委托机制，在版本 17 之前，React 采用了事件池。这意味着所有的`[SyntheticEvent](https://reactjs.org/docs/events.html)`对象被汇集，也就是说，将被重用，并且在事件处理程序被调用后，它们的所有属性都将失效。所以如果想在异步处理程序中获取事件对象的属性，就需要使用`e.persist()`。

17 及以后版本去掉了事件池机制，不需要调用`e.persist()`就可以在异步处理程序中获取事件对象的属性。

注意`currentTarget`的值只有在处理事件时才可用。当被访问时，代码的执行被暂停，并返回`currentTarget`的值。

如果你直接打印`event`，你会发现`currentTarget`属性是`null`，或者如果你想在一个异步函数中访问它，你会得到`null`。注意，这与使用 React 与否无关。

请参见下面的示例:

*感谢阅读。如果你喜欢这样的故事，想支持我，请考虑成为* [*中会员*](https://medium.com/@islizeqiang/membership) *。每月 5 美元，你可以无限制地访问媒体内容。如果你通过* [*我的链接*](https://medium.com/@islizeqiang/membership) *报名，我会得到一点佣金。*

*你的支持对我来说非常重要——谢谢你。*