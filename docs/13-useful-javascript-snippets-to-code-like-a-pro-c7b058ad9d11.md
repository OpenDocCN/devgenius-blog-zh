# 13 个有用的 JavaScript 代码片段，像专业人士一样编写代码

> 原文：<https://blog.devgenius.io/13-useful-javascript-snippets-to-code-like-a-pro-c7b058ad9d11?source=collection_archive---------3----------------------->

## 这是为你日常编程问题准备的 JavaScript 代码片段工具箱

![](img/0f3dc9050330474eda4dd4318c46456f.png)

**由** [设计**由**设计](https://www.freepik.com/)

在本文中，我将分享 JavaScript 的 **13 段代码。这是开发者的工具箱，你可以自由使用这个代码片段。所以，不要再浪费时间了，让我们开始吧。**

# 1.单行交换

每个程序员都知道交换是通过第三个变量 temp 来完成的。但是在 JavaScript 中，我们可以使用破坏性方法在一行中交换两个值，而不需要任何临时变量。

```
// Swappinglet x = 900;
let y = 1000;[y, x] = [x, y];console.log(x, y) // 1000 900
```

# 2.获取字节大小

这段代码将向您展示如何使用内置方法获取 JavaScript 中任何对象的字节大小。

```
// Get Byte Size of any Objectconst ByteSize = data => new Blob([data]).size;console.log(ByteSize("Yoo")) // 3
console.log(ByteSize(23244)) // 5
```

# 3.专业倒车

现在您不再需要使用循环来反转字符串，这个代码片段将帮助您用一行代码反转任何字符串。看看下面的代码示例。

```
// Pro Reversingconst Reverse = str => str.split("").reverse().join("");console.log(Reverse("Coding")) //  gnidoC
console.log(Reverse("World")) //  dlroW
```

# 4.检查 AllEqual

这个有用的代码片段将帮助您检查数组元素是相同还是不同。当您需要检查重复元素的数组时，这很方便。

```
const isEqual = arr => arr.every(val => val === arr[0]);console.log(isEqual([10,10,11,11,12,13])) // false
console.log(isEqual([10,10,10,10])) // true
```

# 5.随机选择

有时我们需要从数组中选择随机数据。假设您正在用 JavaScript 制作一个游戏，您想添加一个从数组中随机选择数据的函数。那么这段代码将会是一个有用的工具。

```
// Random Selectionfunction Random(array){
  return array[Math.floor(Math.random() * array.length )];
}let array = ["Google", "Youtube", "Twitter", "Instagram", "Facebook"];console.log(Random(array)); // Facebook
```

# 6.JSON 可读性

通过使用下面的代码片段，使您的 JSON 数据更整洁、易读。

```
// JSON READABLEconst Jsontest = JSON.stringify({ name: 'Haider', age : 22, class: 12 }, null, '\t');
console.log(Jsontest)// output
{
 "name": "Haider",
 "age": 22,
 "class": 12
}
```

# 7.字符串占位

您可以使用以下代码片段在字符串中包含变量。我们将在 **${}方法**的帮助下完成这项工作。看看下面的代码示例。

```
// String Placeholderlet placeholder = "Front";
let placeholder2 = "Back"
console.log(`I'm a ${placeholder} End Developer`); // I'm a Front End Developerconsole.log(`I'm a ${placeholder2} End Developer`); // I'm a Back End Developer
```

# 8.一行 if Else 语句

您可能在 JavaScript 中使用过 if-else 语句，它基本上是多行代码。但是你知道我们可以用一行写一个 if-else 语句吗？查看下面的代码示例。

```
// normal wayif (2 > 5){
  console.log(true);
}
else{
  console.log(false)
}// One liner way2 > 5 ? console.log(true) : console.log(false)
```

# 9.消除重复

这个代码片段将帮助您从数组中删除重复项。当数组的元素很长并且包含一些重复的元素时，这就很方便了。

```
// Rid of Duplicatesfunction dup(array)
{
  return [...new Set(array)];
}console.log(dup([1, 1, 3, 3, 4, 5, 6, 7, 7, 7, 9])) // [1, 3, 4, 5, 6, 7, 9]
```

# 10.将字符串拆分成数组

这个简单的代码片段将帮助您将字符串拆分成数组形式。看看下面的代码示例就知道了。

```
// split string into arrayconst string = "JavaScript"const convertsion = [...string]console.log(convertsion) // ["J", "a", "v", "a", "S", "c", "r", "i", "p", "t"]
```

# 11.对数组排序

排序是我们日常编程的一部分。您可能已经使用了循环方法和算法来对数据进行排序。但是这段代码将展示如何在 JavaScript 中使用 sort()内置方法进行排序。

```
// Sortingvar array = ["F", "E", "D", "C", "B", "A"]array.sort()console.log(array) // ["A", "B", "C", "D", "E", "F"]
```

# 12.第一个字母大写

这段代码将大写字符串的第一个字母。查看下面的代码示例。

```
const capitalize = ([letter, ...rest]) => letter.toUpperCase() + rest.join('');console.log(capitalize("javascript")) // Javascript
console.log(capitalize("programming")) // programming
```

# 13.获取当前时间

这段代码将获取 JavaScript 中您所在时区的当前时间。看看下面的代码。

```
// Current Timevar date = new Date(); // for now
let hour = date.getHours(); // => 9
let min=  date.getMinutes(); // =>  30
let sec = date.getSeconds(); // => 51console.log(hour + ":" + min + ":" + sec) // 16:34:33
```

# 最后的想法

我希望这篇文章对你有所帮助，并喜欢阅读它。请随意将这篇文章分享给你的程序员朋友。【JavaScript 编码快乐！

***你可以通过成为中等会员来支持我，查看下面。*** 👇

[](https://codedev101.medium.com/membership) [## 通过我的推荐链接加入 Medium—hai der Imtiaz

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

codedev101.medium.com](https://codedev101.medium.com/membership) 

***永不停止学习！看看我的其他文章，希望你会对它们感兴趣*** 😃

[](/how-to-send-gmail-with-python-step-by-step-guide-478b07251208) [## 如何用 Python 发送 Gmail 分步指南

### Gmail 是全球使用最广泛的个人和商业电子邮件，您在日常生活中使用过 Gmail 吗？

blog.devgenius.io](/how-to-send-gmail-with-python-step-by-step-guide-478b07251208) [](/make-an-instagram-bot-with-python-a0c8d5fd2092) [## 用 Python 构建一个 Instagram Bot

### 在本教程中，你将学习如何用 Python 和 InstaPy 构建一个机器人，它通过…

blog.devgenius.io](/make-an-instagram-bot-with-python-a0c8d5fd2092) [](/how-to-make-a-chrome-extension-f37bdfb6edb3) [## 如何制作一个 Chrome 扩展

### 使用分步指南构建您的第一个 chrome 扩展

blog.devgenius.io](/how-to-make-a-chrome-extension-f37bdfb6edb3) [](/opencv-python-tutorial-a-guide-to-learn-opencv-81b786f2365b) [## 使用 Python 的 OpenCV 初学者教程

### OpenCV python 入门，逐步学习基础知识

blog.devgenius.io](/opencv-python-tutorial-a-guide-to-learn-opencv-81b786f2365b) [](/creating-youtube-downloader-in-javascript-5b7e20215271) [## 用 Javascript 创建 Youtube 下载器

### 一步一步地指导如何用 javascript 创建 youtube 视频下载器。

blog.devgenius.io](/creating-youtube-downloader-in-javascript-5b7e20215271)