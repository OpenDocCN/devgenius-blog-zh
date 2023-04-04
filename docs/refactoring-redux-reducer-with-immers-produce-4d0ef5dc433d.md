# 用 Immer 产品重构 Redux Reducer

> 原文：<https://blog.devgenius.io/refactoring-redux-reducer-with-immers-produce-4d0ef5dc433d?source=collection_archive---------1----------------------->

![](img/c68b087d84f1a7c654269fec06b308c6.png)

伊默！

好吧，这个星期我只是想分享 Immer 的生产功能，以及我们如何使用它来使我们的 Redux 减速器看起来更干净。

如果你使用过 Redux，我相信你已经在你的 reducer 中看到过这种语法…

当我们想要修改一个嵌套的属性时，我们必须使用 spread 操作符来构造相同的状态，然后一旦我们到达嵌套的属性，在本例中是`players`，我们就可以进行我们需要的任何更改。

如果我们能像这样直接修改状态不是更好吗？

是的，它会，但是我们不能这样做，因为 React 和 Redux 使用不可变的数据类型。基本上，不可变数据是不能改变或变异的数据。因此，每当我们想在 React 中更改数据时，我们只需给它分配一个全新的对象。

即

当我们使用`this.setState`时也是如此。当我们在`useState`和`this.setState`中使用 set 函数时，我们正在为 state 分配一个新的对象。

*React 之所以使用不可变数据类型，是出于性能的考虑。*简单比较对象的引用，而不是比较同一个对象的每个属性，检测变化的效率要高得多。举个例子，如果我们有一个有 1000 个值的数组，我们直接改变这个数组。如果我们想要检测一个变化，我们必须将旧数组中的每个值与新数组进行比较。

然而，如果我们简单地利用不可变的数据类型，并用一个不同的值创建一个新的数组，我们可以很容易地比较对对象的引用，以确定相等或变化。

因为 Redux 和 React 使用不可变的数据类型，所以上面我们直接改变状态的例子是行不通的。React 不会检测到变化，因为我们直接修改了状态，而不是分配一个新的对象。

# Immer 产品

这就是为什么我们应该使用 Immer 的`produce`功能。`produce`使我们能够以一种非常干净的方式从一个旧对象创建一个新的变异对象。

这就是`produce`的工作方式。它接受两个参数:第一个是我们想要修改的对象，第二个是“变异”对象的回调函数。产生的结果是一个新的对象，它等于原始对象加上“突变”。

使用基本数组的示例

第二个参数`draft => { draft[0] = 5 }`是一个函数，它接收我们传递给它的第一个对象。在函数体内是我们将要执行的突变。在这种情况下，我们只是给索引 0 赋值 5。

使用 produce，我们可以轻松地为我们的 reducer 编写 case 语句，而无需编写一堆嵌套的 spread 操作符。

# 从@reduxjs/toolkit 创建 Reducer

更进一步，我们还可以使用 ReduxJS 库中的`createReducer`函数来进一步重构我们的 reducer。`createReducer`函数集成了 Immer 的`produce`，允许我们在 reducer 中直接写状态突变。

例子

`createReducer`

第一个参数是我们商店的初始状态。

第二个参数是一个具有动作类型和匹配的产生函数的对象。

同样的，我们为`ADD_PLAYER`提供的函数很像`produce`中的回调函数。`createReducer`唯一不同的是我们还可以传入动作(见`ADD_PLAYER_USING_ACTION`)。然后在函数体中，我们可以直接突变状态。

我们的减速器用`produce`和`createReducer`不是干净多了吗？

我希望这能对你有所帮助。祝您愉快！

## 证明文件

*   [Immer 生产](https://immerjs.github.io/immer/docs/produce)
*   [Redux:结构化 Reducers](https://redux.js.org/recipes/structuring-reducers/immutable-update-patterns) (在底部它讲述了使用@reduxjs/toolkit 中的 createReducer)