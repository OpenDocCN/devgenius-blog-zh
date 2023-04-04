# Node.js 应用程序最佳实践—库的使用

> 原文：<https://blog.devgenius.io/node-js-app-best-practices-use-of-libraries-4fb51d7ddf1?source=collection_archive---------30----------------------->

![](img/080437b002aa519d992a4640ecfc2045.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，节点应用程序也必须编写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些使用库的节点最佳实践。

# 文本解码器

`TextDecoder`库可作为全局变量或作为`util`库的一部分。

所以为了保持一致，我们应该使用`TextDecoder`或者从`util`库中引用它。

这意味着我们要么写:

```
TextDecoder
```

或者:

```
require("util").TextDecoder
```

贯穿我们的应用程序。

# 文本编码器

和`TextDecoder`一样，`TextEncoder`可以作为一个全局变量或者作为`util`库的一部分。

所以我们要么使用:

```
TextEncoder
```

或者:

```
require("util").TextEncoder
```

贯穿我们的应用程序。

为了一致起见，我们应该选一个。

# URL 搜索参数

`URLSearchParams`构造函数可以作为一个全局变量或者作为`url`模块的一部分。

我们可以使用:

```
URLSearchParams
```

或者:

```
require("url").URLSearchParams
```

贯穿我们的应用程序。

为了一致起见，我们应该选一个。

# 统一资源定位器

`URL`构造函数可作为全局变量或作为`url`模块的一部分。

所以在我们的应用中，我们应该使用:

```
URL
```

或者:

```
require("url").URL
```

为了一致性，我们应该坚持一个。

# 域名服务器(Domain Name Server)

使用`dns`的 promise 版本，因为它比使用常规异步版本好得多。

例如，我们不应该写:

```
const dns = require("dns");dns.lookup(hostname, (error, address, family) => {
  //...
})
```

相反，我们应该写:

```
const { promises: dns } = require("dns");const lookup = async (hostname) => {
  const { address, family } = await dns.lookup(hostname);
  //...
}
```

promise 版本要干净得多，因为我们可能会将一段或多段异步代码链接在一起。

更多的时候，它们是承诺。

# 财政司司长承诺

我们应该使用`fs`方法的 promise 版本，这样我们可以很容易地链接多段异步代码。

例如，不写:

```
const fs = require("fs")

const readData = (filePath) => {
  fs.readFile(filePath, "utf8", (error, content) => {
    //...
  })
}
```

我们应该写:

```
const { promises: fs } = require("fs")const readData = async (filePath) => {
  const content = await fs.readFile(filePath, "utf8");
  //...
}
```

这样，我们可以轻松地将多个承诺链接起来，而不是嵌套回调。

![](img/798bae0173cf01828d38aa110281187b.png)

照片由 [Omar Flores](https://unsplash.com/@omarg247?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们应该尽可能使用有前途的库版本。

如果一个库有全局版本和模块版本，我们应该选择一个并在整个应用程序中使用它。