# 现代 JavaScript 的精华——模块设计

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-module-design-a124053f5c3e?source=collection_archive---------11----------------------->

![](img/cc8d3d7d0caee4c079aa1ac0171c9aff.png)

照片由[在](https://unsplash.com/@halacious?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 ES6 模块系统的设计。

# ES6 模块设计

ES6 模块的设计考虑了特定的属性。

其中之一是默认出口受到青睐。

模块结构也是静态的。

它支持同步和异步加载。

模块之间的循环依赖也是允许的。

默认导出尽可能方便。

此外，模块是静态的，因此可以在编译时对它们进行静态检查。

我们只需要看代码，不用运行。

因此，我们不能写这样的东西:

```
if (Math.random() < 0.5) {
  import foo from 'foo';
} else {  
  import bar from 'bar';
}
```

带 ES6 模块。但是我们可以这样写:

```
let lib;
if (Math.random() < 0.5) {
  lib = require('foo');
} else {
  lib = require('bar');
}
```

使用 CommonJS 模块。

ES6 模块迫使我们静态地导入和导出。

静态导入的好处是我们可以在捆绑时删除死代码。

我们开发的文件通常在投入生产之前被捆绑成一个大文件。

在捆绑之后，为了加载所有的模块，我们需要加载更少的文件。

压缩捆绑文件比捆绑更多文件更有效。

此外，可以在绑定期间删除未使用的导出以节省空间。

如果通过 HTTP/1 传输包，那么传输多个文件的成本很高。

但是这与 HTTP/2 无关，因为有缓存。

有了标准模块系统，就不再需要定制的捆绑包格式。

的静态结构意味着捆绑包格式不必担心有条件加载的模块。

只读导入意味着我们不必复制导出。

因为它们不会改变，所以我们必须直接引用它们。

导入是对原始文件的引用，这也意味着查找速度更快。

CommonJS 导入是整体对象，比引用大很多。

如果我们在 ES6 中导入一个库，我们知道它的内容并可以优化访问。

变量检查也可以用静态模块结构来完成。

我们知道哪些变量在哪个位置可用。

不再需要创建全局变量来共享资源，我们将只引用全局变量。

模块输出将立即被知道。

模块的局部变量也是已知的。

同样的检查也可以用其他工具来完成，比如像 ESLint 和 JSHint 这样的 linters。

# 支持同步和异步加载

ES6 模块支持同步和异步加载。

`import`和`export`关键字允许同步导入和导出。

还有一个`import`函数，让我们以异步方式动态导入模块。

这些函数返回一个承诺，让我们导入一个模块。

# 支持模块间的循环依赖

循环依赖是 ES6 模块的一个关键目标。

它们可能发生在任何地方，而且它们并不邪恶。

当我们重构代码时，这可能发生在大型系统中。

让 ES6 模块支持循环依赖使我们的生活更容易，因为我们不必担心它们。

![](img/0cb39ca654cf295cb7f296e30ab9c51b.png)

克里斯汀·塔博瑞在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

ES6 模块有多个目标。

他们的设计让我们可以静态地分析它们，并轻松地捆绑它们。

也支持循环导入。