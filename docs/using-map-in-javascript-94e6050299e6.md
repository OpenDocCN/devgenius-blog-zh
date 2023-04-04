# 在 JavaScript 中使用 Map

> 原文：<https://blog.devgenius.io/using-map-in-javascript-94e6050299e6?source=collection_archive---------11----------------------->

## 了解如何在 JavaScript 中使用 Map Array 辅助方法

![](img/abf881469e98e2381cf59771f07b0cac.png)

## 介绍

JavaScript 的 ES6 版本中引入了 Map array helper 方法来帮助我们处理数组。我最近写了一篇文章，解释了如何使用 [forEach](https://javascript.plainenglish.io/how-to-use-the-javascript-foreach-method-a9e6f9e9a071) 。在这篇文章中，我们将看看地图。

## 地图是做什么的？

当我们使用 map 时，我们可以从现有的阵列创建一个新阵列。我们传入一个回调函数，这个函数在数组中的每个元素上都被调用。使用该方法，您可以构建一个新的数组，对原始元素进行处理。使用 map 可以做的一些示例包括挑选某些元素、简单地复制数组(对于这一点，展开可能更简单)或以某种方式操作数组数据。

## 要点

当你使用地图的时候，有一些必备的东西。首先你必须返回一些东西，否则你会得到一个新的数组，每个元素都没有定义。您还应该将对 map 的调用存储到一个变量中，否则您将没有对新数组的任何引用。当我们使用 map 时，初始数组没有改变，但是创建了一个新数组。让我们看一些例子。

## 例子

如果你有一个数组的值，但你想把每个值都加倍，那么 map 就可以做到这一点。

```
const values = [10, 20, 30, 40, 50];const doubled = values.map(function(value) {
  return value * 2;
});console.log(values);
//Returns ---> [10, 20, 30, 40, 50]console.log(doubled);
//Returns ---> [20, 40, 60, 80, 100]
```

在上面的例子中，我们创建了一个名为*值*的变量，并给它分配了一个包含五个值的数组。接下来，我们创建另一个名为 *doubled* 的变量，我们将它分配给方法映射的调用。我们使用希望调用 map 的数组的名称，在我们的例子中是值*，后面跟一个点(记住我们使用的是方法)，后面跟 map 方法名。我们传递一个匿名函数作为对 map 的回调。你可以在这里传入一个命名函数，但是因为我们只使用一次，所以匿名函数通常更合适。我们设置*值*参数，它将是每次 map 运行时来自数组的元素。我们返回值乘以 2 的总和。当我们控制台记录值数组时，我们可以看到什么都没有改变。当我们控制台记录 doubled 时，这是我们的新数组，我们可以看到它包含了 doubled 元素。*

*我们可以很容易地用 for 循环实现相同的结果，但是代码会复杂得多，因此容易出错。*

```
*const values = [10, 20, 30, 40, 50];
const doubled = [];for(let i = 0; i < values.length; i++) {
  doubled.push(values[i] * 2);
}console.log(values);
console.log(doubled);*
```

*我们可以使用 map 轻松地操作字符串数组。我们将获取一个大写问候数组，并以小写形式返回。*

```
*const greetings = ["HI", "BYE", "YO", "WHATS UP"];
const formattedGreetings = greetings.map(function(greeting) {
  return greeting.toLowerCase();
})console.log(greetings);
//Returns ---> ['HI', 'BYE', 'YO', 'WHATS UP']console.log(formattedGreetings);
//Returns ---> ['hi', 'bye', 'yo', 'whats up']*
```

*如果我们有一个嵌套的数据结构，并希望挑选出一个属性，并用这些值创建一个数组，map 也是理想的。*

```
*const students = [
{
  name: "Anna",
  age: 50
},
{
  name: "Pete",
  age: 20
},
{
  name: "Abby",
  age: 12
  }
];const studentAges = students.map(function(student) {
  return student.age
});console.log(studentAges);
//Returns ---> [50, 20, 12]*
```

*在上面的例子中，我们创建了一个数组，存储在一个名为 *students* 的变量中。该数组包含三个带有学生姓名和年龄的对象。我们创建了另一个名为 *studentAges* 的变量，它存储了使用 map 并从学生数组中的每个对象中挑选出*年龄*的结果。*

*我们也可以使用 map 返回一个对象数组。让我们再次使用学生数组，但是格式化值。我们将把名字全部改为小写，并给每个学生的年龄加一。*

```
*const students = [
{
  name: "Anna",
  age: 50
},
{
  name: "Pete",
  age: 20
},
{
  name: "Abby",
  age: 12
  }
];const updatedStudents = students.map(function(student) {
  return {
   lowercaseName: student.name.toLowerCase(),
   agePlusOne: student.age + 1
  }
});console.log(updatedStudents);
//Returns --->
[
 {lowercaseName: 'anna', agePlusOne: 51}
 {lowercaseName: 'pete', agePlusOne: 21}
 {lowercaseName: 'abby', agePlusOne: 13}
]*
```

*在上面的例子中，我们在回调函数中返回了一个对象。我们设置属性 *lowercaseName* 和 *agePlusOne* 。 *lowercaseName* 属性使用 toLowerCase 字符串方法存储学生*的姓名*。属性存储学生的年龄，每个元素加 1。*

## *常见错误*

*在本文的前面，我讨论了不返回 map 回调中的任何内容会导致返回一组未定义的值。下面是一个例子。*

```
*const letters = ["a", "b", "c"];
const upperCased = letters.map(function(letter) {
  letter.toUpperCase();
})console.log(letters);
//Returns ---> ['a', 'b', 'c']console.log(upperCased);
//Returns ---> [undefined, undefined, undefined]*
```

*我希望你喜欢这篇文章，请随时发表任何意见，问题或反馈，并关注我的更多内容！*