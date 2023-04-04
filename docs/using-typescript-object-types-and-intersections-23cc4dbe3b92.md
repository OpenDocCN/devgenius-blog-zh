# 使用类型脚本—对象类型和交集

> 原文：<https://blog.devgenius.io/using-typescript-object-types-and-intersections-23cc4dbe3b92?source=collection_archive---------20----------------------->

![](img/14f3898b06d0c6b05bb952b5710b860e.png)

由 [Jenni Miska](https://unsplash.com/@photosbyjenni?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

TypeScript 是 JavaScript 的自然扩展，在许多项目中取代了 JavaScript。

然而，并不是每个人都知道它实际上是如何工作的。

在本文中，我们将通过创建交集类型和检查对象结构来了解如何在 TypeScript 中处理对象。

# 检查属性

这意味着我们不能使用`typeof`操作符来检查对象的类型。

相反，我们必须找到更好的替代方案。

一种方法是检查对象中是否有属性。

例如，我们可以使用`in`操作符:

```
arr.forEach(a => {
  if ("breed" in a) {
    console.log("animal");
  } else {
    console.log("person");
  }
});
```

`in`操作符检查`'breed'`属性是否在一个对象中。

它检查自己的和继承的属性。

如果在任何一个地方找到了一个属性，它返回`true`，否则返回`false`。

然而，如果一个属性同时存在于两种类型中，这对我们没有帮助。

# 类型谓词函数

我们还可以检查一个属性是否为`undefined`来检查一个对象是否属于某种类型。

例如，我们可以写:

```
arr.forEach(a => {
  if (typeof a.breed !== "undefined") {
    console.log("animal");
  } else {
    console.log("person");
  }
});
```

这类似于`in`，除了我们使用`typeof`来检查属性，而不是使用`in`操作符。

# 类型交叉点

我们可以使用类型交集来定义两个组合在一起的对象类型。

分配给对象类型的变量应该具有这两种类型的所有属性，而不是像联合类型那样具有其中的一些属性。

例如，我们可以创建一个交集类型，并通过编写以下内容为其赋值:

```
type Thing = { name: string };
type Animal = { breed: string };const animal: Thing & Animal = {
  name: "james",
  breed: "dog"
};
```

我们定义了一个`Thing`类型和一个`Animal`类型，并通过使用`&`操作符将它们组合成一个交集类型。

这意味着`animal`必须包含来自`Thing`和`Animal`的属性。

如果我们跳过其中的一个或多个，那么 TypeScript 编译器会给我们一个错误。

例如，如果我们有:

```
const animal: Thing & Animal = {
  name: "jame"
};
```

然后我们将得到“Type”name:string；“}”不能赋给类型“Thing & Animal”。类型“”中缺少属性“breed”:name:string；}'

我们可以使用交集类型向现有对象引入新的属性。

例如，如果我们有:

```
type Thing = { name: string };
type Animal = { breed: string };const thing: Thing = {
  name: "james"
};const animal: Animal = {
  breed: "cat"
};const cat: Thing & Animal = {
  ...thing,
  ...animal
};
```

那么 TypeScript 编译器不会抛出错误，因为它识别出`name`和`breed`是`Thing & Animal`类型的一部分。

只要属性名和相应的数据类型匹配，TypeScript 编译器就可以判断出它与交集类型的结构匹配。

# 合并相同类型的属性

我们也可以合并具有重叠属性的类型。

例如，如果我们有:

```
type Thing = { name: string };
type Animal = { name: string };
```

那么分配给`Thing & Animal`类型变量的任何东西都会有一个名为`name`的字符串属性。

![](img/03706292405c6912f088d42d510a54df.png)

布鲁克·拉克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 合并不同类型的属性

如果有不同类型的属性，那么这两个属性将作为交集合并在一起。

例如，如果我们有:

```
type Thing = { age: string };
type Animal = { age: number };const thing: Thing = {
  age: "1"
};const animal: Animal = {
  age: 1
};const cat: Thing & Animal = {
  ...thing,
  ...animal
};
```

然后我们会从编译器那里得到一个错误，因为`age`的类型`string & number`没有匹配的值。

因此，在我们相交的对象类型中，我们不应该有不同类型的属性。

# 结论

我们可以创建交集类型，将对象类型合并为一种类型。

那么当一个变量或参数具有交集类型时，它必须包含这两种类型的所有属性，并且属性的数据类型必须是这两种类型的数据类型的交集。

我们可以用`in`操作符或`typeof`检查属性类型，以检查每个对象的结构来确定它们的类型。