# JS 系列# 10:JS 中的主函数

> 原文：<https://blog.devgenius.io/js-series-10-functions-in-js-8cd1a42b0695?source=collection_archive---------9----------------------->

## 重构模块中的代码

功能是执行特定动作的一段代码。函数是一种在一个名字下定义代码块的方法，可以在需要时随时使用。

功能可以通过以下要点轻松定义…

1.  函数使我们能够编写可重用的模块化代码。
2.  函数在一个名称下定义了代码块**以便以后使用。**

![](img/bbbeae0e9b9bdd8a5a96bdc1225ca8ec.png)

梅森·金巴罗夫斯基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 使用功能—两步流程

创建和使用函数是一个两步过程…

1.  定义函数
2.  运行功能

# 运行功能

如果你对 JS 没什么经验，那么你应该已经知道有一大堆内置函数被分成不同的类别，比如`Math`函数、`String`函数、`Array`函数等等…

要通过任何函数执行操作，我们只需要执行函数。让我们看一些内置函数的例子…

```
**Example-1:**
const name = 'ishwar'
name.toUpperCase() // String function**Example-2:** Math.pow(2,5) // Math function**Example-3:** const colors = ['Red','Green','Blue']
colors.push('Yellow') // Array function
```

正如你在上面的例子中看到的，每个`function`名称后面都有一对`parenthesis ()`，它们可以有一些`argument`。

为了执行一些任务，我们需要来自用户的输入，而`input`可以由`function`中的`comma separated list of arguments`给出。

# 创建函数

JS 为您提供了一组丰富的预定义函数库来执行最常见的操作，但是大多数时候用户需要他们自己的自定义函数。因此 JS 允许您创建自己的自定义函数集。

要创建自定义函数，首先编写执行所需操作的代码块。

> 例如，如果你想创建一个向你问好的函数，那么代码应该如下所示…

```
**Step-1: // Write a chunk of code to perform action**console.log('Good Morning')**Step-2: // Give a name to the code block**sayGoodMorning()
{
   console.log('Good Morning')
}**Step-3: // Use function keyword to make this function**function sayGoodMorning() 
{   
     console.log('Good Morning')
}
```

## 需要注意的要点

1.  每个函数名后面都必须跟一对`parenthesis ()`。
2.  要创建一个代码块，使用`curly braces {}`。
3.  函数名有两种写法，像`say_good_morning`(用下划线分隔)和`sayGoodMorning (camel Case)`。如今`camel case`大会更受欢迎。

## 风格指南

在编写函数时，有两种流行的编写函数的风格…

```
// 1\. New line stylefunction sayGoodMorning()
{
    console.log('Good Morning')
}// 2\. Inline style
function sayGoodMorning() {
   console.log('Good Morning')
}
```

如果你是编程新手，你可以使用`New line style`,但是大多数有经验的程序员更喜欢使用`inline style`。

## **接收带参数的输入**

正如我们已经讨论过的，当创建一个`function`的时候，有时候接收`inputs`是必要的。所以为了接收`inputs`，我们在函数中使用`parameters`

```
**Example-1:**
function sayGoodMorning(name) {
   console.log(`Good Morning ${name}`)
}**Example-2:** function greet(name,msg) {
   console.log(msg+" "+name)
}
```

让我们运行`Example-1`来祝愿 3 个不同的人…

```
sayGoodMorning('Alex') // Good Morning Alex
sayGoodMorning('Jhon') // Good Morning Jhon
sayGoodMorning('Trilok') // Good Morning Trilok
```

在`Example-2`中，我们也定制了`greet` `msg`。请看下面的例子，它展示了如何`greet` 3 个有不同愿望的人。

```
greet('Avi','Happy Birthday') // Happy Birthday Avi
greet('Trilok','Good Morning') // Good Morning Trilok
greet('Justin','Bye Bye!!!') // Bye Bye!!! Justin
```

## 返回输出

`Functions`是动作，有些动作可以生成一个`output`供进一步使用。所以我们可以用`return`语句将生成的`output`值发送到`function`之外。

```
**Example-1:**
function sum(a,b) {
   return a + b
}**Example-2:**
function arraySum(arr) {
   let sum = 0;
   for(let i = 0; i < arr.length; i++) {
        sum += arr[i]
   }
  return sum;
}
```

如以上示例所示，第一个示例显示`sum()`函数返回两个数`a`和`b`的总和。
类似地`arraySum`函数查找数组元素的总数并返回输出。在下面的代码片段中，让我们看看如何运行和使用上述函数的输出…

```
**Example-1:**
console.log(sum(5,10)) // 15
console.log(sum(10,20) + 30) // 60
console.log(sum(sum(10,20),30)) // 60**Example-2:**
let x = sum(3,4) * 2 
console.log(x) // 14**Example-3:** const nums = [10,20,30,40]
let sum = arraySum(nums)
console.log(sum) // 100
```

