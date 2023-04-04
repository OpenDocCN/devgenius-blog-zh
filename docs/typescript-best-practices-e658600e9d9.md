# TypeScript 最佳实践

> 原文：<https://blog.devgenius.io/typescript-best-practices-e658600e9d9?source=collection_archive---------1----------------------->

![](img/d8994ba69f414eb4e8b924c4d30b7d56.png)

为了使代码易于阅读和维护，您应该遵循一些最佳实践。

在这篇文章中，我将谈论一些你应该遵循的最佳实践，以使每个人的生活更轻松。

# 将函数重载组合在一起

你可以重载函数。

这意味着一个函数可以有多个函数签名，TypeScript 可以检查这些签名。

为了让您的生活更轻松，您应该将它们分组在一起，以便于阅读:

```
function foo(a: string, b: string): string;

function foo(a: number, b: number): number;

function foo(a: any, b: any): any {
  console.log(a, b);
}
```

# 对类成员排序

您可以考虑对类成员进行排序，以使成员更容易阅读。

您可以通过访问修饰符、字母顺序等对它们进行排序。

坚持一个就好。

例如，不要写:

```
class Employee {
  private id: string;
  private empCode: number;
  private empName: string;
}
```

你写道:

```
class Employee {
  private empCode: number;
  private empName: string;
  private id: string;
}
```

按字母顺序排列。

# 不要对名称空间使用 module 关键字

如果您声明了名称空间，那么您应该使用`namespace`关键字。

而不是写:

```
module Math {
  function add(a: number, b: number): number {
    return a + b;
  }
}
```

你写道:

```
namespace Math {
  function add(a: number, b: number): number {
    return a + b;
  }
}
```

这样，您就不会将您所拥有的与 ES 模块混淆。

# 类成员的可见性声明

为了利用 TypeScript 的访问控制功能，可以添加可见性声明或类成员。

例如，您写道:

```
class Employee {
  private getSalary(): number {
    return 90000;
  }
}
```

您添加了`private`访问修饰符，这样`getSalary`只能被类中的其他方法调用。

还有一个`public`修饰符使成员对外部代码可用。

`protected`使成员对子类和当前类可用。

`public`是默认值。

您也可以对实例变量执行相同的操作:

```
class Employee {
  private empCode: number;
}
```

# 消除任何类型的使用

您可以在代码中取消使用`any`类型，以利用 TypeScript 的类型检查功能。

如果你需要比静态类型更灵活的东西，有很多方法可以定义它们。

您可以使用文本类型将值限制为某些文本。

有联合类型可以让您检查多种类型的成员。

交集类型确保变量具有两种类型的成员。

索引签名允许您检查动态属性。

有很多方法可以避免`any`。

例如，不要写:

```
let bar: any;
```

你写道:

```
let foo: string;
```

# 没有空界面

你的代码中不应该有空的接口，因为它们是无用的。

所以与其写:

```
interface I {}
```

你写道:

```
interface I {
  bar: number;
}
```

# 没有 for-in 循环

for-in 循环是遗留的 JavaScript 语法，现在有了更好的替代方法。

这很糟糕，因为您需要使用`hasOwnProperty`来检查非继承属性。

更好的选择包括`Object.keys`来获得对象的非继承键。

`Object.values`获取数值，`Object.entries`获取所有条目。

所以与其写:

```
for (const key in obj) {
  if (obj.hasOwnProperty(key)) {
    console.log(obj[key]);
  }
}
```

你写道:

```
for (const key in Oject.keys(obj)) {
  console.log(obj[key]);
}
```

# 没有进口副作用

`import`应该用于导入模块成员和使用它们。

如果它们产生副作用，那就不好了，因为很难对代码进行静态分析。

所以与其写:

```
import 'foo';
```

你写道:

```
import { bar } from 'foo';
```

一些例外可能是用 Webpack 作为模块导入 CSS。

所以你仍然可以写:

```
import 'styles.css';
```

# 没有带有文字值的变量或参数的显式类型声明

对于用数字、字符串或布尔值赋值的变量或参数，使用类型声明是多余的。

TypeScript 可以在没有显式类型的情况下检查这些。

因此，与其写:

```
const foo: number = 10;
```

你写道:

```
const foo = 10;
```

# 结论

把东西放在一起便于阅读。

可见性修饰符是一种有用的类型脚本功能。

`any`类型可以用很多东西代替。

使用`namespace`来声明名称空间代码。

今天到此为止！

感谢您的阅读，敬请关注！