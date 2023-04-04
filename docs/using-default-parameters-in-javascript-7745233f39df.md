# 在 JavaScript 中使用默认参数

> 原文：<https://blog.devgenius.io/using-default-parameters-in-javascript-7745233f39df?source=collection_archive---------6----------------------->

![](img/71bed8f820a7eeb2ec82ff95b4085163.png)

如果我们定义了一个函数，它要求我们使用一些参数，但是当我们调用这个函数时，我们需要把参数的值作为参数传入。

```
function myName(firstName, lastName) {
  return `${firstName} ${lastName}`;
}myName("Bob", "Murph");//Returns ---> 'Bob Murph'
```

在上面的例子中，我们定义了一个名为 *myName* 的函数，并在括号内为*的名*和*的名*设置参数。在函数体中，我们使用一个返回参数值的模板文本。然后我们调用这个函数，并传入参数 *Bob* 和 *Murph* 。我们得到 *Bob Murph* 作为返回值，因为 *Bob* 成为*名字*参数，而 *Murph* 成为*姓氏*参数。这很好，但是如果我们忘记传递一个参数会怎么样呢？让我们再看一看。

```
function myName(firstName, lastName) {
  return `${firstName} ${lastName}`;
}myName("Bob");//Returns ---> 'Bob undefined'
```

这一次，我们没有向函数调用传递第二个参数，所以当函数运行时，undefined 为第二个参数返回。如果我们做一些数学计算，这将导致更大的问题，因为我们不能使用 undefined 来执行任何计算，因此我们得到 NaN 返回。

```
function ourSum(valOne, valTwo) {
  return valOne + valTwo;
}ourSum(1);//Returns ---> NaN
```

## 我们如何保护自己免受这种伤害？

在 ES6 之前，我们可以自己设置函数的默认参数。如果我们再次使用 ourSum 示例，我们可以看到这是如何工作的。

```
function ourSum(valOne, valTwo) {
  if (typeof valTwo === 'undefined') {
    valTwo = 3;
  } return valOne + valTwo;
}ourSum(1);
//Returns ---> 4
```

在上面的例子中，我们定义了函数 *ourSum* ，并在括号内设置了两个参数 *valOne* 和 *valTwo* 。在函数体中，我们使用 if 语句来检查 *valTwo* 参数的类型是否未定义。如果未定义，则将 *valTwo* 的值设置为 3。这里值得注意的是*类型的*操作符返回一个字符串，这就是为什么使用严格等式我们需要检查*‘未定义’*而不是*未定义*。当我们运行这个函数时，我们得到 4 的返回值。我们只为 *valOne* 参数传入参数，因此 *valTwo* 被设置为默认值 3，总和等于 4。

## 默认参数

自 ES6 以来，我们已经能够使用默认参数，用少得多的代码实现相同的结果。当我们在函数的括号内定义参数时，我们所需要做的就是设置参数的默认值。如果在调用函数时为该参数传递了一个值，那么将不会使用默认值。如果没有为该参数传递任何参数，将使用默认值。

让我们再看一下上面的例子，但是这次我们将使用默认参数。

```
function ourSum(valOne, valTwo = 3) {
  return valOne + valTwo;
}ourSum(1);
//Returns ---> 4function ourSum(valOne, valTwo = 3) {
  return valOne + valTwo;
}ourSum(1, 5);
//Returns ---> 6
```

我们可以为不同类型的值(包括字符串和数组)使用默认参数，因此我们可以使用函数 *myName* 为第一个示例设置默认参数，如下所示:

```
function myName(firstName, lastName = "Murph") {
  return `${firstName} ${lastName}`;
}myName("Bob");//Returns ---> 'Bob Murph'
```

如果我们有一个需要使用数组的函数，那么我们也可以用它作为默认参数！

```
function ourArray(arr = [1, 2, 3]) {
  return arr;
}ourArray();
//Returns ---> [1, 2, 3]
```

我希望你喜欢这篇文章，请随时发表任何意见，问题或反馈，并关注我的更多内容！