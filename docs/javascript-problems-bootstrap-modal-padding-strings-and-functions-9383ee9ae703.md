# JavaScript 问题—引导模式、填充字符串和函数

> 原文：<https://blog.devgenius.io/javascript-problems-bootstrap-modal-padding-strings-and-functions-9383ee9ae703?source=collection_archive---------20----------------------->

![](img/4d46a12d2294b55350205aa861ed1772.png)

照片由[梅格·坎南](https://unsplash.com/@meghankannan4?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 在打开时动态设置引导模式的内容

我们可以监听用于打开模态的按钮的 click 事件。

在监听器回调中，我们可以设置我们想要的内容。

例如，我们可以写:

```
$(document).on("click", ".open-modal", function () {
  const myBookId = $(this).data('id');
  $(".modal-body #bookId").val( myBookId );
  $('#dialog').modal('show');
});
```

那么带有`open-modal`类的元素就是我们用来打开引导程序的元素，看起来像这样:

```
<a data-toggle="modal" data-id="123" class="open-modal btn btn-primary" href="#">open modal</a>
```

在回调中，我们用`data`方法获得了`data-id`属性的值。

`'id'`是不带`data-`前缀的属性名。

然后我们将值设置为我们想要的值。

然后我们用`modal`方法显示模态对话框。

# 用 JavaScript 检测退出键的按下

我们可以通过监听`keydown`事件来检测 escape 按键，方法是给它附加一个监听器。

例如，我们可以写:

```
document.onkeydown = (evt) => {
  const isEscape = (evt.key === "Escape" || evt.key === "Esc");
  //....
};
```

我们键入`key`属性的值，根据浏览器的不同，可以是`'Escape'`或`'Esc'`。

如果`isEscape`是`true`的话，我们可以做一些事情。

# 如何输出带前导零的数字

我们可以调用`padStart`在一个字符串前添加零，直到该字符串满足给定的长度。

例如，我们可以写:

```
str.padStart(5, '0')
```

然后，前导零将被添加到字符串中，直到`str`为 5 个字符长。

# 用一个选择器切换多个元素

我们可以使用 jQuery `fadeToggle`方法用给定的 select 同时切换多个元素。

例如，我们可以写:

```
$('div').fadeToggle('fast');
```

然后我们将所有 div 元素切换到淡入淡出效果。

# 将数组元素从一个数组位置移动到另一个位置

要将一个数组元素从一个数组位置移动到另一个位置，我们可以使用`splice`方法来完成。

例如，我们可以写:

```
const move = (arr, fromIndex, toIndex) => {
  const element = arr[fromIndex];
  arr.splice(fromIndex, 1);
  arr.splice(toIndex, 0, element);
}
```

我们通过使用`fromIndex`来获取元素。

然后，我们使用`splice`方法从现有索引中删除该元素。

然后我们可以在我们想要的索引处插入元素，方法是用`toIndex`调用`splice`在那里插入一个项目，0 表示我们不删除任何东西，而`element`是我们正在插入的元素。

`element`之后的项目会自动移过去。

# 遍历一个 JSON 数组

如果我们想迭代一个 JSON 数组，我们应该用`JSON.parse`将字符串解析成一个 JavaScript 数组。

然后我们可以用 for-of 循环遍历它。

例如，我们可以写:

```
for (const car of cars) {
  console.log(car.name);
}
```

# 从抛出的异常中获取堆栈跟踪

堆栈跟踪在`Error`实例的`stack`属性中。

例如，如果我们有:

```
const err = new Error();
```

然后，我们可以通过编写以下内容来获得堆栈跟踪:

```
err.stack;
```

此外，我们可以使用`console.trace`方法来获取堆栈跟踪。

我们可以把它放在任何地方来获得堆栈跟踪。

# “箭头功能”和“功能”是否等价/可互换

箭头函数和函数是不等价或不可交换的。

箭头函数是具有特殊特征的 JavaScript 函数的子集。

他们没有自己的`this`值。

它们也不绑定到`arguments`对象。

此外，它们不能用作构造函数，因为它们没有绑定到自己的值`this`。

因此，它们也不能用作实例方法。

常规函数绑定到自己的值`this`。

它们也绑定到`arguments`对象。

并且它们可以被用作构造函数。

箭头功能是:

```
const foo = () => {
  //...
}
```

而常规函数有`function`关键字:

```
function foo(){
  //...
}
```

或者:

```
function Foo(){
  //...
}
```

对于构造函数。

![](img/15933c97f0f57066e3e107ca58a2640a.png)

托马斯·博诺梅蒂在 Unsplash 上的照片

# 结论

我们可以打开 Bootstrap 模型并用它设置数据。

箭头函数和函数是不等价的。

同样，我们可以使用`splice`将一个项目从一个索引移动到另一个索引。

for-of 循环非常适合循环遍历项目。