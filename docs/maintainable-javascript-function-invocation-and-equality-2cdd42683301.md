# 可维护的 JavaScript——函数调用和相等

> 原文：<https://blog.devgenius.io/maintainable-javascript-function-invocation-and-equality-2cdd42683301?source=collection_archive---------10----------------------->

![](img/1b0069b716b5776033be66ec22119263.png)

[粘土堤](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看变量和函数来了解创建可维护 JavaScript 代码的基础。

# 函数调用

我们应该用括号将直接函数调用括起来，这样我们就可以将它们与函数声明区分开来。

例如，我们应该写:

```
const doSomething = (function() {
  //...
  return {
    message: "hello"
  }
})();
```

我们用括号把函数括起来，这样我们就知道我们在调用它。

# 严格模式

ES5 引入了严格模式，它改变了 JavaScript 的执行和解析方式以减少错误。

为了启用严格模式，我们在想要启用严格模式的代码上方添加了`'use strict'`。

避免将`'use strict'`放在全局范围内是一个常见的建议。

这是因为如果我们将多个文件连接成一个文件，那么我们将在所有文件中启用严格模式。

如果我们对所有代码都使用严格模式，那么旧代码中很有可能会出现错误。

因此，在我们修复所有代码以遵循严格模式之前，我们应该部分启用严格模式。

例如，不写:

```
"use strict";function doSomething() {
  // ...
}
```

我们应该写:

```
function doSomething() {
  "use strict";
  // ...
}
```

如果我们想在多个函数上启用严格模式，我们应该写:

```
(function() {
  "use strict"; function doSomething() {
    // ...
  } function doMoreWork() {
    // ...
  }
})();
```

这很好，因为我们在函数中保持了严格的模式。

新代码应该总是打开严格模式。

它纠正了许多错误，比如不小心给内置的全局变量赋值等等。

默认情况下，模块总是有严格的模式，所以我们总是要遵循它。

# 箭头功能

箭头函数是 ES6 中引入的一种较新的函数。

这对于定义非构造函数非常有用，因为它们没有绑定到自己的`this`，也没有自己的实例方法。

另一个好处是它不绑定到`arguments`对象，所以我们不能用它来获取函数调用的参数。

例如，我们可以写:

```
const doSomething = () => {
  // ...
}
```

定义一个箭头函数。

因为他们没有绑定到自己的`this`值，所以他们非常适合回调。

# 平等

由于类型强制，与`==`或`!=`相等是很棘手的。

类型强制导致特定类型的变量被自动转换，以使操作成功。

这可能会导致一些意想不到的结果。

当我们使用`==`和`!=`时，它们会对操作数进行类型强制。

如果值没有相同的数据类型，那么将对一个或两个操作数进行类型强制。

在很多情况下，他们并没有按照我们的期望去做。

例如，我们有:

```
console.log(5 == "5");
```

和日志`true`。

并且:

```
console.log(25 == "0x19");
```

也返回`true`。

这是因为类型强制是通过`Number`函数完成的。

在进行比较之前，字符串被转换为数字。

这是避免使用`==`和`!=`进行比较的一个原因。

而是用`===`和`!==`。

![](img/b90ae51eaf4525f66f30d627cb481884.png)

照片由[ярославалексеенко](https://unsplash.com/@webaliser?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该小心函数调用。

此外，箭头函数对于回调非常有用。

并且应该用`===`和`!==`进行比较。