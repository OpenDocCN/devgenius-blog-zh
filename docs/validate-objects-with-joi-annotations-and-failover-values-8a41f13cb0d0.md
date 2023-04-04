# 用 Joi 验证对象—注释和故障转移值

> 原文：<https://blog.devgenius.io/validate-objects-with-joi-annotations-and-failover-values-8a41f13cb0d0?source=collection_archive---------2----------------------->

![](img/6292a90f12bb4f847bcdf2f34fc40a13.png)

[米 PHAM](https://unsplash.com/@phammi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

Joi 是一个让我们轻松验证对象结构的库。

在本文中，我们将看看如何用 Joi 验证对象。

# `any`

`any`创建匹配任何数据类型的模式:

```
const any = Joi.any();
await any.validateAsync('a');
```

## `failover`

如果模式验证通过`failover`失败，我们可以设置一个故障转移值。

## `forbidden`

`forbidden`让我们将某个键标记为禁止:

```
const schema = {
  a: Joi.any().forbidden()
};
```

现在，如果`a`在我们的对象中，验证就会失败。

## `id`

`id`方法让我们设置获取模式的模式 ID。

## `invalid`

`invalid`让我们标记无效值:

```
const schema = {
  a: Joi.any().invalid("a"),
  b: Joi.any().invalid("b", "B")
};
```

## `label`

`label`让我们覆盖错误消息中的键名:

```
const schema = {
  name: Joi.string().label("Name")
};
```

## `meta`

`meta`让我们向关键字添加元数据:

```
const schema = Joi.any().meta({ index: true });
```

## `note`

`note`添加注释作为单独的参数对其进行注释:

```
const schema = Joi.any().note('this is special', 'this is important');
```

## `optional`

`optional`将一个键标记为可选:

```
const schema = Joi.any().optional();
```

## `prefs`

`prefs`让我们设置验证选项。

## `raw`

`raw`输出原始未接触值，而不是铸造值。

所以如果我们有:

```
const rawTimestampSchema = Joi.date().timestamp().raw();
rawTimestampSchema.validate('12376834097810');
```

我们验证字符串，而不是将它转换成一个`Date`对象。

## `required`

`required`按要求制作钥匙:

```
const schema = Joi.any().required();
```

## `ruleset`

`ruleset`让我们应用多个规则选项:

```
const schema = Joi.number().ruleset.min(1).max(20).rule({ message: 'Number must be between 1 and 20' });
```

## `shared`

`shared`注册一个模式，供链接引用中该模式的后代使用。

如果我们有:

```
const schema = Joi.object({
  a: [Joi.string(), Joi.link("#x")],
  b: Joi.link("#type.a")
})
  .shared(Joi.number().id("x"))
  .id("type");
```

然后用来自`a`的规则验证`b`和`x`

## `strip`

`strip`标记验证后要从对象或数组中移除的键，以整理输出:

```
const schema = Joi.object({
  username: Joi.string(),
  password: Joi.string().strip()
});
```

如果我们根据此模式进行验证，`password`将从输出中删除。

## `tag`

`tag`方法用标签注释了键:

```
const schema = Joi.any().tag('foo', 'bar');
```

## `tailor`

`tailor`返回一个由`alter`创建的模式:

```
const schema = Joi.object({
  key: Joi.string().alter({
    get: (schema) => schema.required(),
    post: (schema) => schema.forbidden()
  })
});const getSchema = schema.tailor("get");
```

`getSchema`将是`get`属性的值。

## `unit`

`unit`标注单位:

```
const schema = Joi.number().unit('milliseconds');
```

## `valid`

`valid`标记有效值:

```
const schema = {
  a: Joi.any().valid("a")  
};
```

`'a'`是`a`的有效值。

## `validate`

`validate`使用模式验证数据:

```
const schema = Joi.object({
  a: Joi.number()
});const value = {
  a: "123"
};const result = schema.validate(value);
```

`result`应该是`true`，因为`value.a`是一个数字。

## `validateAsync`

`validateAsync`异步进行验证。

它返回一个解析为验证结果的承诺。

## `warning`

`warning`设置警告:

```
const schema = Joi.any()
    .warning('custom.x', { w: 'world' })
    .message({ 'custom.x': 'hello {#w}!' });const { value, error, warning } = schema.validate('anything');
```

`#w`在我们传入的对象中有`w`的值。

# 结论

我们可以用警告、引用和其他注释来定制我们的验证。