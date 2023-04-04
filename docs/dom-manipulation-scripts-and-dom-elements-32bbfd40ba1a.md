# DOM 操作—脚本和 DOM 元素

> 原文：<https://blog.devgenius.io/dom-manipulation-scripts-and-dom-elements-32bbfd40ba1a?source=collection_archive---------19----------------------->

![](img/6f290aeffc285d628e96725049273834.png)

[克莱德何](https://unsplash.com/@clyde_he?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究如何加载脚本，以及如何将事件侦听器附加到 DOM 元素。

# 推迟脚本下载

脚本标签可以使用`defer`属性来推迟脚本的下载。

它被推迟，直到浏览器解析了结束的`html`节点。

它将浏览器遇到脚本节点时发生的事情推迟到遇到最终的`html`时。

例如，我们可以写:

```
<script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" defer></script>
```

# 异步加载外部 JavaScript 文件

我们可以用`async`属性加载外部 JavaScript 文件。

当 DOM 已经创建了浏览器时，`async`属性将覆盖脚本元素的顺序阻塞特性。

我们告诉浏览器不要阻止其他动作，比如解析 DOM、下载资源等。

使用`async`，脚本被并行下载，并按照下载的顺序进行解析。

也是类似`defer`的布尔值。例如，我们可以写:

```
<script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" async></script>
```

# 使用动态脚本元素强制异步下载脚本

我们可以通过动态创建来创建动态脚本元素。

例如，我们可以写:

```
const script = document.createElement("script");
script.src = "https://code.jquery.com/jquery-3.4.1.slim.min.js";
document.body.appendChild(script);
```

动态加载 jQuery 脚本。

动态创建脚本元素意味着它们可能会被无序解析。

所以我们不能保证所有的依赖都是可用的。

# 使用 onload 回调异步脚本加载

我们可以将脚本加载代码放在`onload`回调函数中。

例如，我们可以通过编写以下代码来附加一个`onload`监听器:

```
const script = document.createElement("script");
script.src = "https://code.jquery.com/jquery-3.4.1.slim.min.js";
script.onload = () => {
  console.log('loaded');
}
```

那我们就知道它什么时候上膛了。

# HTML 中的脚本放置

默认情况下，脚本元素按顺序同步加载。

例如，如果我们有:

```
<script>
  console.log(document.querySelector('p').innerHTML);
</script><body>
  <p>
    foo
  </p>
</body>
```

然后我们得到“未捕获的类型错误:无法读取 null 的属性“innerHTML ”,因为主体尚未加载。

另一方面，如果我们有:

```
<body>
  <p>
    foo
  </p>
</body><script>
  console.log(document.querySelector('p').innerHTML);
</script>
```

然后我们得到`'foo'`日志，因为主体已经被加载。

因此，我们必须小心脚本标签的放置。

# 获取 DOM 中的脚本

为了获取 DOM 中的脚本标签，我们可以使用`document.scripts`属性来获取它们。

例如，如果我们有:

```
<script src="https://code.jquery.com/jquery-3.4.1.slim.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"></script>
```

然后，我们可以通过编写以下内容来获取脚本元素:

```
for (const s of document.scripts) {
  console.log(s);
}
```

`document.scripts`是一个类似数组的对象，所以我们可以在 for-of 循环中使用它。

![](img/0d4525e4ae0afa62e1db345f56dc526e.png)

照片由 [Alasdair Elmes](https://unsplash.com/@alelmes?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# DOM 事件

DOM 元素可以监听用户触发的事件。

为了监听它们，我们可以在 DOM 元素上调用`addEventListener`。

我们还可以将回调设置为事件处理程序的属性值。

例如，如果我们有:

```
<button>
  click me
</button>
```

然后，我们可以通过编写以下内容来附加一个点击监听器:

```
const button = document.querySelector('button');
button.addEventListener('click', () => {
  console.log('clicked');
}, false);
```

第一个参数是事件名称，第二个是回调，第三个是指示我们是否要使用捕获模式。

捕获模式意味着事件从父节点传播到子节点，而不是通常的子节点到父节点。

同样，我们可以设置一个事件监听器回调到`onclick`:

```
const button = document.querySelector('button');
button.onclick = () => {
  console.log('clicked');
};
```

两者的缺点是我们一次只能给它分配一个事件侦听器。

如果我们使用`function`关键字来定义我们的事件处理函数，作用域也可能会产生冲突。

如果我们使用`function`，那么点击处理程序将把这个元素作为`this`的值。

`window`处理对主体或框架集的事件。

# 结论

我们可以加载带有`defer`或`async`属性的脚本，以便在后台加载它们。

此外，我们可以将事件侦听器附加到 DOM 对象，以侦听对它们所做的操作，并相应地运行代码。