# 提高 Javascript 开发人员素质的 15 个技巧

> 原文：<https://blog.devgenius.io/15-tips-to-improve-as-a-javascript-developer-e364597edc6f?source=collection_archive---------2----------------------->

在本文中，我将向您展示掌握 JavaScript 语言的 15 个绝佳技巧。我们将学习编码快捷方式、很少有人知道的特性和一些对 JS 程序员非常有用的“技巧”。

> **警告:**不是所有的提示都会取悦所有人。目的是展示有趣的可能性，但是知道什么时候是使用它们的最佳时机取决于程序员(例如，考虑代码的可读性)。

今天的提示难度增加了，所以如果你觉得有些提示太简单，继续读下去，它会变得更难(或者，你可能已经掌握了我今天要教的内容😅).

![](img/c2a083710c44dedfb4a89dcf61ca936e.png)

由 [Artem Maltsev](https://unsplash.com/@art_maltsev?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 1.无效和未定义的评估

在 JavaScript 中，我们很快了解到的一件事是，并不是所有的事情都像它看起来的那样，在像这样的动态语言中，变量会以多种方式给你带来问题。一个非常常见的测试是检查变量是否为空或未定义，甚至是“空”，如下例所示:

```
let username;
if (name !== null || name !== undefined || name !== '') {
   userName = name;
} else {
   userName = ""; 
```

进行相同评估的一个更简单的方法是:

```
let userName = name || "";
```

如果你不相信，请测试一下！

# 2.数组定义

所以你必须创建一个数组对象，然后用它的元素填充它，对吗？您的代码可能看起来像这样:

```
let a = new Array(); 
a[0] = "s1"; 
a[1] = "s2"; 
a[2] = "s3";
```

用一行代码做同样的事情怎么样？

```
let a = ["s1", "s2", "s3"]
```

很不错，是吧！

> 警告:我知道这个技巧比较简单，但是对我来说很简单，它可能会帮助一些从其他编程语言开始的人

# 3.三元运算符

著名的“一行 if/else”，三元运算符已经是 Java 和 C#等类似 C 语言的许多程序员的老熟人了。它也存在于 JS 中，可以像这样轻松地转换代码块:

```
let big;
if (x > 10) {
    big = true;
}
else {
    big = false;
}
```

在此:

```
let big = x > 10 ? true : false;
```

或者更简单:

```
let big = x > 10;
```

但是它也适用于函数调用吗？如果我有两个不同的函数，我想在 If 为真的情况下调用一个，在 if 为假的情况下调用另一个，通常你会这样做:

```
function x() { console.log('x') };
function y() { console.log('y') };

let z = 3;
if (z == 3) {
    x();
} else {
    y();
}
```

但是，抓紧你的椅子…你可以用三元函数做同样的函数调用:

```
function x() { console.log('x') };
function y() { console.log('y') };

let z = 3;
(z==3 ? x : y)(); // Shortcut
```

同样值得一提的是测试变量是否为真的 ifs，有些程序员仍然这样做:

```
if (likeJs === true)
```

当他们可以这样做的时候:

```
if (likeJs)
```

# 4.声明变量

是的，即使是变量的声明也有它的怪癖。尽管这并不完全是一个秘密，但您仍然会看到许多程序员做出这样的声明:

```
let x;
let y;
let z = 3;
```

当他们可以这样做的时候:

```
let x, y, z = 3;
```

# 5.使用正则表达式

当涉及到文本分析和验证，以及某些类型的网络爬虫的数据提取时，正则表达式是创建优雅而强大的代码的伟大工具。

您可以在以下链接中了解有关如何使用正则表达式的更多信息:

*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Guide/Regular _ Expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
*   https://regexr.com/
*   【https://regex101.com/ 

# 6.charAt()快捷方式

所以你想从字符串中选择一个特定位置的字符，对吗？我敢打赌，您想到的第一件事就是使用 charAt 函数，如下所示:

```
"string".charAt(0);
```

但是记住这个，你会得到同样的结果，记住字符串是一个字符数组的类比:

```
"string"[0]; // Returns 's'
```

# 7.基数为 10 的幂

这只是以 10 为基数的指数或者著名的充满零的**数的一种更精简的表示法。对于那些接近数学的人来说，看到其中的一个不会太惊讶，但一个数字 10000 在 JS 中可以很容易地被 1e4 取代，即 1 后面跟着 4 个 0，如下所示:**

```
for (let i = 0; i < 1e4; i++) {
```

# 8.模板文字

这个语义特性是 ECMAScript 版本 6 或更高版本所独有的，它极大地简化了读取变量集中的字符串连接。例如，下面的串联:

```
const question = “My number is “ + number + “, ok?”
```

这个很简单，你可能做过更糟糕的连接。从 ES6 开始，我们可以使用模板文字进行这种连接:

```
const question = `My number is ${number}, ok?`
```

# 9.箭头功能

箭头函数是声明函数的简称。是的，自从 JavaScript 的第一个版本以来，有了更多的方法来做同样的事情。例如，下面是一个求和函数:

```
function sum(n1,n2){
   return n1 + n2;
}
```

我们也可以这样声明这个函数:

```
const sum = function(n1,n2){
   return n1+n2;
}
```

但是使用箭头函数:

```
const sum = (n1,n2) => n1 + n2;
```

# 10.变元解构

这个技巧是针对那些有很多参数的函数，并且你决定用一个对象来替换它们。或者那些真正需要配置对象作为参数的函数。

到目前为止没问题，毕竟谁没经历过这个？问题是必须不断地访问对象，这个对象是由参数传递的，后面跟着我们想读取的每个属性，对吗？像这样:

```
function init(config){
   const s = config.s;
   const t = config.t;
   return s + t;// or config.s + config.t
}

init({s: "1", t: "2"});
```

参数析构功能正好可以简化这一点，同时通过用下面的语句替换前面的语句来提高代码的可读性:

```
function init({s, t}){
   return s + t;
}

init({s: 1, t: 2});
```

最重要的是，我们仍然可以在参数对象的属性中添加默认值:

```
function init({s = 10, t = 20}){
   return s + t;
}

init({s: 1});
```

这样，s 的值将为 1，但 t 的值将默认为该属性，即 20。

# 11.键值名称

一个非常令人上瘾的特性是给对象分配属性的简化方式。假设您有一个 person 对象，它有一个 name 属性，该属性将通过 name 变量进行赋值。它看起来像这样:

```
const name = "Joseph"
const person = { name: name }// { name: "Joseph" }
```

虽然你可以这样做:

```
const name = "Joseph"
const person = { name }// { name: "Joseph" }
```

也就是说，如果你的变量和属性同名，你不需要调用它，只需要传递变量。对于多个属性也是如此:

```
const name = "Joseph"
const canCode = true
const person = { name, canCode }
// { name: "Joseph", canCode: true }
```

# 12.地图

考虑以下对象数组:

```
const animals = [
    {
        "name": "cat",
        "size": "small",
        "weight": 5
    },
    {
        "name": "dog",
        "size": "small",
        "weight": 10
    },
    {
        "name": "lion",
        "size": "medium",
        "weight": 150
    },
    {
        "name": "elefante",
        "size": "large",
        "weight": 5000
    }
]
```

现在想象一下，我们只想将动物的名字添加到另一个数组中。通常我们会这样做:

```
let animalNames = [];

for (let i = 0; i < animals.length; i++) {
    animalNames.push(animals[i].name);
}
```

但是有了地图，我们可以这样做:

```
let animalNames = animais.map((animal) => {
    return animal.nome;
})
```

请注意，map 需要一个最多带有三个参数的函数:

*   第一个是**当前对象**(如在 foreach 中)
*   第二个是当前迭代的**索引**
*   第三个是**全阵**

显然，这个函数将为动物数组中的每个对象调用一次。

# 13.过滤器

如果我们想像上一篇技巧文章一样遍历动物对象的同一数组，但这次只返回那些大小“小”的对象，那该怎么办？

对于普通的 JS 我们该如何做呢？

```
let smallAnimals = [];

for (let i = 0; i < animals.length; i ++) {
    if (animals[i].size === "small") {
       smallAnimals.push(animals[i])
    }
}
```

然而，使用 **filter** 操作符，我们可以以一种更简洁、更清晰的方式来完成这项工作:

```
let smallAnimals = animais.filter((animal) => {
    return animal.size === "small"
})
```

Filter 需要一个带有参数的函数，该参数是当前迭代的对象(如在 foreach 中)，并且它应该返回一个布尔值，指示该对象是否将成为返回数组的一部分(true 表示它通过了测试，并将成为它的一部分)。

# 14.减少

Javascript 的另一个重要特性是 **reduce** 。它允许我们以一种非常简单和强大的方式对集合进行分组和计算。例如，如果我们想把动物对象数组中所有动物的重量加起来，我们该怎么做呢？

```
let totalWeight = 0;

for (let i = 0; i < animals.length; i++) {
    totalWeight += animals[i].weight;
}
```

但是使用 reduce，我们可以这样做:

```
let totalWeight = animals.reduce((total, animal) => {
    return total += animal.weight;
}, 0)
```

Reduce 需要一个带以下参数的函数:

*   第一个是**累加器**变量的当前值(在所有迭代结束时，它将包含最终值)
*   第二个参数是当前迭代的**对象**
*   第三个参数是当前迭代的**索引**
*   第四个参数是包含所有将被迭代的对象的**数组**

该函数将对数组中的每个对象执行一次，在执行结束时返回聚合值。

# 结论

你呢？你还有其他建议要添加到列表中吗？留在评论里吧！

感谢阅读！在本平台关注我，阅读更多开发内容。祝您愉快，再见！👋