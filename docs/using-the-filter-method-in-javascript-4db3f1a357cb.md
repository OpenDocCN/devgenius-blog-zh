# 在 JavaScript 中使用 Filter 方法

> 原文：<https://blog.devgenius.io/using-the-filter-method-in-javascript-4db3f1a357cb?source=collection_archive---------11----------------------->

## 如何在 JavaScript 中使用数组过滤方法

![](img/974c352579fd996696843edd4db49c3c.png)

## 介绍

当我们使用数组时，我们可能会发现我们需要完成的一个常见任务是从数组中过滤出一些数据。也许我们只想返回数字数组中的奇数或偶数元素。也许我们只想让数组返回只包含特定字符的字符串。在这种情况下，数组过滤方法非常有用。

## 我们如何使用过滤器？

为了使用 filter 方法，我们向该方法传递一个回调。通常这是一个匿名函数，尽管我们可以传入一个函数的引用。我们传入的回调函数就像一个测试函数，需要返回一个布尔值(真或假)。如果元素通过了这个测试函数，并且返回 true，那么元素将被添加到返回的数组中。如果元素没有通过测试函数，元素将不会被添加到返回的数组中。使用 filter 方法时，原始数组不会发生变化。

## 对于循环

与其他数组帮助器方法一样，我们可以通过使用 for 循环来达到我们想要的结果，但是通常使用数组帮助器方法可以让我们编写出可读性和可扩展性更好的代码。让我们从一个例子开始，这个例子从一个数组的值中返回偶数。我们将从 for 循环开始。

```
const values = [10, 15, 20, 25, 30];let results = [];for(let i = 0; i < values.length; i++) {
  if(values[i] % 2 === 0) {
    results.push(values[i]);
  }
}console.log(results);
//Returns ---> [10, 20, 30]
```

在上面的例子中，我们首先用 const 声明一个变量，我们称之为*值*。我们用一个包含一些数字的数组来初始化它。接下来我们初始化另一个变量，这次使用字母 *results* 。我们继续使用一个 for 循环，它遍历*值*数组。如果元素是偶数，则将元素推入结果数组。当我们在循环外控制台记录*结果*数组时，我们看到该数组只包含偶数元素。这是可行的，但是如果我们使用 filter 方法，一些分支工作将会为我们完成，我们将会有不容易出错的代码。让我们看一下同一个例子，但是这次使用了 filter 方法。

## 使用过滤方法

```
const values = [10, 15, 20, 25, 30];const results = values.filter(function(value) {
  return value % 2 === 0;
});console.log(results);
//Returns ---> //Returns ---> [10, 20, 30]
```

这次我们使用存储在*值*变量中的同一个数组。然后我们创建另一个名为 *results* 的变量，它将存储使用 filter 返回的新数组。我们对*值*调用 filter 方法，检查*值*是否为偶数。当我们控制台记录*结果*时，我们得到了与使用 for 循环时相同的数组。

## 需要注意的事项

值得记住的是，当我们使用 filter 时，原始数组不会发生变化。我们可以比较上面示例中的原始阵列和新阵列，我们会发现它们非常不同。

```
console.log(results);
//Returns ---> [10, 20, 30]console.log(values);
//Returns ---> [10, 15, 20, 25, 30]
```

我们还必须确保当我们将回调传递给 filter 方法时，我们确实返回了一些东西，否则新数组将返回一个空数组。这显示在下面的例子中，我们没有使用 return 关键字。

```
const values = [10, 15, 20, 25, 30];const results = values.filter(function(value) {
  value % 2 === 0;
});console.log(results);
//Returns ---> []
```

## 其他示例

我们可以同样容易地对字符串数组使用 filter。让我们来看一个例子。我们将返回包含字母 a 的字符串数组。

```
const names = ["Sally", "Bob", "Lauren", "Ted"];const filteredNames = names.filter(function(name) {
  return name.includes("a");
});console.log(filteredNames);//Returns --> (2) ['Sally', 'Lauren']
```

在上面的例子中，我们使用 const 声明了一个名为 *names* 的变量。我们给它分配一个包含名字字符串的数组。然后我们创建一个名为 *filteredNames* 的变量。我们将使用过滤方法的结果赋给它。在 filter 方法的回调中，我们设置 name 参数来表示数组中的每个元素，并使用 includes 方法检查字符串元素是否包含字母 *a* 。当我们控制台记录 *filteredNames* 时，我们从包含字母 *a* 的 *names* 数组中获得一个只包含字符串的数组。

## 嵌套数据结构

最后，让我们看看如何在嵌套数据结构上使用 filtered 方法。我们将获取一个包含对象的数组，并返回 score 属性高于 90 的对象。

```
const results = [
  {
    name: "Billy",
    score: 90
  },
  {
    name: "Ted",
    score: 100
  },
  {
    name: "Laura",
    score: 9
  },
  {
    name: "Alice",
    score: 100
  },
  {
    name: "Peter",
    score: 98
  }
];const passed = results.filter(result => result.score > 90);console.log(passed);//Returns --->[
   {name: 'Ted', score: 100}
   {name: 'Alice', score: 100}
   {name: 'Peter', score: 98}
]
```

在上面的例子中，我们在名为 *results* 的变量中设置了嵌套数据结构。然后我们创建另一个名为*的变量并传递给*。此变量将包含从 filter 方法创建的新数组。我们设置一个*结果*参数，返回*分数*大于 90 的结果。

我希望你喜欢这篇文章，请随时发表任何意见，问题或反馈，并关注我的更多内容！