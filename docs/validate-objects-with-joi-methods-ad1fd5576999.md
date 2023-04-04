# 用 Joi 方法验证对象

> 原文：<https://blog.devgenius.io/validate-objects-with-joi-methods-ad1fd5576999?source=collection_archive---------2----------------------->

![](img/26d3064ebae3c4e50ff560ecea544896.png)

[Ben White](https://unsplash.com/@benwhitephotography?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

Joi 是一个让我们轻松验证对象结构的库。

在本文中，我们将看看如何用 Joi 验证对象。

# `isError`

我们可以用`isError`方法检查一个属性是否是一个`Error`实例:

```
Joi.isError(new Error());
```

# `isExpression`

`isExpressiuon`方法让我们检查参数是否是一个表达式:

```
const expression = Joi.x('{a}');
Joi.isExpression(expression);
```

# `isRef`

`isRef`让我们检查参数是否是对另一个模式的引用:

```
const ref = Joi.ref('a');
Joi.isRef(ref);
```

# `isSchema`

`isSchema`让我们检查参数是否是一个模式:

```
const schema = Joi.any();
Joi.isSchema(schema);
```

# `ref`

`ref`生成对另一个模式的引用。

例如，我们可以写:

```
const Joi = require("joi");
const schema = Joi.object({
  a: Joi.ref("b.c"),
  b: {
    c: Joi.any()
  }
});
```

通过引用`b.c`属性的模式来应用`Joi.any`模式。

引用是相对的，所以我们可以这样写:

```
const Joi = require("joi");
const schema = Joi.object({
  x: {
    b: {
      c: Joi.any(),
      d: Joi.ref("c")
    }
  }
});
```

引用`b`中的`c`属性。

# `types`

我们可以使用`types`方法来获得可以用来创建模式的验证器。

例如，我们可以写:

```
const Joi = require("joi");
const { object, string } = Joi.types();const schema = object.keys({
  property: string.min(10)
});
```

从`types`方法中获取`string`模式。

# `any`

`any`创建匹配任何数据类型的模式:

```
const any = Joi.any();
await any.validateAsync('a');
```

## `type`

我们可以使用`type`属性来获取模式的类型:

```
const schema = Joi.string();
schema.type === 'string';
```

## `allow`

`allow`方法让我们设置模式中允许的值:

```
const schema = {
  a: Joi.any().allow("a")
};
```

仅允许`'a'`作为属性`a`的值。

我们可以通过传入更多的参数来检查多个值。

## `artifact`

`artifact`返回验证结果:

```
const schema = {
  a: [
    Joi.number().max(10).artifact("under"),
    Joi.number().min(11).artifact("over")
  ]
};
```

## `custom`

我们可以用`custom`方法创建自己的模式:

```
const Joi = require("joi");
const method = (value, helpers) => {
  if (value === "1") {
    throw new Error("nope");
  } if (value === "2") {
    return "3";
  }
  return value;
};const schema = Joi.string().custom(method, "custom validation");
```

我们检查`value`并返回我们想要的值。

如果值无效，我们也可以抛出错误。

此外，我们可以抛出现有的错误:

```
const Joi = require("joi");
const method = (value, helpers) => {
  if (value === "1") {
    return helpers.error("any.invalid");
  } if (value === "2") {
    return "3";
  }
  return value;
};const schema = Joi.string().custom(method, "custom validation");
```

我们从`helpers`对象得到错误。

## `empty`

`empty`匹配一个空模式:

```
let schema = Joi.string().empty('');
```

这将认为空字符串是有效的。

如果我们有:

```
let schema = Joi.string().empty();
```

然后我们配`undefined`。

## `error`

`error`让我们在验证失败时抛出一个错误:

```
const schema = Joi.string().error(new Error('string required'));
```

如果验证失败，我们将传递的错误抛出到`error`方法中。

## `extract`

`extract`从另一个模式中提取一个模式:

```
const schema = Joi.object({ foo: Joi.object({ bar: Joi.number() }) });
const number = schema.extract('foo.bar');
```

这将从`foo.bar`中获取模式。

# `Conclusion`

我们可以用各种 Joi 方法验证值。