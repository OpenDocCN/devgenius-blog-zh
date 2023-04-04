# 如何在 JavaScript 中获得一个月的简称

> 原文：<https://blog.devgenius.io/javascript-get-month-short-name-c7adf1b7d182?source=collection_archive---------7----------------------->

![](img/0ad7c4cb1e03982fe953c343a5462bab.png)

要在 JavaScript 中获得月份的简称，创建一个具有给定月份的`Date`对象，然后使用给定的区域设置和一组选项调用`Date`上的`toLocaleString()`方法。其中一个选项应该指定月份名称应该是缩写形式。

例如，下面是我们如何获得 3 个字母的月份名称:

```
function getMonthShortName(monthNo) {
  const date = new Date();
  date.setMonth(monthNo - 1); return date.toLocaleString('en-US', { month: 'short' });
}console.log(getMonthShortName(1)); // Jan
console.log(getMonthShortName(2)); // Feb
console.log(getMonthShortName(3)); // Mar
```

我们可以通过将`month`设置为`narrow`来获得 1 个字母的月份名称:

```
function getMonthShortName(monthNo) {
  const date = new Date();
  date.setMonth(monthNo - 1); return date.toLocaleString('en-US', { month: 'narrow' });
}console.log(getMonthShortName(1)); // J
console.log(getMonthShortName(2)); // F
console.log(getMonthShortName(3)); // M
```

我们的`getMonthShortName()`函数获取一个位置，并返回该位置所在月份的简称。

方法将对象的月份设置为一个指定的数字。

## 注意

传递给`setMonth()`的值应该是从零开始的。例如，`0`代表一月，`1`代表二月，`2`代表三月，依此类推。这就是为什么我们将从月份号(`monthNumber - 1`)中减去的`1`的值传递给`setMonth()`。

# Date toLocaleString()方法

我们使用 [Date toLocaleString()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString) 方法来获取日期所在月份的名称。`toLocaleString()`返回一个字符串，该字符串带有日期的语言敏感表示。

该方法有两个参数:

1.  `locales`:带有 BCP 47 语言标签的字符串，或者此类字符串的数组。我们可以指定许多语言环境，比如`en-US`表示美国英语，`en-GB`表示英国英语，`en-CA`表示加拿大英语。
2.  `options`:用于调整日期输出格式的对象。

在我们的示例中，我们将`en-US`作为语言标签来使用美国英语，并将`short`和`narrow`的值设置为 options 对象的`month`属性，以显示月份的简称。

我们可以传递一个空数组(`[]`)作为第一个参数，让`toLocaleString()`使用浏览器的默认语言环境:

```
function getMonthShortName(monthNo) {
  const date = new Date();
  date.setMonth(monthNo - 1); // Use the browser's default locale
  return date.toLocaleString([], { month: 'short' });
}console.log(getMonthShortName(1)); // Jan
console.log(getMonthShortName(2)); // Feb
console.log(getMonthShortName(3)); // Mar
```

这有利于国际化，因为输出会根据用户偏好的语言而变化。

# 国际机场。DateTimeFormat 对象

使用`toLocaleString()`意味着每次想要一个语言敏感的月份简称时，都必须指定一个`locale`和`options`。为了使用相同的设置来格式化多个日期，我们可以使用一个 [Intl 的对象。改为 DateTimeFormat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat) 类。

例如:

```
function getTwoConsecutiveMonthNames(monthNumber) {
  const date1 = new Date();
  date1.setMonth(monthNumber - 1); const date2 = new Date();
  date2.setMonth(monthNumber); const formatter = new Intl.DateTimeFormat('en-US', { month: 'short' }); // Format both dates with the same locale and options
  const firstMonth = formatter.format(date1);
  const secondMonth = formatter.format(date2); return `${firstMonth} & ${secondMonth}`;
}console.log(getTwoConsecutiveMonthNames(1)); // Jan & Feb
console.log(getTwoConsecutiveMonthNames(2)); // Feb & Mar
console.log(getTwoConsecutiveMonthNames(3)); // Mar & Apr
```

*最初在 codingbeautydev.com 出版*

# ES13 中 11 个惊人的新 JavaScript 特性

本指南将带您快速了解 ECMAScript 13 中添加的所有最新功能。这些强大的新特性将会用更短、更富于表现力的代码来更新您的 JavaScript。

![](img/75a56482761ab63cfc081ad691d70d61.png)

[**报名**](https://cbdev.link/900477) 立即免费领取一份。