# JavaScript 问题——不良承诺、堆栈跟踪、NPX 和 void

> 原文：<https://blog.devgenius.io/javascript-problems-bad-promises-stack-trace-npx-and-void-f888bd33b9ed?source=collection_archive---------24----------------------->

![](img/c558359109d26ea24aa54e30cc968678.png)

安妮·斯普拉特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 承诺反模式

写 promise 代码有一些不好的方法。

编写 promise 代码的一个不好的方法是在调用`resolve`的构造函数中有一个回调。

例如，我们不应该这样写:

```
new Promise((resolve) => {
  otherPromise()
  .then((result) => {
    resolve(result);
  });
})
```

我们在承诺中有一个承诺，在回调中调用`resolve`。

这很糟糕，因为如果承诺被拒绝，我们不会得到一个错误，因为我们从未在传递给`Promise`构造函数的回调中调用过`reject`。

# 在 Node.js 中打印堆栈跟踪

我们可以在带有`stack`属性的节点应用程序中打印堆栈跟踪。

例如，我们可以写:

```
const stack = new Error().stack;
console.log(stack);
```

用`console.log`获取堆栈跟踪并打印。

我们还可以使用`console.trace`方法来记录堆栈跟踪:

```
console.trace("hello")
```

然后我们得到下面有堆栈跟踪的`'hello'`。

# 将数组拆分成块

我们可以用普通的 JavaScript，通过使用一个常规的 for 循环，将一个数组分割成块。

例如，我们可以写:

```
let temp;
const chunk = 10;
for (let i = 0; i < array.length; i += chunk) {
  temp = array.slice(i, i + chunk);
}
```

我们有一个 for 循环，它通过增加`chunk`的大小而不是 1 来遍历`for` 循环。

在循环体内部，我们使用`slice`方法获取数组的块，并将其分配给`temp`。

# 将数组转换为对象

我们可以通过使用`Object.assign`方法或 spread 操作符将一个数组转换成一个对象，其中的键是索引。

例如，我们可以写:

```
const obj = Object.assign({}, ['a', 'b', 'c']);
```

或者:

```
{ ...['a', 'b', 'c'] }
```

# 制作一个不提交表单的按钮

要制作一个不提交表单的按钮，我们可以将按钮的`type`属性设置为`'button'`。

例如，我们可以写:

```
<button type="button">button</button>
```

创建一个不触发提交操作的按钮。

# 在数组的开头添加一个元素

我们可以调用`unshift`方法将一个元素添加到数组的开头。

例如，我们可以写:

```
arr.unshift(obj);
```

然后我们添加`obj`作为数组的第一个元素，现有的条目被移动以适应它。

# 将数字转换为字符串的最佳方式

要将数字转换成字符串，我们可以使用`toString`方法。

此外，我们可以用一个空字符串连接我们的数字。

例如，我们可以写:

```
const str = num.toString();
```

或者:

```
const str = '' + num;
```

# npx 和 npm 的区别

NPX 是从版本 5.2 开始与 NPM 捆绑在一起的一个新程序

它让我们不用安装就可以运行一个包。

NPM 本身不运行任何软件包。

要在 NPM 运行某些东西，我们必须向`package.json`文件添加一个脚本。

NPX 让我们通过在`npx`后面添加一个包名来运行一个包。

例如，如果我们想用`create-react-app`创建一个新的 React 项目，我们可以写:

```
npx create-react-app example-app
```

然后我们运行`create-react-app`而不先安装它。

`create-react-app`总是运行最新版本，我们想运行最新版本就不用升级了。

# “void 0”的含义

`void`是一个前缀关键字，它接受一个参数并总是返回`undefined`。

例如，以下所有返回`undefined`:

```
void 1
void 'foo'
void false
void (new Date())
```

它在我们的代码中毫无用处，但是它存在是因为`undefined`在旧版本的 JavaScript 中不是保留字。

`undefined`是全局对象的属性。

`undefined`是一个允许的变量名。

因此，我们可以做任何事情。

ES5 或更早版本阻止我们使用`undefined`作为变量。

因此，`void 0`是有用的，因为它总是返回实际的`undefined`值。

由于`void 0`比`undefined`短，所以缩小器用`void 0`代替`undefined`以节省空间。

![](img/87e95b19e8bcc2d734389d31bc528683.png)

[Lidya Nada](https://unsplash.com/@lidyanada?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

`void`始终返回`undefined`。

我们不应该退回那些里面有承诺的承诺，那些承诺只会召唤`resolves`。

NPX 是 NPM 的一部分，我们可以用它来运行最新版本的软件包。