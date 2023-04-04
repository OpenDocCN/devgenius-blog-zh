# 有用的 JavaScript 技巧——对象和方法

> 原文：<https://blog.devgenius.io/useful-javascript-tips-objects-methods-and-urls-62306d7e8665?source=collection_archive---------22----------------------->

![](img/440158f7bc0c1d7f655ee3210c487238.png)

戴安娜·莫莱斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 动态选择一种方法

我们可以通过使用括号符号来动态地选择一个方法。

例如，我们可以写:

```
animal[isDog ? 'bark' : 'speak']()
```

而不是:

```
if (isDog) {
  animal.bark()
} else {
  animal.speak()
}
```

他们做同样的事情。

第一个使用三元运算符。

第二个我们这个`if`声明。

第一个比较短，可以考虑用。

# 调试工具

我们可以使用`console`对象来帮助我们调试。

例如，我们可以使用`console.log`来记录表达式的值。

我们可以写:

```
console.log(a)
```

记录`a`的值。

# Chrome 开发工具

控制台日志的输出最终出现在 Chrome 开发工具的控制台标签中。

# 调试器

Chrome 开发工具窗口也有一个内置的调试器。

我们可以添加断点并检查那里的变量值。

调试器位于“源代码”选项卡中。

我们可以转到一个文件并打开断点。

那么代码将在断点处暂停。

有多种类型的断点。

一个是 XHR/获取断点，当一个网络请求被发送时，它中止程序。

DOM 元素改变时触发 DOM 断点。

当事件发生时，触发事件侦听器断点。

当到达断点时，打印该范围内的所有变量。

我们可以单击断点上的+按钮来显示表达式的值。

如果我们想继续运行代码，我们可以跳过代码继续执行，直到下一行并停止。

单步执行进入正在运行的函数。

同一个窗格具有调用堆栈，其中记录了所有调用的函数。

我们可以使用:

```
node --inspect
```

在浏览器中调试 Node.js 应用程序。

# 打电话申请

`call`和`apply`可用于改变`this`的值并调用函数。

它与传统功能一起工作。

例如，给定以下函数:

```
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}
```

我们可以写:

```
greet.call({
  name: 'james'
}, 'hi');
```

我们使用了`call`函数，通过用第一个参数设置`this`的值来调用`greet`。

然后将我们想要的参数作为第二个参数传递给`greet`函数。

因此，我们看到`'hi, james’`记录在控制台中。

`apply`与此类似，只是参数是以数组的形式传入的。

例如，我们可以写:

```
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}greet.apply({
  name: 'james'
}, ['hi']);
```

我们有一个数组，而不仅仅是第二个参数。

我们得到了同样的结果。

# 计算对象中属性的数量

我们可以使用`Object.keys`来返回一个非继承的字符串属性键数组。

因此，我们可以使用`length`属性来获取对象中属性的数量。

例如，如果我们有:

```
const dog = {
  color: 'white',
  breed: 'poodle'
}
```

然后我们可以写:

```
Object.keys(dog).length
```

那么我们得到 2，因为`dog`有 2 个自己的属性。

# 按属性值对对象数组进行排序

`sort`方法可用于根据属性值对对象数组进行排序。

例如，如果我们有:

```
const list = [
  { color: 'red', size: 'm' },
  { color: 'green', size: 's' },
  { color: 'blue', size: 'xl' }
]
```

然后我们可以写:

```
list.sort((a, b) => (a.color > b.color)
```

然后，我们根据`color`属性按字母顺序对条目进行排序。

那么我们得到`list`是:

```
[
  {
    "color": "blue",
    "size": "xl"
  },
  {
    "color": "green",
    "size": "s"
  },
  {
    "color": "red",
    "size": "m"
  }
]
```

# 对 URL 进行编码

JavaScript 附带了`encodeURI()`函数来编码 URL。

例如，如果我们有:

```
encodeURI("http://example.com/ hello!/")
```

然后我们得到:

```
"http://example.com/%20hello!/"
```

已退回。

还有一个`encodeURIComponent()`函数，它对包括 URL 路径分隔符在内的所有内容进行编码。

例如，我们可以写:

```
encodeURIComponent("http://example.com/ hello!/")
```

然后我们得到:

```
"http%3A%2F%2Fexample.com%2F%20hello!%2F"
```

已退回。

![](img/e68d52839f252f42aedf5aaa5cd279a7.png)

惠特尼·赖特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用括号和表达式动态地选择一个方法。

此外，我们可以根据需要用`encodeURI`或`encodeURIComponent`编码 URL。

使用调试器进行调试也很容易。

如果我们传入一个回调函数，排序条目就很容易了。