# 有用的 JavaScript 技巧——闭包和数组

> 原文：<https://blog.devgenius.io/useful-javascript-tips-closures-and-arrays-43ef5d8e2a78?source=collection_archive---------23----------------------->

![](img/65bfe39158ce28d2de78ffad33a6bc47.png)

照片由[利奥·里瓦斯](https://unsplash.com/@leorivas?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 在数组上循环

JavaScript 提供了许多数组方法，我们可以用它们以简洁的方式处理数组。

## while 循环

我们可以使用`while`循环来遍历数组。

例如，我们可以写:

```
let index = 0;
const arr = [1,2,3];while (index < arr.length) {
  console.log(arr[index]);
  index++;
}
```

我们在每个迭代器上更新`index`来访问`arr`中的对象。

## 经典 for 循环

还有经典的 for 循环，用于迭代数组中的显式项。

它通过在 loo 的第一行设置初始条件、结束条件和数组索引更新语句来工作。

例如，我们写道:

```
const arr = [1,2,3];
for (let index = 0; index < arr.length; index++) {
  console.log(arr[index]);
}
```

## 为每一个

我们可以使用`forEach`方法遍历数组条目。

它需要一个回调，我们可以在每个条目上运行代码。

例如，我们可以写:

```
const arr = [1,2,3];

arr.forEach((val, index, array) => {
  console.log(index, array, val);
});
```

我们有`arr`，我们称之为`forEach`。

然后我们用`val`得到数组条目，用`index`得到条目的索引，用`array`得到数组本身。

## 地图

让我们将数组条目从一个映射到另一个。

例如，我们可以写:

```
const arr = [1,2,3];
const cube = x => x ** 3;
const cubes = arr.map(cube);
```

我们有`cube`函数，我们将它作为`map`函数的回调函数传入。

`x`有数组条目的值。

`cubes`有一个新数组，每个条目都是`arr`的立方。

## 减少

`reduce`方法让 is 将条目组合成一个值。

例如，我们可以写:

```
const arr = [1,2,3];
const sum = (total, y) => total + y;
const sum = arr.reduce(sum, 0);
```

我们有`arr`,我们通过传入`sum`函数作为其回调函数来调用`reduce`。

第一个参数是到目前为止的累计总数，第二个参数是添加到`total`中的条目。

`reduce`的第二个参数是初始值。

条目的组合是从左到右完成的。

还有一种`reduceRight`方法，从右到左组合条目。

## 过滤器

`filter`方法返回一个满足回调条件的数组。

例如，我们可以写:

```
const arr = [1,2,3];
const isEven = x => x % 2 === 0;
const evenArr = arr.filter(isEven);
```

我们有`isEven`，它是一个检查数组条目`x`是否能被 2 整除的函数。

我们在`arr`上调用`filter`，同时传递`isEven`。

那么`evenArr`应该拥有`arr`中的所有偶数条目。

## 每个

`every`方法检查数组中的每个条目是否满足给定的条件。

例如，我们可以写:

```
const arr = [1,2,3];
const under10 = x => x < 10;
const allUnder10 = arr.every(under10);
```

我们有`under10`，它检查每个条目是否都在 10 岁以下。

`x`是数组条目。

然后我们用`under10`作为回调来调用`every`。

`allUnder10`应为`true`，因为`arr`中的所有条目都小于 10。

## 一些

`some`检查数组中的一个或多个元素是否与给定条件匹配。

例如，我们可以写:

```
const arr = [1,2,3];
const under10 = x => x < 10;
const someUnder10 = arr.some(under10);
```

那么`someUnder10`应该是`true`，因为`arr`中至少有一个条目小于 10。

# 关闭

我们可以通过运行以下命令在循环中创建一个封闭的作用域:

```
for (let i = 0; i < 3; i++) {
  funcs[i] = ((value) => {
    console.log(value);
  })(i);
}
```

这样，当我们调用`funcs`中的函数时，我们得到循环的索引变量的值。

![](img/c951243fb1cd14c83b36ef1cd14f35f7.png)

照片由[阿兹甘·米耶什特里](https://unsplash.com/@azganmjeshtri?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以使用数组方法来节省遍历条目和做我们想做的事情的努力。

此外，我们应该将代码包装在 IIFEs 中，用循环的索引变量值创建一个闭包。