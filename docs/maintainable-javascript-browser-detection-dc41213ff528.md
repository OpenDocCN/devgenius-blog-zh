# 可维护的 JavaScript —浏览器检测

> 原文：<https://blog.devgenius.io/maintainable-javascript-browser-detection-dc41213ff528?source=collection_archive---------7----------------------->

![](img/ef356f44204f8f5be4b997e654c6c320.png)

照片由 [Y .佩扬科夫](https://unsplash.com/@peyankov?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看如何检测浏览器来了解创建可维护的 JavaScript 代码的基础。

# 浏览器检测

浏览器检测是一个棘手的话题。

没有完美的方法来检测实际的浏览器。

# 用户代理检测

检测浏览器的一种方法是用户代理检测。

客户端或服务器可以查看用户代理字符串来确定发出请求的浏览器。

浏览器的用户代理字符串可以在`navigation.userAgent`属性中找到。

所以我们可以考虑这样写:

```
if (navigator.userAgent.includes("MSIE")) {
  // ...
} else {
  // ...
}
```

我们通过使用用户代理字符串来检查 Internet Explorer。

问题是解析用户代理字符串很困难。

此外，浏览器之间互相复制以保持兼容性。

随着每个新浏览器的发布，我们需要更新用户代理检测代码。

这增加了我们的维护负担。

用户代理字符串检查应该是检查正确操作过程的最后一种方法。

如果我们检查用户代理字符串，那么我们应该只检查旧的浏览器。

这是因为新的浏览器可能有我们想要的新功能。

旧浏览器不会增加新功能。

# 特征检测

我们应该检查浏览器中是否存在我们想要的功能，而不是检查用户代理字符串。

为此，我们可以检查属性。

例如，我们可以写:

```
if (typeof document.getElementById === 'function') {
  // ...
}
```

检查`document.getElementById`是一个函数。

这样，我们就能确切地知道我们要找的东西是否存在。

它不依赖于使用哪个浏览器的知识，只依赖于哪些功能可用。

因此，很容易支持新的浏览器。

如果浏览器中没有提供我们想要的功能，我们应该提供一个逻辑后备。

如果我们想使用更新的特性，那么我们必须对不同浏览器的实现进行不同种类的检查。

例如，我们可以写:

```
function setAnimation(callback) {
  if (window.requestAnimationFrame) { 
    return requestAnimationFrame(callback);
  } else if (window.mozRequestAnimationFrame) {
    return mozRequestAnimationFrame(callback);
  } else if (window.webkitRequestAnimationFrame) { 
    return webkitRequestAnimationFrame(callback);
  } else if (window.oRequestAnimationFrame) { 
    return oRequestAnimationFrame(callback);
  } else if (window.msRequestAnimationFrame) {
    return msRequestAnimationFrame(callback);
  } else {
    return setTimeout(callback, 0);
  }
}
```

检查每个浏览器对`requestAnimation`方法的实现。

如果没有可用的变体，那么我们使用`setTimeout`方法。

# 避免特征推断

我们应该假设，如果一个特性可用，那么相关的特性也可用。

例如，我们不应该假设如果`document.getElementsByTagName()`存在，那么`document.getElementById()`也存在。

这也适用于任何其他种类的对象。

# 避免浏览器推断

此外，我们不应该根据给定的功能来推断浏览器。

所以我们不应该写这样的话:

```
if (document.all) {
  //...
} else {
  //...
}
```

要推断 is `document.all`存在，那么 IE 正在被使用。

上面的代码不等同于:

```
const isIE = navigator.userAgent.includes("MSIE");
if (isIE) {
  //...
} else {
  //...
}
```

我们不能假设如果一个特性存在，那么我们的代码就运行在给定的浏览器上。

这是因为该功能可以出现在其他浏览器中。

![](img/ac07ed07e00182e0cfbb2b15e347e5a2.png)

约翰尼·马丁内斯在 Unsplash[上的照片](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

浏览器检测是一个棘手的话题。

用户代理字符串检测不是很准确。

但是我们可以在使用这些功能之前检查它们是否存在。