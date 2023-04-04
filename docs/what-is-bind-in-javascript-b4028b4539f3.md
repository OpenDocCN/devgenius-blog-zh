# JavaScript 中的 Bind 是什么？

> 原文：<https://blog.devgenius.io/what-is-bind-in-javascript-b4028b4539f3?source=collection_archive---------8----------------------->

在 JavaScript 中使用 Bind()的介绍

![](img/ad4efba989d3c8875c0a6ecedd30b5ec.png)

## 介绍

最近发表了一篇关于在 [JavaScript](https://medium.com/@codecupdev/call-and-apply-in-javascript-f4974fc781aa) 中使用 Call()和 Apply()的文章。Bind 经常与这些方法一起讨论，但它的工作方式略有不同。

当我们使用 bind 方法时，我们正在创建一个新的函数，但是当我们调用这个方法时，它的值被设置为传入的值。让我们先看一个基本的例子。

## 使用 Bind()

```
const studentOne = {
  name: "Abby",
  score: 80
};function studentScore() {
  return `${this.name} scored ${this.score}.`
};const result = studentScore.bind(studentOne);console.log(result());
//Returns ---> Abby scored 80.
```

在上面的例子中，我们创建了一个*学生通*对象。在这个对象内部，我们为学生*的名字*和*的分数*设置属性。接下来，我们使用一个函数声明并创建一个名为 *studentScore* 的函数。这个函数返回一个带有学生姓名和分数的模板文本。我们继续声明一个名为 *result* 的变量，并将调用 *studentScore* 函数的结果赋给这个变量，但是使用 bind 方法。我们传入 *studentOne* 对象作为要绑定的参数。最后，在控制台日志中，我们返回调用 result 的结果。

当在上面的例子中调用 bind 时，一个新的函数被创建并存储在变量 *result* 中。这与此的上下文一起存储。只有当我们稍后在控制台日志中调用这个函数时，这个函数才会真正运行。澄清一下，与 call()和 apply()不同，bind 方法不会立即执行。它只是返回一个新函数，这个函数的值是我们传递给参数的值。

## 传递要绑定的参数

我们可以通过查看如何向 bind 传递额外的参数来扩展前面的示例。如果我们的 *studentScore* 函数有一个参数，当我们调用 bind 时，我们可以同时传入这个参数。让我们来看一个例子。

```
const studentOne = {
  name: "Abby",
  score: 80
};function studentScore(subject) {
  return `${this.name} scored ${this.score} in ${subject}.`
};const result = studentScore.bind(studentOne, "Art");console.log(result());
//Returns ---> Abby scored 80 in Art.
```

我们通过将主题设置为*学生核心*函数的一个参数来扩展前面的例子。现在，当我们调用 bind 时，我们将字符串作为参数传递。当我们调用如下所示的*结果*时，通过传递 *Art* 参数，我们同样可以获得相同的结果。

```
const studentOne = {
  name: "Abby",
  score: 80
};function studentScore(subject) {
  return `${this.name} scored ${this.score} in ${subject}.`
};const result = studentScore.bind(studentOne);console.log(result("Art"));
//Returns ---> Abby scored 80 in Art.
```

这里需要注意的一点是，额外的参数将被忽略。

```
const studentOne = {
  name: "Abby",
  score: 80
};function studentScore(subject) {
  return `${this.name} scored ${this.score} in ${subject}.`
};const result = studentScore.bind(studentOne, "Art");console.log(result("Maths"));
//Returns ---> Abby scored 80 in Art.
```

在上面的例子中，当我们调用 bind 时，我们把 Art 作为参数传入。当我们调用 result 时，我们把数学作为参数传入。这并没有取代 *Art* ，相反，它只是增加了一个额外的参数，并且由于 *studentScore* 函数只接受一个参数，因此该参数被忽略。如果它有两个参数，那么它会工作。

```
const studentOne = {
  name: "Abby",
  score: 80
};function studentScore(subjectOne, subjectTwo) {
  return `${this.name} scored ${this.score} in ${subjectOne} & ${subjectTwo}.`
};const result = studentScore.bind(studentOne, "Art");console.log(result("Maths"));
//Returns ---> Abby scored 80 in Art & Maths.
```

我希望你喜欢这篇文章。请随时发表任何评论、问题或反馈，并关注我以获取更多内容！