# Javascript 函数的不同行为

> 原文：<https://blog.devgenius.io/different-behaviors-of-javascript-functions-7e2e07d52351?source=collection_archive---------2----------------------->

看看箭头和关键字对“这个”做了什么

![](img/2407bd5130bb6bd820571d72ff123d07.png)

我在熨斗学校学习软件工程项目期间，我一直认为用`function`关键字声明的函数和箭头函数的行为是一样的，箭头函数只是一种更高级的编写方式。直到我犯了一个懒错误(这是最好的学习方法)才意识到这两种声明函数的方式对`this`关键字的影响是不同的。

让我们看看下面的两个函数，以及调用它们时会发生什么:

```
function sayMyName () {
  name = "Travis"
  return this.name
}sayMyName() // => "Travis" sayMyName = () => {
  name = "Travis"
  return this.name
}sayMyName() // => "Travis"
```

如您所见，这两个函数在被调用时都返回`“Travis”`。在这两种情况下，`this`都在这两个函数的范围之内，而`this.name`指的是那个函数的 name 属性。

当我们试图在一个对象上调用这些函数时，行为发生了变化。让我们只考虑`function`关键字版本，看看当我们在一个具有名称键/值对和函数引用的对象上调用它时会发生什么:

```
function sayMyName () {
  name = "Travis"
  return this.name
}let catt = {
  name: "Catt",
  sayMyName: sayMyName
}let scott = {
  name: "Scott",
  sayMyName: sayMyName
}sayMyName() // => "Travis"
catt.sayMyName() // => "Catt"
scott.sayMyName() // => "Scott"
```

正如你在上面看到的，调用使用`function`关键字的函数会改变`this`所指的内容。在这种情况下，`this`指的是调用函数的对象，或者是`catt`对象，或者是`scott`对象。

现在我们来看看箭头函数版本:

```
sayMyName = () => {
  name = "Travis"
  return this.name
}let catt = {
  name: "Catt",
  sayMyName: sayMyName
}let scott = {
  name: "Scott",
  sayMyName: sayMyName
}sayMyName() // => "Travis"
catt.sayMyName() // => "Travis"
scott.sayMyName() // => "Travis"
```

在这种情况下，它每次都返回`“Travis”`,因为当`this`在一个箭头函数中使用时，它被绑定到该函数的作用域中，因此无论在什么对象上调用该函数，都将`this.name`绑定到`“Travis”`的值。

这种对范围动作的绑定正是我们在 React.js 中总是希望在基于类的组件中使用箭头函数的原因。

```
*class* Button extends React.Component {
  state = { greeting: "Hello World!" } clickHandler ()  {
    this.setState({greeting: "Goodnight World!"})
  } render() {
    return (
      <>
        <h1>{this.state.greeting}</h1>
        <button onClick={this.clickHandler}>Click Me</button>
      </>
    )
  }
}export default Button
```

这里的预期操作是将屏幕上的文本从“Hello World！”当按钮被按下时，“晚安世界”。然而，当按下按钮时，我们得到一个错误`Cannot read property ‘setState’ of undefined`。因为我们使用了 function 关键字，所以`this`的值被绑定到我们调用它的对象上，在这个例子中这个对象是空的，这使得它是未定义的。

要实现这一点，只需将`clickHandler`函数的格式改为箭头即可！

```
*class* Button extends React.Component {
  state = { greeting: "Hello World!" }clickHandler = () => {
    this.setState({greeting: "Goodnight World!"})
  }render() {
    return (
      <>
        <h1>{this.state.greeting}</h1>
        <button onClick={this.clickHandler}>Click Me</button>
      </>
    )
  }
}export default Button
```

现在，`this`关键字被绑定到它所在的函数的范围内，从而可以访问内置于“反应”中的`setState`函数。