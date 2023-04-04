# 如何用 JavaScript 从日期中减去天数

> 原文：<https://blog.devgenius.io/javascript-date-subtract-days-62e7c7d6b1a7?source=collection_archive---------8----------------------->

![](img/2a23e962deb4bf6b6890fa129f5b0be4.png)

# 1.Date setDate()和 getDate()方法

在 JavaScript 中从`Date`中减去天数:

1.  调用`Date`上的`getDate()`方法来获取天数。
2.  减去天数。
3.  将减法的结果传递给`setDate()`方法。

例如:

```
function subtractDays(date, days) {
  date.setDate(date.getDate() - days); return date;
}// May 15, 2022
const date = new Date('2022-05-15T00:00:00.000Z');const newDate = subtractDays(date, 5);// May 10, 2022
console.log(newDate); // 2022-05-10T00:00:00.000Z
```

我们的`subtractDays()`函数将一个`Date`对象和要减去的天数作为参数。它返回减去天数后的同一个`Date`对象。

`Date getDate()`方法返回一个介于 1 和 31 之间的数字，表示特定`Date`在一个月中的第几天。

`Date setDate()`方法将`Date`对象中的日期更改为作为参数传递的数字。

如果您指定的日期会改变`Date`的月份或年份，`setDate()`会自动更新`Date`信息以反映这一点。

```
// April 25, 2022
const date = new Date('2022-04-25T00:00:00.000Z');date.setDate(40);// May 10, 2022
console.log(date); // 2022-05-10T00:00:00.000Zconsole.log(date.getDate()); // 10
```

四月只有 30 天，所以在这里将`40`传递给`setDate()`会使月份增加 1，并将月份的日期设置为`10`。

## 避免副作用

`setDate()`方法改变了它所调用的`Date`对象。这给我们的`subtractDays()`函数带来了一个副作用。为了避免修改传递的`Date`并创建一个纯函数，制作一个`Date`的副本并在这个副本上调用`setDate()`，而不是原始的。

```
function subtractDays(date, days) {
  // 👇 Make copy with "Date" constructor
  const dateCopy = new Date(date); dateCopy.setDate(dateCopy.getDate() - days); return dateCopy;
}const date = new Date('2022-05-15T00:00:00.000Z');const newDate = subtractDays(date, 5);console.log(newDate); // 2022-05-10T00:00:00.000Z// 👇 Original not modified
console.log(date); // 2022-05-15T00:00:00.000Z
```

**提示:**不修改外部状态的函数(即纯函数)往往更容易预测，也更容易推理，因为它们对于特定的输入总是给出相同的输出。这使得限制代码中副作用的数量成为一个很好的实践。

# 2.date-fns subDays()函数

或者，我们可以使用 [date-fns](https://www.npmjs.com/package/date-fns) NPM 包中的`subDays()`函数来快速从`Date`中减去天数。它的工作原理类似于我们的纯`subtractDays()`函数。

```
import { subDays } from 'date-fns';const date = new Date('2022-05-15T00:00:00.000Z');const newDate = subDays(date, 5);console.log(newDate); // 2022-05-10T00:00:00.000Z// Original not modified
console.log(date); // 2022-05-15T00:00:00.000Z
```

*原载于*[*codingbeautydev.com*](https://cbdev.link/5869ce)

# JavaScript 做的每一件疯狂的事情

一本关于 JavaScript 微妙的警告和鲜为人知的部分的迷人指南。

![](img/143ee152ba78025ea8643ba5b9726a20.png)

[**报名**](https://cbdev.link/d3c4eb) 立即免费领取一份。