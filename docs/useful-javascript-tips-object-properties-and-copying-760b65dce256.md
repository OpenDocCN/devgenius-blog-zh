# 有用的 JavaScript 提示—对象属性和复制

> 原文：<https://blog.devgenius.io/useful-javascript-tips-object-properties-and-copying-760b65dce256?source=collection_archive---------31----------------------->

![](img/75d13f7d64c29c5f781ed57ab68347f5.png)

照片由 [Kouji 鹤](https://unsplash.com/@pafuxu?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 将 2 个对象与 Object.is()进行比较

比较两个对象的另一种方法是使用`Object.is`方法。

和`===`操作符差不多。

然而，`NaN`同它本身是一样的。

例如，我们可以写:

```
Object.is(a, b)
```

然后我们可以比较两个变量`a`和`b`。

# 获取对象的原型

我们可以用`Object.getPrototypeOf`方法得到一个物体的原型。

例如，我们可以如下获得一个对象的原型:

```
const animal = {
  name: 'james',
  age: 7
};const dog = Object.create(animal);
```

我们用`Object.create`创建了一个`dog`对象，这样`dog`将继承`animal`。

所以如果我们用它调用`Object.gerPrototypeOf`:

```
const prot = Object.getPrototypeOf(dog);
```

然后:

```
prot === animal
```

会是`true`，因为`animal`是`dog`的原型。

# 获取对象的非继承符号

`Object.getOwnPropertySymbols`返回一个对象的所有非继承符号键。

如果我们有以下对象:

```
const name = Symbol('name')
const age = Symbol('age')
const dog = {
  [name]: 'james',
  [age]: 7
}
```

那么我们可以调用`getOwnPropertySymbols`如下:

```
const syms = Object.getOwnPropertySymbols(dog);
```

那么我们得到`syms`是:

```
[Symbol(name), Symbol(age)]
```

# 获取对象的非继承字符串键

`Object.getOwnPropetyNames`方法让我们获得一个对象的字符串键数组。

返回的键不是从任何原型继承的。

例如，我们可以写:

```
const dog = {
  breed: 'poodle'
}const keys = Object.getOwnPropertyNames(dog);
```

然后我们得到`[“breed”]`作为`keys`的值。

# 获取对象的键值对

方法返回一个对象的键值对数组。

例如，我们可以写:

```
const person = { name: 'james', age: 18 }
const pairs = Object.entries(person);
```

那么`pairs`就会是:

```
[
  [
    "name",
    "james"
  ],
  [
    "age",
    18
  ]
]
```

其中，内部数组的第一项是键名，第二项是值。

# 向对象添加单个属性

我们可以在一个对象上调用`Object.defineProperty`来创建一个新的属性。

例如，我们可以写:

```
const dog = {};
Object.defineProperty(dog, 'breed', {
  value: 'poodle'
})
```

我们使用`defineProperty`方法将`breed`属性添加到`dog`中。

# 一次向一个对象添加多个属性

除了`Object.defineProperty`，还有一个`Object.defineProperties`方法可以给一个对象添加多个属性。

例如，我们可以写:

```
const dog = {};
Object.defineProperty(dog, {
  breed: {
    value: 'poodle'
  },
  name: {
    value: 'james'
  },
})
```

那么`dog`就是 `{breed: “poodle”, name: “james”}`。

这是一种一次向一个对象添加多个属性的便捷方式。

# 使用 Object.create 创建对象

`Object.create`让我们用原型创建一个对象。

例如，我们可以写:

```
const animal = {
  name: 'james',
  age: 7
};const dog = Object.create(animal);
```

那么`dog`就是以`animal`为原型的。

它将继承来自`animal`的所有属性。

所以`dog.name`就是`'james'`。

![](img/4b9db7c540e7a27421e069923f63fba4.png)

[维克托·马尔尤舍夫](https://unsplash.com/@malyushev?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 使用 Object.assign 复制和组合对象

`Object.assign`让我们将多个对象合并成一个或复制它们。

要复制一个对象，我们可以写:

```
const copy = Object.assign({}, original)
```

我们复制了一个`original`对象，并将其赋给了`copy`变量。

`{}`应该是第一个参数，这样我们就不会修改任何现有的对象并将它们复制到一个空对象中。

要组合多个对象，我们可以写:

```
const merged  = Object.assign({}, obj1, obj2)
```

我们将所有自己的字符串属性从`obj1`和`obj2`复制到第一个参数中的空对象中。

因此`merged`将拥有两者的所有属性。

# 结论

我们可以用静态的`Object`方法来比较和复制对象。

同样，我们可以用它们定义对象的属性。

同样，我们可以用`Object.assign`方法复制和合并对象。

它进行浅层复制，以便复制顶层。