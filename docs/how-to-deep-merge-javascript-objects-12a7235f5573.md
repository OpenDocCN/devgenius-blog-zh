# 如何深度合并 JavaScript 对象？

> 原文：<https://blog.devgenius.io/how-to-deep-merge-javascript-objects-12a7235f5573?source=collection_archive---------2----------------------->

![](img/007a0463dd945b488666b3ed799bb09d.png)

照片由[克莱顿·卡迪纳利](https://unsplash.com/@clayton_cardinalli?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

`Object.assign`方法或 spread 操作符让我们浅层合并 JavaScript 对象。

这意味着只有第一层被合并，但是更深的层仍然引用原始对象。

深度合并确保我们合并到另一个对象中的对象的所有级别都被复制，而不是引用原始对象。

在本文中，我们将看看如何深度合并 JavaScript 对象。

# 递归合并

为了深度合并 JavaScript 对象，我们必须自己递归地合并每一层。

为此，我们必须编写自己的函数。

例如，我们可以写:

```
const isObject = (item) => {
  return (item && typeof item === 'object' && !Array.isArray(item));
}const mergeDeep = (target, ...sources) => {
  if (!sources.length) return target;
  const source = sources.shift();if (isObject(target) && isObject(source)) {
    for (const key in source) {
      if (isObject(source[key])) {
        if (!target[key]) Object.assign(target, {
          [key]: {}
        });
        mergeDeep(target[key], source[key]);
      } else {
        Object.assign(target, {
          [key]: source[key]
        });
      }
    }
  } return mergeDeep(target, ...sources);
}const merged = mergeDeep({
  a: 1
}, {
  b: {
    c: 123
  }
});
console.log(merged)
```

首先，我们通过检查`item`参数是否真实以及它是否是一个不是数组的对象来创建`isObject`辅助函数。

我们使用`typeof`操作符来检查对象是否是对象。

如果它是真的并且它的类型是一个对象，那么它一定是一个对象。

然后我们通过使用`Array.isArray`方法检查这个对象不是一个数组。

接下来，我们创建`mergeDeep`方法将多个对象合并到`target`对象中。

为此，我们首先检查是否有任何东西存储在`sources`中。

如果没有，那么我们返回`target`对象，因为没有第二个和随后的参数。

接下来，我们检查`target`和`source`是否是对象。

我们通过用`shift`从`source`数组中获取第一项来获取`source`。

我们用 for-in 循环遍历`source`的键。

如果属性是一个对象，那么我们用`Object.assign`将一个空对象合并到`target`对象中。

然后我们调用`mergeDeep`将`source`的对象属性内的键递归合并到对象中。

否则我们有一个原语属性值，我们调用`Object.assign`直接合并到对象中。

最后，我们在最后返回合并的对象。

接下来，我们用两个对象调用`mergeDeep`来试试。

而我们应该看到，`merged`是:

```
{
  "a": 1,
  "b": {
    "c": 123
  }
}
```

# 洛达什合并法

如果我们不想写自己的函数，那么我们可以使用 Lodash `merge`方法来合并对象。

例如，我们可以写:

```
const merged = _.merge({
  a: 1
}, {
  b: {
    c: 123
  }
});
```

对于`merged`，我们得到相同的值。

# 结论

我们可以用自己的 JavaScript 函数或 Lodash `merge`方法深度合并 JavaScript 对象。