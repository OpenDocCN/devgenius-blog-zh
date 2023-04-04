# jQuery 技巧——属性检查、分解和表单提交

> 原文：<https://blog.devgenius.io/jquery-tips-attribute-check-break-out-of-each-and-form-submission-7ff5132bbb68?source=collection_archive---------20----------------------->

![](img/55a8169289f7c1d8b9d95cf24916cf00.png)

照片由[程昕婷·德斯皮斯](https://unsplash.com/@tdespins?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

尽管年代久远，jQuery 仍然是操纵 DOM 的流行库。

在本文中，我们将了解一些使用 jQuery 的技巧。

# 检查元素上是否有属性

我们可以通过使用`attr`方法获得给定名称的属性来检查元素上是否有属性。

例如，我们可以写:

```
const attr = $(this).attr('name');if (typeof attr !== 'undefined' && attr !== false) {
  // ...
}
```

我们检查在 if 语句中`attr`是否没有被定义，以及它是否不是`false`。

普通 JavaScript 也有`hasAttribute`方法。

例如，我们可以写:

```
this.hasAttribute("name")
```

其中`this`是 DOM 元素对象。

# 用 jQuery 预加载图像

为了用 jQuery 预加载图像，我们可以创建图像元素，然后将`src`属性设置为图像的 URL。

例如，我们可以写:

```
const arrayOfImages = [
  'http://placekitten.com/200/200',
  'http://placekitten.com/201/201',
  'http://placekitten.com/199/199'
];$(arrayOfImages).forEach((a) => {
  $('<img/>')[0].src = a;  
});
```

我们动态创建一个 img 元素，并将`src`属性设置为我们想要的 URL。

同样，我们可以使用`Image`构造函数来创建图像元素。

例如，我们可以写:

```
const arrayOfImages = [
  'http://placekitten.com/200/200',
  'http://placekitten.com/201/201',
  'http://placekitten.com/199/199'
];
$(arrayOfImages).each((a) => {
  (new Image()).src = a;  
});
```

回调中的`a`参数是 URL。

# 用 PHP 实现 jQuery Ajax POST 请求

我们可以将`ajax`方法用于我们想要发出请求的 PHP 脚本。

例如，我们可以写:

```
const data = $('form').serialize();$.ajax({
  url: "test.php",
  type: "post",
  data,
  success: (response) => {
     //...
  },
  error: (jqXHR, textStatus, errorThrown) => {
     console.log(textStatus, errorThrown);
  }
});
```

我们向`test.php`发出一个 POST 请求，首先用`serialize`方法序列化输入值，这样我们就可以发送这些值。

然后，我们可以用一个对象调用`ajax`方法，该对象具有向其发出请求的 URL。

`type`是请求的类型。

`data`有数据。

`success`有请求成功处理程序。

`error`有错误处理程序，当网络出错时调用。

如果我们想在 submit 上提交一个表单，我们可以在 form 元素上调用`submit`方法。

在对`submit`的回调中，我们可以用同样的方式发出 Ajax 请求。

例如，我们可以写:

```
$("form").submit(function(event) {
  event.preventDefault();
  const data = $(this).serialize(); const ajaxRequest = $.ajax({
    url: "test.php",
    type: "post",
    data
  });

  ajaxRequest.done((response, textStatus, jqXHR) => {
    alert('Submitted successfully');
  }); ajaxRequest.fail(() => {
    alert('There is error while submit');
  });
});
```

我们用回调来调用`submit`方法。

回调代码从表单中获取值，然后发出 Ajax 请求。

首先，我们调用`event.preventDefault()`来阻止默认提交行为的数据提交。

我们通过以下方式获得表单的值:

```
const data = $(this).serialize();
```

`this`是表单元素。

接下来，我们调用 jQuery 的`ajax`方法来请求提交表单值。

`url`、`type`和`data`属性相同。

然后我们可以用`done`方法监听结果，当它成功时得到响应。

我们使用带有`responnse`参数的回调来获得响应。

`textStatus`获取状态文本。

`jqXHR`拥有 XmlHttpRequest 对象。

我们还有`fail`方法来监听任何故障。

出现网络错误时会调用`fail`回调。

# 如何跳出 jQuery 的每一个循环

我们可以通过编写以下代码在回调中返回`false`:

```
$.each(array, (index, value) => { 
  if(value === "bar") {
    return false;
  }
});
```

我们用`each`遍历一个数组。

在回调中，我们获得了带有数组索引的`index`。

`value`具有被迭代的条目的值。

我们检查如果`value`是`'bar'`，那么我们返回`false`来结束循环。

![](img/fc956442fd973c5c4a60a59ecfeabda6.png)

凯文·诺布尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以用`attr`方法检查属性值。

为了预加载图像，我们可以创建图像元素并用图像 URL 设置它们的`src`属性。

我们可以用 Ajax 通过序列化值来提交表单输入值，然后发出请求。

我们可以用`return false`打破`each`循环。