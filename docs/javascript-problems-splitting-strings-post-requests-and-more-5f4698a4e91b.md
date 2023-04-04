# JavaScript 问题——拆分字符串、Post 请求等等

> 原文：<https://blog.devgenius.io/javascript-problems-splitting-strings-post-requests-and-more-5f4698a4e91b?source=collection_archive---------30----------------------->

![](img/3f79968beb304019d18e98828a4078aa.png)

照片由[克里斯蒂娜·特里普科维奇](https://unsplash.com/@tinamosquito?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 通过在特定字符处断开来拆分字符串

我们可以使用 string 实例的`split`方法根据我们想要的字符拆分字符串。

例如，我们可以写:

```
const input = 'foo~baz~baz';
const arr = input.split('~');
```

然后我们可以用`split`分割`input`字符串和`'~'`字符。

我们得到`[“foo”, “baz”, “baz”]`作为`arr`的值。

# 函数中可变数量的参数

我们可以用 rest 操作符让一个函数接受可变数量的参数。

例如，我们可以写:

```
const foo = (...args) => {
  console.log(args)
  //...
}
```

`...`接受传递给函数的参数，并将数组设置为`args`参数的值。

然后我们就可以用`args`为所欲为了。

`...`操作符也可以用来将数组展开成逗号分隔的参数列表。

例如，我们可以写:

```
console.log(...args);
```

然后`args`的所有条目将作为参数传递给`console.log`。

# 用字符串设置选择控件的选定值

我们可以用`find`功能选择选中的字符串。

例如，我们可以写:

```
const text = 'Bar';
$("select option")
.filter(function() {
  return $(this).text() === text;
})
.prop('selected', true);
```

我们找到文本与`text`变量中的给定文本相匹配的`option`元素。

为此，我们调用`text`方法来获取`option`标签中文本的值。

然后我们将它与`text`进行比较。

然后我们将`selected`道具设置为`true`来选择选项。

考虑到以下下拉列表，这应该可行:

```
<select>
  <option value="0">Foo</option>
  <option value="1">Bar</option>
</select>
```

# JavaScript 中的字符串插值

我们可以使用模板字符串进行字符串插值。

例如，我们可以写:

```
const age = 20;
console.log(`I am ${age}.`);
```

然后将`age`的值插入到字符串中。

# 超出最大调用堆栈大小错误

如果我们递归地调用一个函数，而没有添加一个基本用例来在某一点停止它，可能会发生“超过最大调用堆栈大小错误”。

例如，如果我们有:

```
(function foo() {
   foo();
})();
```

然后我们会得到错误，因为我们一直在没有任何条件地调用`foo`来结束递归。

为了解决这个问题，我们应该确保用一个基本用例来结束递归。

例如，我们可以写:

```
(function foo(x) {
  if (x === 0) {
    return;
  }
  foo(x - 1);
})(100);
```

现在我们确定我们停止调用`foo`是`x`是 0。

在把它传入参数之前，我们一直在减少`x`，直到它变成 0。

当`x`为 0 时，我们停止调用`foo`。

这样，我们就不会得到无限递归。

# 发送帖子数据

要用普通的 JavaScript 发出 POST 请求，我们可以使用 Fetch API 来完成。

例如，我们可以写:

```
(async () => {
  const url = "http://example.com";
  const res = await fetch(url, {
    method : "POST",
    body: new FormData(document.getElementById("inputform")),
  })
  const data = await res.json();
  console.log(data);
})()
```

我们通过将整个表单传递给`FormData`构造函数来传递表单中的输入值，从而发出 POST 请求。

我们将表单数据设置为`body`的值。

然后通过调用`json`方法解析响应。

然后我们记录响应的数据。

我们传入的表单元素类似于:

```
<form name="inputform">
  <input type="text" value="name" name="name">
  <input type="number" value="age" name="age">
</form>
```

如果我们想在请求中发送 JSON 数据，我们可以写:

```
(async () => {
  const url = "http://example.com";
  const res = await fetch(url, {
    method : "POST",
    body: JSON.stringify({
      name: document.getElementById('name').value,      
    })
  })
  const data = await res.json();
  console.log(data);
})()
```

其中 ID 为`name`的元素是一个输入元素。

![](img/3dc6e2aea1726d188a524fe3310f4177.png)

[主播李](https://unsplash.com/@anchorlee?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以使用 Fetch API 来发出 POST 请求。

如果我们得到一个超过最大调用堆栈大小的错误，那么我们可能有一些无限递归的代码。

函数可以通过 rest 操作符接受可变数量的参数。