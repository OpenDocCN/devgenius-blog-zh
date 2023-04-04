# 可维护的 JavaScript——名称空间和模块

> 原文：<https://blog.devgenius.io/maintainable-javascript-namespaces-and-modules-618ac5766d6?source=collection_archive---------6----------------------->

![](img/a1c52c869e2378f3522771fafe3d397b.png)

由[万花筒](https://unsplash.com/@kaleidico?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看命名空间来了解创建可维护的 JavaScript 代码的基础。

# 名称空间

即使我们只创造了一个，也有可能污染了我们唯一的全球目标。

因此，如果我们有一个全局变量，我们通过命名来组织代码。

例如，我们可以写:

```
var Books = {};
Books.JavaScript = {};
Books.Ruby = {}
```

我们有了`Books`全局变量，并用属性将它们划分开来。

我们有`JavaScript`和`Ruby`属性来组织对象内部不同的东西。

此外，我们可以通过向对象添加属性来动态添加名称空间。

例如，我们可以写:

```
var GlobalObj = {
  namespace(ns) {
    const parts = ns.split(".");
    let object = this; for (const part of parts) {
      if (!object[part]) {
        object[part] = {};
      }
      object = object[part];
    }
    return object;
  }
};
```

我们有`namespace`函数，它获取由点分隔的字符串。

然后我们检查该属性是否存在。

如果没有，那么我们创建一个属性，并将其设置为一个空对象。

我们这样做，直到所有的字符串部分和添加。

然后我们返回`object`。

这样，我们可以通过书写来使用它:

```
GlobalObj.namespace('foo.bar.baz');
```

我们应该会看到嵌套了`bar`和`baz`的`foo`属性。

这个方法给了我们创建名称空间的自由，并假设它存在。

我们只是预先运行`namespace`方法，这样我们就可以一直使用它。

# 模块

自 ES6 以来，模块已经成为 JavaScript 语言的一部分。

例如，我们可以通过从模块中导出我们想要的成员并导入它们来使用它们。

为了定义一个模块，我们写:

`module.js`

```
export const foo = 1;
export const bar = () => {};
export const baz = {};
export default function() {}
```

用文件名`module.js`创建我们的 JavaScript 模块。

然后我们可以通过使用`import`关键字来使用它:

```
import qux, { foo, bar, baz } from "./module";console.log(qux, foo, bar, baz);
```

我们导入默认导出为`qux`。

默认导出总是在花括号之外，并且只能有一个。

命名的导出在花括号中，导入时使用它们被定义的名称。

我们可以将默认导入的名称改为任何名称。

我们可以用`as`关键字来改变命名导出的名称。

所以我们可以写:

```
import qux, { foo, bar, baz as quux } from "./module";console.log(qux, foo, bar, quux);
```

模块名可以是相对路径或绝对路径。

如果它以`./`或`../`开头，那么它是一个相对路径。

`./`表示相对于当前文件。

`../`表示向上一级目录。

没有这些意味着这是一个绝对的路径。

在`node_modules`文件夹中的模块可以只用模块名导入。

如果某些东西没有从模块中导出，那么在模块之外就不能访问它们。

![](img/fe35a570da468bc1df6aa2a052688d66.png)

[奥比·奥尼耶德](https://unsplash.com/@thenewmalcolm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

可以添加名称空间来划分我们的代码。

此外，我们可以使用模块只导出我们想要公开的内容，而将其他内容保持私有。

因此，我们也可以用模块创建更小的文件。