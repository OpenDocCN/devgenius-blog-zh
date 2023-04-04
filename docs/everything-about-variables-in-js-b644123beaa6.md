# JS 系列#3:关于 JS 中变量的一切

> 原文：<https://blog.devgenius.io/everything-about-variables-in-js-b644123beaa6?source=collection_archive---------20----------------------->

了解 let、const、var 和更多信息…

> 对于 JS 中的值，变量被标记为 jar。

![](img/b1af22b2ec7c378148e06e3f6e26844a.png)

你可以在上面的图片中看到日常生活中使用的贴有标签的罐子，有助于识别罐子里的东西。

![](img/9936a5f331ef17a19f0bcced9ffa3167.png)

像罐子一样易变

类似地，在 JavaScript 这样的编程中，变量就像上图所示的值的罐子。

我们可以存储一个值&给它一个名字，这样我们就可以…

1.  回忆一下
2.  使用它
3.  或者以后改变它。

# 基本语法

使用以下语法在 JavaScript 中创建变量。

```
let someName = someValue;
```

`let`JS 中的关键字/保留字是用来创建变量的吗？我们可以用任何有效的变量名创建一个变量，并用一个值初始化它。

`let`是 ES6 中引入的创建变量的新方法。

```
let age = 6; // Number variable
let name = 'Alex'; // String variable
let isValid = false; // Boolean variable
```

## 变量的类型

变量的类型取决于将要存储在该变量中的值的类型。

# 有效变量名的规则

1.  变量可以是字母(A-Z / a-z)、数字(0–9)、下划线(_)和美元($)的组合。
2.  变量名不能以数字开头。
3.  变量名中不允许有空格和特殊符号。
4.  保留字不允许作为变量名。
5.  JS 中的变量区分大小写，因此年龄、年龄和年龄是 JS 中的 3 个不同变量。

## 用于变量名的约定

如果一个变量包含多个单词，有两种常见的书写方式。

1.  用下划线分隔每个单词`number_of_players`
2.  camel case 命名约定`numberOfPlayers`

如今，camel case 命名约定更加流行。但她想用哪种约定完全是作者的选择。

## 有效的变量名

```
first_name
lastName
__ax
age
avgRating
friend1
friend2
num_of_rooms
numOfPlayers
```

## 无效的变量名

```
99acre
2friends
num of rooms
this // reserved words
let  // reserved words
```

# 回忆变量

一旦创建了一个变量，就可以在以后使用它来获取存储在其中的值。

```
**Example-1:**
let age = 25;
console.log(age);**Example-2:** let sal = 10000;
let bonus = 2000;
console.log(sal + bonus);
```

# 更新变量

一旦变量被创建并用一个值初始化，我们可以在以后更新变量的值。

```
let sal = 10000;
let increment = 5000;sal = sal + increment; // update
```

# 介绍 const

`const`与`let`相似，唯一不同的是`const`用于创建一个常量变量，一旦创建就不能更改。

常量变量的一些例子如下…

```
const PI = 3.14159
const DAYS_IN_WEEK = 7
const MIN_RENT = 60
const MIN_WAITING_IN_SECONDS = 120
```

## const 的命名约定

`const`可以和变量同名，但是编写常量变量的一个好习惯是常量变量全部大写，每个单词用下划线隔开。

## 为什么使用 const

一般使用`const`来存储在执行过程中不会改变的值。

一旦我们讨论了`Arrays` & `Objects`，我们将讨论`const`比`let`更有意义的其他情况。

# var——创造变量的遗产

`var distance = 77.5`

在`let`和`const`之前，`var`是声明变量的唯一方式。如今，没有理由使用它。

`var` & `let`在我们讨论范围时有一些区别。
总是建议使用`let`来创建变量。

如果你喜欢这篇文章，请关注我:

**中:**[https://medium.com/@maheshshittlani](https://medium.com/@maheshshittlani)
**Github:**[https://github.com/maheshshittlani](https://github.com/maheshshittlani)
**LinkedIn:**[https://in.linkedin.com/in/mahesh-shittlani-638b7429](https://in.linkedin.com/in/mahesh-shittlani-638b7429)