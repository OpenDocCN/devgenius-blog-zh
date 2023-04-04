# JavaScript 最佳实践—箭头函数

> 原文：<https://blog.devgenius.io/javascript-best-practices-arrow-functions-259b890df795?source=collection_archive---------25----------------------->

![](img/3c57545682322d3d35832b34d0432a29.png)

[Jason Hafso](https://unsplash.com/@jasonhafso?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些使用箭头函数的最佳实践。

# 箭头功能的放置

箭头功能的放置完全取决于偏好。

我们可以把它们放在顶层。

此外，我们可以将它们放在对象或函数中。

这完全取决于我们。

所以我们可以写:

```
const person = {
    getData: (id) => ajax(URL,{ id })
};
```

或者:

```
export default id => person.getData(id)
```

或者:

```
export const getPerson = id => person.getData(id)
```

或者:

```
const getPerson = id => People.getData(id,onData)
```

我们可以将它们放在顶层，作为导出或内部对象。

# 返回数据

我们应该确保正确地编写返回语法。,

例如，我们可以写:

```
const fn = prop => ( val => { return { [prop]: val }; } );
```

或者:

```
const fn = (x, y) => {
  return (
    x > 3 ? x : y
  );
};
```

如果我们有一个表达式，我们应该把它们放在括号里。

同样，我们应该添加`return`关键字，除非我们的函数只有一个语句，并且只返回一个短表达式。

例如，我们也可以写:

```
const cube = x => x ** 3;
```

这也是可读的。

如果我们有比这更长的箭头函数，那么我们应该添加括号、花括号和关键字`return`。

# 这个的用途

我们不应该在不需要`this`的回调中使用它们。

例如，如果我们有:

```
const squared = arr.map(a => a ** 2, this);
```

那么我们不需要`this`，因为它什么也不做。

箭头函数没有自己的`this`，所以`this`引用函数外`this`的任何值。

因此，`this`不应该在顶层，因为它将在严格模式下成为`undefined`。

所以有这样的东西:

```
a => this.bar(a);
```

在高层是行不通的。

# 不要使用 arguments 关键字

我们永远不应该使用`arguments`关键字。

它用于将所有参数传递给一个传统函数。

它不具备箭头功能。

此外，它是一个类似数组的可迭代对象，所以它没有任何数组方法。

因此，不使用:

```
function sum() {
  const numbers = [...arguments];
  return numbers.reduce((a, b) => a + b);
}
```

我们可以使用 rest 操作符来代替:

```
const sum = (...numbers) => {
  return numbers.reduce((a, b) => a + b);
}
```

`numbers`将是一个数组，所以我们可以对它调用`reduce`。

rest 操作符为我们省去打字和头痛的麻烦。

# 类别的使用

我们可以对构造函数使用`class`。

理想情况下，我们尽量少用它们，因为它们拥有内部状态，并且拥有会产生副作用的方法。

因此，只要有可能，我们就使用纯函数。

所以我们可以写:

```
const rectangle = (height, width) => {
  return {
    height: height,
    width: width
  };
}
```

而不是:

```
class Rectangle{
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```

# 不要使用删除关键字

关键字`delete`从一个对象中删除一个属性。

这意味着物体发生了变异。

因此，最好制作一个没有我们想要的属性的对象的副本。

而不是写:

```
delete foo.bar;
```

我们可以使用类似 Lodash 的东西来创建一个没有属性的新对象:

```
const _ = require('lodash/fp');

const fooWithoutBar = _.omit('bar', foo);
```

# 不要使用事件模块

如果我们想坚持函数式编程，那么我们就不应该使用节点的`events`模块。

它让我们通过发射和监听事件来制造副作用。

相反，我们可以创建纯函数来传递数据。

![](img/1753d9464deb877b6537ec475a9c3014.png)

[赫塞·科林斯](https://unsplash.com/@jtc?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

编写纯函数使我们的生活更容易，因为它们不会产生副作用。

此外，我们应该注意如何在箭头函数中返回数据。