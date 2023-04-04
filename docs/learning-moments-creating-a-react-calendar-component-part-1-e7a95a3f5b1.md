# 学习时刻创建 React 日历组件:第 1 部分

> 原文：<https://blog.devgenius.io/learning-moments-creating-a-react-calendar-component-part-1-e7a95a3f5b1?source=collection_archive---------7----------------------->

![](img/e548491b4be6954939300fb60f88a92b.png)

埃里克·罗瑟梅尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在最近与我的一位同事的讨论中，我们谈到了我们投资组合的项目主题。对话最终触及到一个想法，即你不需要创建庞大的功能程序来展示你的知识。迷你项目足以展示一套特定的技能，这是我在做下一个项目时一直记在心里的。

在我的日常工作中，我主要是一名后端开发人员，使用 NodeJs 和无服务器框架。每隔一段时间，我会做前端代码，但总是发现自己很难掌握我的同事对我们的产品做出的最新变化。考虑到这一点，我开始创建自己的组件，同时用 React 复习我的知识。因此，在这个由 3 部分组成的系列文章中，我将展示如何在 React 中创建一个日历组件。

请注意，我将不会深入到项目的设置和直接进入过程。如果你想要更多的知识或者一个简单的方法来启动你自己的 React 应用，看看[创建 React 应用](https://reactjs.org/docs/create-a-new-react-app.html)。

![](img/2879d52e7a713f20a108ea485d155afc.png)

使用 React、Javascript 和 MomentJs 创建

在本文(第 1 部分)中，我们将严格审视生成一个月中的日期以及前一个月和后一个月的溢出日期的逻辑。如果你将来选择创建自己的组件，我希望这篇文章能为你提供一些知识和有趣的读物。

乍一看，日历似乎简单明了。让我们抓住一堆日期，拍在屏幕上！当然，事情并不像我们希望的那样简单，在编写任何代码之前，必须考虑几件事情:

1.  如何获得给定月份的天数？
2.  如何知道给定月份中一周的第一天？
3.  *如何获取前/后几个月的溢出日期？*

在继续前进之前，我鼓励你花点时间想想你会怎么做。毕竟，脑力锻炼让你保持健康！

挑战 1:我如何获得给定月份的天数？

日期在编程中是一件棘手的事情，Javascript 和 NodeJs 社区在他们心中有一个特殊的位置。这个挑战很简单，因为这个库提供了惊人的功能，我们将利用这个功能，用`npm install --save moment`把这个包安装到我们的项目根目录中。

MomentJs 具有功能`daysInMonth()`。问题解决了！让我们看看当你给它一个月的时候它会给你什么。

```
import moment from 'moment';const getDaysInMonth = (month) => {
  return moment(month, 'MM').daysInMonth();
}// '01' = January...
console.log(getDaysInMonth('01'))
```

控制台应该返回 31 作为结果。简单吧？这是可行的，但是有一些挑战。默认情况下，Moment 通过从当前日期获取信息来假设任何缺失的信息，这意味着即使我们不直接传递年份，也会获取 2020 年 1 月的日期。当然，2020 年是一个独特的年份…你已经猜到了，是闰年！

如果我传入`getDaysInMonth('02') // 29`这个函数就可以了，但是如果我想要 2019 年 2 月的日子呢？幸运的是，MomentJs 有能力为我们处理这个问题。

```
import moment from 'moment';const getDaysInMonth = (month, year) => {
  return moment(`${month}-${year}`, 'MM-YYYY').daysInMonth();
}console.log(getDaysInMonth(2, 2019))
```

哒哒！挑战解决。该函数现在可以接受月份和年份作为其参数，并正确地确定特定年份的月份*中的天数。*

挑战 2:我如何知道给定月份的第一天是星期几？

这个挑战相对来说比较简单，所以我不会在这上面花太多时间。然而，让我们花些时间来弄清楚为什么我们需要这些信息。如果您见过 Javascript 中的大多数日期对象，您会看到类似这样的内容:

```
new Date() // 2020-07-07T05:00:00:000Z
```

有两种方法可以做到这一点。首先，让我们看看普通的 Javascript 方式:

```
const getFirstWeekdayOfMonth = (month, year) => {
  return new Date(year, month, 1).getDay()
}// Note, the month and result is 0-indexed
console.log(getFirstWeekdayOfMonth(0, 2020))
```

月份被表示为等于一月的`0`，因此 2020 年一月的第一个工作日是星期三，其索引在`3`。如果你对索引感到困惑，`[0, 1, 2, 3, 4, 5, 6] = Sunday — Saturday`。解决方案很简单，并且在很大程度上是可行的，但是处理日期是一件痛苦的事情，这里那里的一些警告会带来一些挑战。不过，知道这一点还是很好的，所以如果你感兴趣，可以学习更多关于 [Javascript 日期](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)的知识。

现在，让我们利用这个机会做同样的事情！

```
const getFirstWeekdayOfMonth = (month, year) => {
  return moment(
    `${month}-${year}`, 
    'MM-YYYY'
  ).startOf('month').weekday()
}// Note, since we use MomentJs's formatting, we do not index the month. This is getting the first weekday of the month for January 2020\. Result is 0-indexed
console.log(getFirstWeekdayOfMonth(1, 2020))
```

简单，结果与`3!`相同

**挑战 3:我如何获得前几个月/后几个月的溢出日期？**

![](img/2879d52e7a713f20a108ea485d155afc.png)

最后一个挑战是计算出我们需要在前几个月和后几个月中显示多少天。看这张成品组件的图片，我们需要灰色的日期信息。

但在我们开始之前，让我们做一些快速逻辑。

我们知道一个月中可能有 28-31 天。一周有 7 天。假设一个月的第一天可以是任何给定的工作日，我们想知道给定的一个月可以是几周的一部分。看看上面的图片，我们知道 2020 年 7 月将在 5 周之后到来。但是，等等…如果这个月的第一天是星期六呢？

如果一个月的第一天是星期六，那么一个月可以是 6 周的一部分。假设一个月最多有 31 天，那么一个月最多有 6 周。这是考虑到在我们的有生之年不会出现新的日期约定。当开发人员需要开始处理跨多个星球的日期时，我希望我不在身边！

因为我们知道 6 周是任何给定月份可以包含的最大周数，假设我们的日历需要显示总共 42 个日期(7 * 6 = 42)。

现在，让我们算出上个月的溢出日期。为此，我们需要使用上面创建的函数知道当前月份和第一个工作日。首先，让我们在使用 MomentJs 构造日期之后，将第一个月添加到一个数组中。

```
const getDatesInMonthDisplay = (month, year) => {
  const daysInMonth = getDaysInMonth(month, year);
  const firstWeekday = getFirstWeekdayOfMonth(month, year);
  const result = []; for (let i = 1; i <= daysInMonth; i++) {
    result.push(
      moment(`${month}-${i}-${year}`, 'MM-DD-YYYY').toDate()
    )
  } return result;
}// July 2020// Note, since we use MomentJs's formatting, we do not index the month. This is getting the first weekday of the month for January 2020\. Result is 0-indexedconsole.log(getDatesInMonthDisplay(7, 2020))
```

结果应该由表示 2020 年 7 月的每一天的 date 对象数组组成。

```
[ 2020-07-01T07:00:00.000Z,
  2020-07-02T07:00:00.000Z,
  2020-07-03T07:00:00.000Z,
  2020-07-04T07:00:00.000Z,
  2020-07-05T07:00:00.000Z,
  ...
]
```

看一下`firstWeekday`变量，我们可以确定上个月有多少天需要溢出。对于 2020 年 7 月，正如我们上面所确定的，第一个工作日是星期三或指数 3。因此，我们知道我们需要上个月的 3 天才能在月显示开始时完成一整周… [0，1，2…]。

首先，让我们添加两个快速助手功能来确定前/后月份和年份！

```
const getPrevMonthYear = (month, year) => {
  // If it is January... prev month is Dec of the previous year
  if (month === 1) { 
    return {
      month: 12,
      year: year - 1
    }
  } // Otherwise, same year, but month - 1
  return {
    month: month - 1,
    year
  }
}const getNextMonthYear = (month, year) => {
  // If it is January... prev month is Dec of the previous year
  if (month === 1) { 
    return {
      month: month + 1,
      year
    }
  }// Otherwise, same year, but month - 1
  return {
    month: 12,
    year: year + 1
  }
}
```

现在，使用助手功能…

```
const getDatesInMonthDisplay = (month, year) => {
  const daysInMonth = getDaysInMonth(month, year);
  const firstWeekday = getFirstWeekdayOfMonth(month, year);
  const result = []; const prev = getPrevMonthYear(month, year);
  const prevDaysInMonth = getDaysInMonth(
    prev.month, 
    prev.year
  ); // Add prev overflow dates... 
  for (let j = firstWeekday - 1; j >= 0; j--) {
    result.push(
      moment(
        `${prev.month}-${prevDaysInMonth - j}-${prev.year}`, 
        'MM-DD-YYYY'
      ).toDate()
    )
  } // Add current month's dates
  for (let i = 1; i <= daysInMonth; i++) {
    result.push(
      moment(`${month}-${i}-${year}`, 'MM-DD-YYYY').toDate()
    )
  } return result;
}// July 2020// Note, since we use MomentJs's formatting, we do not index the month. This is getting the first weekday of the month for January 2020\. Result is 0-indexedconsole.log(getDatesInMonthDisplay(7, 2020))
```

现在，我们应该让数组正确地从上个月的日期开始，直到显示当前活动月份的第一个工作日。让我们用下个月的溢出日期来填满其余的！

使用助手`getNextMonthYear` …

```
const getDatesInMonthDisplay = (month, year) => {
  const daysInMonth = getDaysInMonth(month, year);
  const firstWeekday = getFirstWeekdayOfMonth(month, year);
  const result = [];const prev = getPrevMonthYear(month, year);
  const prevDaysInMonth = getDaysInMonth(
    prev.month, 
    prev.year
  ); // Add prev overflow dates... 
  for (let j = firstWeekday - 1; j >= 0; j--) {
    result.push(
      moment(
        `${prev.month}-${prevDaysInMonth - j}-${prev.year}`, 
        'MM-DD-YYYY'
      ).toDate()
    )
  } // Add current month's dates
  for (let i = 1; i <= daysInMonth; i++) {
    result.push(
      moment(`${month}-${i}-${year}`, 'MM-DD-YYYY').toDate()
    )
  } // Overflow dates for next month to meet 42 days per month display   requirement
  if (result.length < 42) {
    const daysToAdd = 42 - result.length;
    const next = getNextMonthYear(month, year); for (let k = 1; k <= daysToAdd; k++) {
      result.push(
        moment(
          `${next.month}-${k}-${next.year}`, 
          'MM-DD-YYYY'
        ).toDate()
      )
    }
  } return result;
}// July 2020// Note, since we use MomentJs's formatting, we do not index the month. This is getting the first weekday of the month for January 2020\. Result is 0-indexedconsole.log(getDatesInMonthDisplay(7, 2020))
```

还有…维奥拉！我们将日期对象中的总天数传递到日历组件中的月份显示中，包括当前月份以及之前和之后的溢出日期！这个`result.length`就是`42`确保第一个索引是星期天，最后一个索引是星期六。

```
[ 2020-06-28T07:00:00.000Z,
  2020-06-29T07:00:00.000Z,
  ...,
  2020-08-07T07:00:00.000Z,
  2020-08-08T07:00:00.000Z ]
```

在我们结束之前，让我们添加一些快速信息，以便更容易地确定哪些日期是显示的当前月份的一部分。

```
const getDatesInMonthDisplay = (month, year) => {
  const daysInMonth = getDaysInMonth(month, year);
  const firstWeekday = getFirstWeekdayOfMonth(month, year);
  const result = [];const prev = getPrevMonthYear(month, year);
  const prevDaysInMonth = getDaysInMonth(
    prev.month, 
    prev.year
  ); // Add prev overflow dates... 
  for (let j = firstWeekday - 1; j >= 0; j--) {
    result.push({
      date: moment(
        `${prev.month}-${prevDaysInMonth - j}-${prev.year}`, 
        'MM-DD-YYYY'
      ).toDate(),
      currentMonth: false
    })
  } // Add current month's dates
  for (let i = 1; i <= daysInMonth; i++) {
    result.push({
      date:moment(`${month}-${i}-${year}`, 'MM-DD-YYYY').toDate(),
      currentMonth: true
    })
  } // Overflow dates for next month to meet 42 days per month display   requirement
  if (result.length < 42) {
    const daysToAdd = 42 - result.length;
    const next = getNextMonthYear(month, year);for (let k = 1; k <= daysToAdd; k++) {
      result.push({
        date: moment(
          `${next.month}-${k}-${next.year}`, 
          'MM-DD-YYYY'
        ).toDate(),
        currentMonth: false
      })
    }
  } return result;
}// July 2020// Note, since we use MomentJs's formatting, we do not index the month. This is getting the first weekday of the month for January 2020\. Result is 0-indexedconsole.log(getDatesInMonthDisplay(7, 2020))
```

当我们把它传递给组件时，像这样的小细节会使它变得更容易，它们比你想象的更有帮助。这是一个新结果的例子。

```
[ { date: 2020-06-28T07:00:00.000Z, currentMonth: false },
  { date: 2020-06-29T07:00:00.000Z, currentMonth: false },
  { date: 2020-06-30T07:00:00.000Z, currentMonth: false },
  { date: 2020-07-01T07:00:00.000Z, currentMonth: true },
  ...,
  { date: 2020-08-07T07:00:00.000Z, currentMonth: false },
  { date: 2020-08-08T07:00:00.000Z, currentMonth: false } ]
```

接下来，在[第 2 部分](https://medium.com/@elbert.bae/creating-a-react-calendar-component-part-2-8992af1426fb)中，我们将看看 React 的渲染逻辑，使用这个函数来构造为我们显示的日期。