# 最佳现代 JavaScript——浏览器中的模块和导入/导出关系

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-modules-in-browsers-and-import-export-relationship-d69ba6d7c96d?source=collection_archive---------4----------------------->

![](img/74e05876258e3706374f7dd955dfbbec.png)

由[万花筒](https://unsplash.com/@kaleidico?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解如何在浏览器中使用 JavaScript 模块，以及导出和导入之间的关系。

# 浏览器中的 ES6 模块

我们可以在大多数现代浏览器中使用 ES6 模块。

要使用它们，我们只需添加一个脚本标签，将`type`属性设置为`module`。

文件扩展名仍然是像楷书文件一样的`js`。

JavaScript 文件的内容通过 web 服务器传送。

浏览器中的 ES6 模块通过将语法 os 同步加载与底层异步加载相结合，方便地结合在一起。

它们没有 ES6 模块灵活。

它们只能在顶层加载，所以不能有条件。

这种限制让我们可以静态地分析模块，看看在执行之前其他模块导入了什么模块。

常规脚本不能成为模块，因为它们是同步的。

我们不能以声明的方式导入模块，因为它们是一个接一个加载的，

因此，添加了一种新类型的脚本，以便我们可以异步加载模块。

要在浏览器中导入模块，我们可以编写:

```
<script type="module">
  import _ from "https://unpkg.com/lodash-es"; console.log(_.uniq([1, 2, 2, 3]));
</script>
```

我们导入了 Lodash 的 ES6 模块版本并使用了它的方法。

`type`属性被设置为`module`，这样我们就可以在脚本中导入模块。

JavaScript 文件是模块还是脚本取决于代码的编写位置。

如果我们没有任何导出或导入，并且使用带有脚本标签的文件，而没有将`type`属性设置为`module`，那么它就是一个脚本。

否则就是一个模块。

# 进口与出口的关系

模块系统之间的导入与导出的关系不同。

在 CommonJS 中，导入是导出值的副本。

在 ES6 模块中，导入是导出的只读视图，随着导出值的更新而更新。

例如，如果我们有:

`baz.js`

```
module.exports = {
  add(x, y) {
    return x + y;
  },
  subtract(x, y) {
    return x - y;
  }
};
```

然后我们可以通过编写以下内容来导入它:

```
const { add, subtract } = require("./baz");const sum = add(1, 2);
const difference = subtract(1, 2);
```

当我们需要一个 CommonJS 模块时，我们会制作一个模块的副本。

当我们将整个模块作为一个对象导入时，这也适用。

所以如果我们有:

```
const baz = require("./baz");const sum = baz.add(1, 2);
const difference = baz.subtract(1, 2);
```

那么`baz`就是`baz.js`模块的副本。

另一方面，ES6 模块导入并对导出的值进行只读查看。

导入实时连接到导出的数据。

如果我们导入整个模块，那么整个导入的模块就像一个冻结的对象。

例如，如果我们有:

`baz.js`

```
export const add = (x, y) => {
  return x + y;
};export const subtract = (x, y) => {
  return x - y;
};
```

`foo.js`

```
import { add, subtract } from "./baz";const sum = add(1, 2);
const difference = subtract(1, 2);
```

`add`和`subtract`是只读的。

如果我们试图将一个导入的成员重新分配给某个东西，我们会得到一个错误。

如果导入整个模块，我们会得到相同的结果:

```
import * as baz from "./baz";const sum = baz.add(1, 2);
const difference = baz.subtract(1, 2);
```

我们不能给`baz`赋值。

![](img/a1a36eb8cf6929a339fd7eefb7140a2c.png)

塞尔吉奥·阿尔维斯·桑托斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

ES6 模块导入是导出的只读视图。

此外，我们可以在浏览器中使用模块。