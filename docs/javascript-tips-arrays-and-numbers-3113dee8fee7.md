# JavaScript 提示—数组和数字

> 原文：<https://blog.devgenius.io/javascript-tips-arrays-and-numbers-3113dee8fee7?source=collection_archive---------17----------------------->

![](img/48d08bc85c9a9cfe1cef9d18be1bcdf1.png)

丹尼尔·奥拉莱耶在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些在编写 JavaScript 代码时应该遵循的技巧。

# **验证给定的参数是一个数字**

我们可以用`isNaN`和`isFinite`函数检查给定的参数是否是一个数字。

例如，我们可以写:

```
function isNumber(n){
  return !isNaN(parseFloat(n)) && isFinite(n);
}
```

`isNaN`检查一个数字不是`NaN`，而`isFinite`检查一个数字是有限的。

如果`isNaN`返回`false`，`isFinite`返回`true`，那么我们知道`n`是一个数字。

我们先用`parseFloat`试着把它转换成浮点数。

# 验证给定的参数是数组

我们可以使用`Array.isArray`方法来检查一个变量是否是一个数组。

例如，我们可以写:

```
Array.isArray(obj)
```

这在任何地方都适用，包括外部框架和其他环境。

如果我们不必担心框架，我们也可以使用:

```
Object.prototype.toString.call(obj) === '[object Array]' 
```

或者:

```
arr instanceof Array
```

去做检查。

# 获取数字数组中的最大值或最小值

我们可以用`Math.max`或`Math.min`方法得到一个数的最大值或最小值。

我们可以使用 spread 操作符来传播参数。

例如，我们可以写:

```
const numbers = [5, 4, 4, 7, 2, 6, 2]; 
const maxInNumbers = Math.max(...numbers); 
const minInNumbers = Math.min(...numbers);
```

我们将`numbers`数组扩展为`Math.max`和`Math.min`方法的参数，分别从数组中获取最大值和最小值。

# 清空数组

我们可以通过将`length`属性设置为 0 来清空数组。

例如，我们可以写:

```
const myArray = [1, 2, 3];  
myArray.length = 0;
```

然后`myArray`将被清空。

# 不要使用 delete 从数组中删除项目

`delete`不应该用来从数组中移除一个元素。

所以我们不应该写这样的代码:

```
const myArray = [1, 2, 3];
delete myArray[1];
```

相反，我们使用`splice`:

```
const myArray = [1, 2, 3];
myArray.splice(1, 1);
```

移除索引为 1 的项目。

第一个参数是要删除的索引。

第二个参数是从索引开始要删除多少项。

# 使用长度截断数组

我们可以将`length`属性设置为一个大于 0 的数字来截断一个数组。

例如，我们可以写:

```
const myArray = [1, 2, 3, 4, 5, 6];
myArray.length = 2;
```

现在`myArray`的长度是 2。

# **使用逻辑“与/或”作为条件**

我们可以使用`&&`或`||`来创建条件表达式。

例如，我们可以写:

```
const foo = 10;  
(foo === 10) && doSomething();
(foo === 5) || doSomething();
```

`(foo === 10) && doSomething();`同:

```
if (foo === 10){
  doSomething();
}
```

和`(foo === 5) || doSomething();`相同的是:

```
if (foo === 5){
  doSomething();
}
```

# **使用 map()方法循环遍历数组的项**

如果我们需要将每个数组条目映射到一个新值，我们可以使用`map`方法。

例如，我们可以写:

```
const squares = [1, 2, 3].map((val) => {  
  return val ** 2;  
});
```

我们用`map`对数组中的每个数字求平方。

所以我们得到:

```
[1, 4, 9]
```

作为`squares`的值。

![](img/a8f864003896b4a5d4f266f06000034c.png)

杰罗姆·霍伊泽在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用数组和数字做很多事情。