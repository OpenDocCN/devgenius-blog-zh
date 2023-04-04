# 用 Joi 验证对象

> 原文：<https://blog.devgenius.io/validate-objects-with-joi-ef4132ce2150?source=collection_archive---------1----------------------->

![](img/5cf353673baa8b2f33c94fd02ae127d8.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[泰勒·尼克斯](https://unsplash.com/@jtylernix?utm_source=medium&utm_medium=referral)拍摄

Joi 是一个让我们轻松验证对象结构的库。

在本文中，我们将看看如何用 Joi 验证对象。

# 入门指南

我们可以通过运行以下命令来安装 Joi:

```
npm i joi
```

然后我们可以通过写来使用它:

```
const Joi = require("joi");const schema = Joi.object({
  username: Joi.string().alphanum().min(3).max(30).required(),
  password: Joi.string().pattern(new RegExp("^[a-zA-Z0-9]+$"))
});const value = schema.validate({ username: "abc", password: "abc123" });
console.log(value);
```

我们通过调用 Joi 的内置方法来创建一个模式。

关键字是属性名，

确保它是一个字符串。

`alphanum`检查字母数字字符。

`min`检查最小数量的字符。

`max`检查最大字符数。

`required`使财产增值所需。

`pattern`检查给定的正则表达式模式。

一旦我们创建了一个模式，我们就调用`schema.validate`来验证属性值。

它返回一个具有`value`和`error`属性的对象。

有我们传进去的论点。

`error`发现有任何错误。

如果我们打电话:

```
const value = schema.validate({});
```

那么`error`将是一个带有消息的对象:

```
'ValidationError: "username" is required'
```

`details`属性有更多的细节。

我们可以用`validateAsync`方法异步进行验证。

例如，我们可以写:

```
const Joi = require("joi");const schema = Joi.object({
  username: Joi.string().alphanum().min(3).max(30).required(),
  password: Joi.string().pattern(new RegExp("^[a-zA-Z0-9]+$"))
});(async () => {
  try {
    const value = await schema.validateAsync({
      username: "abc",
      password: "abc123"
    });
    console.log(value);
  } catch (err) {
    console.log(err);
  }
})();
```

来做验证。

`value`有我们传入`validateAsync`的值。

并且`err`有验证错误。

# 断言

`assert`方法让我们根据模式进行验证，如果验证失败就抛出一个错误。

例如，我们可以写:

```
Joi.assert("x", Joi.number());
```

那么我们会得到:

```
"value" must be a number
```

显示错误。

第一个参数是要验证的值。

第二个参数是要验证的数据类型。

`attempt`方法验证模式并返回一个有效的对象。

如果验证失败，它将抛出一个错误。

例如，我们可以写:

```
const result = Joi.attempt("10", Joi.number());
console.log(result);
```

而`result`是 10。

# 默认

`defaults`方法返回一个新的`joi`实例，该实例将提供的修饰符函数应用于每个新模式。

例如，如果我们有:

```
const Joi = require("joi");
const custom = Joi.defaults((schema) => {
  switch (schema.type) {
    case "string":
      return schema.allow("");
    case "object":
      return schema.min(1);
    default:
      return schema;
  }
});const schema = custom.object();
```

那么`schema`就是`schema.min(1)`，因为`'object'`是`schema.type`的值。

# `in`

`in`方法返回对另一个模式的引用。

例如，我们可以写:

```
const Joi = require("joi");
const schema = Joi.object({
  a: Joi.array().items(Joi.number()),
  b: Joi.number().valid(Joi.in("a"))
});
```

然后我们检查属性`a`的模式，这是一个有数字的数组。

所以我们用`in`方法检查它是一个在`a`数组中的数字。

# 结论

我们可以用 Joi 检查对象属性和值。