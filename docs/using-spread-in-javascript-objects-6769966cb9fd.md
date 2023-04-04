# 在 JavaScript 对象中使用跨页

> 原文：<https://blog.devgenius.io/using-spread-in-javascript-objects-6769966cb9fd?source=collection_archive---------10----------------------->

了解如何在对象中使用 ES6 扩展运算符

![](img/cc0615556b5bac0827aac6530b7a9827.png)

我最近发表了两篇文章，介绍如何在[数组](https://javascript.plainenglish.io/using-spread-in-arrays-bcfea95162c5)和[函数](https://javascript.plainenglish.io/how-to-use-spread-in-javascript-functions-5111cae3648f)中使用 JavaScript spread 操作符。本文将介绍如何在 JavaScript 对象中使用 spread 操作符。

## 在对象中引入扩散

当我们在一个对象中使用 spread 操作符时，我们可以将属性从一个对象复制到另一个对象中，并且可以将不同的对象组合成一个目标对象。我们还可以进一步扩展目标对象，使其包含复制的对象和其他属性。让我们看一个例子。

```
const alien = {
  species: "Alien",
  arms: 5
};const ufo = {
  ...alien,
  legs: 8
};console.log(ufo);//Returns ---> {species: 'Alien', arms: 5, legs: 8}
```

在上面的例子中，我们创建了一个对象文字，分配给一个名为*外星人*的变量。这个对象包含一个*物种*属性和一个*武器*属性。然后我们创建另一个对象文字，这次叫做 *ufo* 。在 *ufo* 对象中，我们使用 spread 操作符复制*外星人*对象。我们还添加了一个带有键*腿*的附加属性。当我们运行这段代码时，我们得到一个对象，它具有来自*外星人*对象的所有属性，以及额外的*腿*属性。

## 冲突的属性

当您对对象使用 spread 运算符时，有几件事情需要记住。当您扩散的对象和父对象具有冲突的属性时，值得记住的是该属性将被覆盖。属性的最终值将是程序运行时读取的最后一个值。让我们看看这个。

```
const alien = {
  species: "Alien",
  arms: 5,
  feet: 9
};const ufo = {
  ...alien,
  legs: 8,
  feet: 6
};console.log(ufo);//Returns ---> {species: 'Alien', arms: 5, feet: 6, legs: 8}
```

在上面的例子中，我们给两个对象都添加了一个*英尺*属性。在*外星人*的物体*的脚上*有 9 的数值。在*不明飞行物*中物体*的脚*的值为 6。当代码运行时，返回的对象具有值为 6 的属性*foots*。这是因为代码运行时 *ufo* 是最后一个设置*脚*属性的对象。

## 复印副本

当您使用扩展操作符和对象时，可以复制复制的对象。让我们看一个例子。

```
const alien = {
  species: "Alien",
  arms: 5,
  feet: 9
};const ufo = {
  ...alien,
  legs: 8,
  feet: 6
};const copiedUfo = {
  ...ufo
}console.log(copiedUfo);//Returns ---> {species: 'Alien', arms: 5, feet: 6, legs: 8}
```

*copiedUfo* 对象简单地返回了一个 *ufo* 对象的副本，所以当我们使用控制台日志为 *copiedUfo* 对象返回相同的属性。不过这里值得注意的是，带有对象的 spread 操作符只对对象进行浅层复制(复制对象的地址)。这两个对象不会严格相等，因为它们是通过引用而不是通过值来存储的。

```
const alien = {
  species: "Alien",
  arms: 5,
  feet: 9
};const ufo = {
  ...alien,
  legs: 8,
  feet: 6
};const copiedUfo = {
  ...ufo
}console.log(ufo === copiedUfo);
//Returns ---> false
```

因此，不会复制对象的嵌套实例。下面是一个浅层复制如何工作的例子。

```
const alien = {
  species: "Alien",
  arms: 5,
  feet: 9
};const ufo = {
  ...alien,
  legs: 8,
  feet: 6
};const copiedUfo = {
  ...ufo
}ufo.legs = 1console.log(copiedUfo);//Returns ---> {species: 'Alien', arms: 5, feet: 6, legs: 8}console.log(ufo);
//Returns ---> {species: 'Alien', arms: 5, feet: 6, legs: 1}
```

当我们更新 *ufo* 对象上的 *legs* 属性，并在控制台记录 *copiedUfo* 对象时，我们可以看到 *ufo* 对象的 *legs* 属性没有受到影响，仍然是值 8。当我们从控制台注销 ufo 对象时，我们可以看到它现在是 1。然而，如果我们改变一个嵌套对象中的一个属性，两个对象都会改变。让我们看一看。

```
const alien = {
  species: "Alien",
  arms: 5,
  feet: 9
};const ufo = {
  ...alien,
  legs: 8,
  feet: 6,
  journey: {
    length: 9000
  }
};const copiedUfo = {
  ...ufo
}ufo.legs = 1
ufo.journey.length = 10console.log(copiedUfo);//Returns ---> 
 *{
  species: 'Alien',
  arms: 5,
  feet: 6,
  legs: 8,
  journey: { length: 10 }
}*
```

在上面的例子中，我们给包含一个对象的 *ufo* 对象添加了一个*旅程*属性。对象内部是一个设置为 9000 的*长度*属性。在示例的最后，我们将其更改为 10。当代码运行时，我们可以看到虽然*腿*没有在*副本中发生变化，但是长度属性发生了变化。*

## 不同的数据结构

您只能将对象扩散到对象中，而不能将对象扩散到数组中。

```
const alien = {
  species: "Alien",
  arms: 5,
  feet: 9
};const ourArray = [...alien];
//Returns ---> VM229:6 Uncaught TypeError: alien is not iterable
    at <anonymous>:6:22
```

您可以将一个对象分散到数组中的一个对象中

```
const alien = {
  species: "Alien",
  arms: 5,
  feet: 9
};const ourArray = [{...alien}];console.log(ourArray);//Returns ---> [{species: "Alien", arms: 5, feet: 9}]
```

您也可以将一个数组扩展到一个对象中，但最终会得到基于元素索引的键值对。

```
const letters = ["a", "b", "c"];
const alphabet = {...letters};console.log(alphabet);//Returns ---> {0: 'a', 1: 'b', 2: 'c'}
```

请随时发表任何评论、问题或反馈，并关注我以获取更多内容！

我也有一个关于 ES6 的 Udemy 课程。