# 用例:模拟掷骰子

```
function rollDice() {
    return Math.floor(Math.random() * 6) + 1;
}**Execution:**rollDice() // 2
rollDice() // 6
rollDice() // 4
```

# 避免！！！执行不到的代码

不可达代码表示已编写但从未执行的代码行。请看下面的例子来了解事实。

```
function sqr(x) {
   return x * x
   console.log(x) // Unreachable code
}sqr(5) // 25
```

如上例所示，return 会立即将控制权转移出函数，因此`console.log(x)`永远不会执行，所以我们必须删除这段代码。

一些编程语言如`Java`会为`unreachable`代码生成`errors`，但 JS 不会为`unreachable`代码显示任何`warning`和`error`。所以请小心！！！。

# 优化回报

Return 语句在执行时会立即将控制权转移到函数之外，return 之后的语句永远不会执行。因此，我们可以明智地优化 return 语句的使用，使代码更加紧凑。看一些例子来了解事实…

```
function max(a,b) {
   let result
   if(a > b) {
     result = a
   } else {
     result = b
   }
  return result
}
```

如上例所示，首先我们将输出存储在一个名为`result`的变量中，最后返回`result`的值。我们可以直接返回`result`而不存储到变量中，以使代码简单紧凑。请参见下面的示例…

```
function max(a,b) {
   if(a > b) {
      return a
   } else {
      return b
   }
}
```

在上面的代码片段中，如果条件`a > b`评估为`true`，则`return a`将被执行，否则`return b`将被执行。正如我们所知，如果执行了`return`，那么写在`return`之后的语句将永远不会被执行，所以我们可以使上面的例子更加简洁。请参见下面的代码片段…

```
function max(a,b) {
  if(a > b) {
     return a
  }
  return b
}
```

## 练习示例— 1:阵列平均值

> 使用函数求数组元素的平均值。

```
function avg(arr) {
   let sum = 0
   for(let i = 0; i < arr.length; i++) {
       sum += arr[i]
   }
   return sum / arr.length
}
```

## 练习例— 2:句子是 Pangram 吗？？

> 写一个函数`isPangram`来检查给定的句子是不是`pangram`。`pangram`是一个包含字母表中每个字母的句子，比如:
> `A quick brown fox jumps over the dog.`

```
function isPangram(sentence) {
   let lowerCaseSentence = sentence.toLowerCase() for(let char of 'abcdefghijklmnopqrstuvwxyz') {
      if(lowerCaseSentence.indexOf(char) === -1) {
          return false
      }
   }
  return true
}
```

> **对于..循环的**

`for…of`循环用于迭代集合，就像`array`一样，它迭代`array`并逐个返回数组项。

在上面的例子中，`for…of`循环用于迭代`string`，从`string`返回一个接一个的字符。

```
let nums = [10,20,30]
for(let item of nums) {
    console.log(item)
}**Output:**
10
20
30
```

> **for…in 循环**

`for…in`回路主要用作`for…of`回路。`for…in`循环的区别在于它遍历集合(`array`和`object`)并返回`key`值而不是`item`值。请参见下面的示例…

```
let nums = [10,20,30]
for(let item in nums) {
    console.log(item)
}**Output:** 0
1
2
```

要使用`for…in`循环访问该值，请使用带有索引值的`subscript []`操作符。请参见下面的示例…

```
let nums = [10,20,30]
for(let index in nums) {
    console.log(nums[index])
}**Output:** 10
20
30
```

## 练习示例— 3:挑选一张卡片

> 创建一个函数 pick card，它模拟纸牌游戏，为你挑选一张牌，并返回一个对象，如:
> `{ value: 'k', suit: 'clubs' }`
> 
> 从集合`{1,2,3,4,5,6,7,8,9,10,J,Q,K,A}`中选择一个随机值，并从`{clubs, spades, hearts, diamonds}`中选择随机花色

```
function pickCard() {
   const values = [2,3,4,5,6,7,8,9,10,J,Q,K,A];
   const suits = ['clubs','spades','hearts','diamonds']; const valueIndex = Math.floor(Math.random() * values.length);
   const pickedValue = values[valueIndex]; const suitIndex = Math.floor(Math.random() * suits.length);
   const pickedSuit = suits[suitIndex] return {value: pickedValue, suit: pickedSuit}}
```

如果你喜欢这篇文章，请关注我:

【中:】[https://medium.com/@maheshshittlani](https://medium.com/@maheshshittlani)
**Github:**[https://github.com/maheshshittlani](https://github.com/maheshshittlani)
**LinkedIn:**[https://in.linkedin.com/in/mahesh-shittlani-638b7429](https://in.linkedin.com/in/mahesh-shittlani-638b7429)