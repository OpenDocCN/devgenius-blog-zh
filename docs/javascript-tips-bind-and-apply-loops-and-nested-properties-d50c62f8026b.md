# JavaScript 提示—绑定和应用、循环和嵌套属性

> 原文：<https://blog.devgenius.io/javascript-tips-bind-and-apply-loops-and-nested-properties-d50c62f8026b?source=collection_archive---------18----------------------->

![](img/a00a484b114d6426a8a6004f38cb44c6.png)

照片由 [Ansgar Scheffold](https://unsplash.com/@ansgarscheffold?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# JavaScript for…in vs for

for-in 和 for 循环是不同的。

for-in 专门用于遍历对象的属性。

例如，我们可以写:

```
for (const key in obj){
  //...
}
```

`key`是对象的属性名。

for 循环可以用于任何事情。

例如，我们可以写:

```
for (let i = 0; i < a.length; i++){
  //...
}
```

我们用循环遍历数组`a`。

但是我们可以用它做任何事情。

# 的使用。使用“new”运算符应用()

我们可以将`apply`用于任何功能。

为了一起使用它们，我们用`bind`和`apply`方法一起创建了一个新的构造函数。

例如，我们可以写:

```
function newCall(Cls, ...args) {
  return new (Function.prototype.bind.apply(Cls, args));  
}
```

我们创建了`newCall`构造函数 get 类，我们想用。

然后我们得到了`args`。

`bind`返回一个新的构造函数，它用我们想要的参数`args`调用带有`Cls`构造函数的`apply`。

然后，我们可以通过编写以下代码来使用构造函数:

```
const s = newCall(Foo, a, b, c);
```

我们用参数`a`、`b`和`c`将`Foo`构造函数传递给`newCall`。

然后我们得到一个用这些参数创建的`Foo`实例。

#  的使用

类型为`text/template`的脚本标签被一些库用作模板。

例如，jQuery 可以从这个脚本标签中获取模板。

如果我们有:

```
<script id="hello" type="text/template">
  hello world
</script>
<script>
  console.log($('#hello').html());
</script>
```

然后 jQuery 获取 ID 为`hello`的脚本标签，并通过调用`html`从其中返回 HTML 内容。

所以我们在控制台日志中记录了`'hello world'`。

# 在不知道键的情况下获取 JavaScript 对象的所有属性值

我们可以通过几种方式获得 JavaScript 对象的所有属性值，而不需要知道键。

一种方法是使用 for-in 循环:

```
for(var key in obj) {
  const value = obj[key];
  //...
}
```

我们还可以使用`Object.key`方法获得所有属性键的数组。

所以我们可以写:

```
const keys = Object.keys(obj);

for (const key of keys) {
  const val = obj[key];  
}
```

我们使用 for-of 循环遍历从`Object.keys`返回的键数组。

`Object.values`返回数组中的所有值。

所以我们可以循环遍历这些值，而不需要获取键:

```
const values = Object.values(obj);for (const val of values) {
  //...
}
```

# 通过字符串路径访问嵌套的 JavaScript 对象和数组

有几种方法可以获得对象中嵌套属性的值。

一种方法是从头开始编写一个函数:

```
const resolve(path, obj = {}, separator = '.') {
  const properties = Array.isArray(path) ? path : path.split(separator);
  return properties.reduce((prev, curr) => prev && prev[curr], obj)
}
```

我们创建了`resolve`方法来获得每个级别的属性。

属性路径由分隔符分开。

然后我们遍历所有属性，用`reduce`得到嵌套属性的值。

那么我们可以这样称呼它:

```
resolve("style.width", document.body)
```

获取对象的属性。

如果有一个数组，我们写:

```
resolve("part.0.size", obj)
```

其中 0 是数组的索引。

为了让我们的生活更容易，我们可以使用洛达什`get`法。

然后我们可以写:

```
_.get(object, 'a[0].b.c');
```

通过路径获取属性。

# 对飞行前请求的响应未通过访问控制检查错误

此错误意味着在实际请求失败之前发出的选项请求。

要解决此问题，我们可以关闭 CORS。

此外，我们可以使用类似 Nginx 的代理来处理选项请求。

我们还可以通过确保 options 请求得到成功响应来适当地启用 CORS。

![](img/1cffae9a76758e9efb04b0d2c2127d38.png)

照片由[埃迪 Tsy](https://unsplash.com/@eddietsy?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

for-in 循环不同于 for 循环。

为了确保跨域请求成功，我们应该确保正确配置我们的服务器。

有一些方法可以访问对象的嵌套路径。

我们可以一起使用`bind`和`apply`。