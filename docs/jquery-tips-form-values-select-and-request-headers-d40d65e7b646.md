# jQuery 提示—表单值、选择和请求头

> 原文：<https://blog.devgenius.io/jquery-tips-form-values-select-and-request-headers-d40d65e7b646?source=collection_archive---------4----------------------->

![](img/7c0ef4af23063c38b62ab911d12e86c8.png)

照片由 [niko photos](https://unsplash.com/@niko_photos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

尽管年代久远，jQuery 仍然是操纵 DOM 的流行库。

在本文中，我们将了解一些使用 jQuery 的技巧。

# 使用 jQuery 获取表单输入字段值

我们可以用各种方法获得表单字段及其值。

一种方法是调用`submit`，然后获取所有的输入字段。

例如，我们可以写:

```
$('form').submit(() => {
  const inputs = $('form :input');
  const values = {};
  inputs.each(function() {
    values[this.name] = $(this).val();
  });
});
```

我们获取表单元素并对其调用`submit`方法，

然后我们遍历输入，并调用`each`遍历它们的值。

在`each`回调中，我们用`this.name`得到字段的表单名称，用`$(this).val()`得到输入的值。

jQuery 也有`serializeArray`方法让我们以数组的形式获取字段。

然后我们可以在它上面使用`each`来遍历每个字段。

例如，我们可以写:

```
const values = {};
$.each($('form').serializeArray(), (i, field) => {
  values[field.name] = field.value;
});
```

我们在表单元素上调用`serializeArray`来将字段转换成数组。

然后我们有一个回调函数来获取带有`field`参数的字段。

我们通过获取`name`和`value`属性来分别获取 name 和 value 属性。

同样，我们可以调用`serialize`来获取每个字段的值。

例如，我们可以写:

```
$('form').submit(function() {
  const values = $(this).serialize();
});
```

我们用`serialize`方法从表单元素中获取值。

# 用 jQuery 获取元素的渲染高度

我们可以通过各种方法用 jQuery 获得元素的渲染高度。

要做到这一点，我们可以编写下面的任何代码:

```
$('#foo').height();
$('#foo').innerHeight();
$('#foo').outerHeight();
$('#foo').outerHeight(true);
```

`height`返回元素的高度，不包括边距、填充或边框。

`innerHeight`获取有填充但没有边框或边距的 div 的高度。

`outerHeight`返回元素的高度，包括填充和边框。

`outerHeight`带参数`true`返回高度，包括边框、边距和填充。

# 使用 jQuery 获取 Select 的所有选项

要使用 jQuery 获取 select 元素的所有选项，我们可以使用带有选择器的`$`方法来获取所有选项。

例如，我们可以写:

```
$("select#id option").each(function() {
  //...
});
```

我们用这个选择器获得 ID 为`id`的`option`元素的 select 元素。

然后我们使用`each`来遍历所有的元素。

或者，我们可以使用`map`方法从选项元素中获取值:

```
$("select#id option").map(function() {
  return $(this).val();
}).get();
```

我们通过回调获得每个选项元素的 value 属性值。

`get`返回 jQuery 对象匹配的 DOM 元素。

我们还可以使用`find`来具体获取 select 元素中的 option 子元素。

例如，我们可以写:

```
$('select#id').find('option').each(function() {
   console.log($(this).val());
});
```

我们调用`find`来获取 ID 为`id`的 select 的子元素。

我们得到回调中期权的价值。

# 将 jQuery 对象转换为字符串

我们可以通过调用`html`方法将 jQuery 对象转换成字符串。

例如，我们可以写:

```
$('<div>').append($('#foo').clone()).html();
```

创建一个 div。

然后，我们将 ID 为`foo`的元素的克隆添加到其中。

最后，我们得到 ID 为`foo`的元素的 HTML 字符串表示。

此外，我们可以使用`outerHTML`属性来使我们的生活更轻松。

例如，我们可以写:

```
$('#foo').prop('outerHTML');
```

我们获取 ID 为`foo`的元素，并用`'outerHTML'`对其调用`prop`方法，以获取元素的`outerHTML`属性。

# 使用 jQuery 向请求添加请求头

我们可以用`setRequestHeader`方法给请求添加访问控制请求头。

例如，我们可以写:

```
$.ajax({
  type: "POST",
  beforeSend(request) {
    request.setRequestHeader("Authority", authorizationToken);
  },
  url: "/login",
  data: JSON.stringify(request),
  processData: false,
  success(msg) {
    alert('success');
  }
});
```

我们用`beforeSend`方法调用`ajax`在请求之前运行代码，包括添加请求头。

它带有一个`request`参数，我们可以用它来调用`setRequestHeader`。

第一个参数是键，第二个是值。

![](img/d43a9476847a91fd7aecc1226c30ee1c.png)

Johannes Plenio 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

有很多方法可以得到一个元素的高度。

我们可以用`ajax`给请求添加请求头。

我们还可以通过各种方式获得表单值和选项。