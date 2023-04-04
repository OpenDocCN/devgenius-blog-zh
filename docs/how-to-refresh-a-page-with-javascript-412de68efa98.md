# 如何用 JavaScript 刷新页面？

> 原文：<https://blog.devgenius.io/how-to-refresh-a-page-with-javascript-412de68efa98?source=collection_archive---------3----------------------->

![](img/eb75261c8775f4f44a493b3e066494ed.png)

[乔丹·霍普金斯](https://unsplash.com/@jhopkinswriting?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

刷新页面是我们有时候不得不做的操作。

在本文中，我们将看看如何用 JavaScript 刷新页面。

# 不带参数的 location.reload()

`location.reload()`方法是一种我们可以用 JavaScript 调用来刷新页面的方法。

它的作用和浏览器中的重新加载按钮一样。

只有从与应用程序本身相同的域中发出时，才能调用此方法。

# history.go(0)

`history.go`方法让我们从会话历史中加载特定的页面。

我们可以通过传入一个正数来使用它向前移动，如果传入一个负数就可以向后移动。

如果我们传入 0，我们可以刷新页面。

# history.go()

不向`history.go`传递任何内容与传递 0 是一样的。

它还会刷新页面。

# `location.href = location.pathname`

我们可以指定`location.pathname`，它包含主机名后面的 URL 部分。

我们可以把它赋给`location.href`来触发导航到同一个页面，这和刷新页面是一样的。

`location.href`是我们可以分配来导航到给定 URL 的属性。

# 位置.替换

`location.replace`用提供的 URL 替换当前资源。

`replace`如果我们用记录触发导航，则不会将记录保存到会话历史中。

例如，我们可以写:

```
location.replace(location.pathname)
```

重新加载页面，因为`location.pathname`有当前页面的 URL 路径。

# 用`true`重新加载

我们可以调用`location.reload(true)`通过获取页面 HTML 的新的、非缓存的副本来刷新页面。

缓存中不会提供任何内容。

这与在浏览器中进行硬刷新是一样的。

对于像 Google Chrome 这样不接受参数的浏览器,`location.reload(true)`的行为与`location.reload()`相同。

# 结论

用 JavaScript 刷新页面有几种方法。