# JavaScript 对象操作

> 原文：<https://blog.devgenius.io/javascript-object-manipulation-12c2e599822b?source=collection_archive---------1----------------------->

![](img/1fe3c93cfdd2c7a20b47d14ac580b732.png)

analyticsindiamag.com

JavaScript 总是让我感到惊讶:最近我在复习对象操作的一些基本概念，发现了一些我以前从未使用过的简单方法。然而，它们真的很方便，所以我决定对我的发现做一个简短的回顾。

简单提醒一下:JavaScript 是基于简单的基于对象的范例设计的。简单来说，都是关于对象的。与现实生活中的对象类似，JavaScript 对象是属性的集合，属性是名称(或键)和值的组合。下面是一个简单对象的示例:

```
let cat = {
name: "Mary",
age: 2,
color: "black",
mischievous: true
}
```

我们有两种方法来访问对象的属性:点符号和括号符号。

```
cat.name
// "Mary"cat['name']
// "Mary"
```

但是如果你需要从一个对象返回所有的值呢？下面是 *Object.values()* 方法。

```
let cat = {
name: "Mary",
age: 2,
color: "black",
mischievous: true
}let catValues = Object.values(cat);
console.log(catValues) //["Mary", 2, "black", true]
```

类似的概念适用于**所有按键**！这里可以使用 *Object.keys()* 方法。

```
let cat = {
name: "Mary",
age: 2,
color: "black",
mischievous: true
}let catKeys = Object.keys(cat);
console.log(catKeys) //["name", "age", "color", "mischievous"]
```

哇，真有用。但是如果你需要一个对象的键/值对数组呢？对于这个任务，可以勇敢地使用 *Object.entries()* 方法。

```
let cat = {
name: "Mary",
age: 2,
color: "black",
mischievous: true
}let catAll = Object.entries(cat);
console.log(catAll) 
//[["name", "Mary"], ["age", 2], ["color", "black"], ["mischievous", true]]
```

好吧，另一个挑战！你如何将两个物体结合在一起？您可以使用 spread 运算符-或 *Object.assign()* 方法。

```
let cat = {
name: "Mary",
age: 2,
color: "black",
mischievous: true
}let catBreed = {
  breed: "Scottish Fold"
}let catCombo = Object.assign(cat, catBreed)console.log(catCombo)//
{name: "Mary", 
age: 2, 
color: "black", 
mischievous: true, 
breed: "Scottish Fold"}
```

另一个好玩的方法是 *Object.freeze()* 。它允许你**禁止对现有属性的任何修改或者在对象**中添加新的属性和值(剧透: *const* 仍然允许你修改一个对象！).

```
let cat = {
name: "Mary",
age: 2,
color: "black",
mischievous: true
}Object.freeze(cat);
cat.color = "white";
console.log(cat.color) //"black"
```

同样，如果你想检查一个对象是否被冻结，你可以简单地使用 *Object.isFrozen()* 。它将返回一个布尔值。

如果您想防止添加新属性，但是您**仍然需要更改现有属性**的值，您可以使用 *Object.seal()* 。要判断一个对象是否密封，可以使用 *Object.isSealed()* 方法。

希望这些方法对你有用！

**来源:**

1.  [JavaScript 对象操作](https://medium.com/infancyit/javascript-object-manipulation-5d1145cf06ef)
2.  [点与括号符号](https://codeburst.io/javascript-quickie-dot-notation-vs-bracket-notation-333641c0f781)
3.  [使用对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects)