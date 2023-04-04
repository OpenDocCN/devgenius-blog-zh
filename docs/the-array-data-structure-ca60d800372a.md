# 数组数据结构

> 原文：<https://blog.devgenius.io/the-array-data-structure-ca60d800372a?source=collection_archive---------5----------------------->

![](img/88d040bcc3b4551cf013b1ca655ee1f7.png)

这是我的使用 JavaScript 的数据结构和算法系列的第二篇文章。上周讨论了[时间复杂度，空间复杂度，大 O 记法](https://www.martincartledge.io/time-complexity-Space-complexity-and-big-o-notation/)。本周我将谈论一个大多数程序员日常使用的非常流行的数据结构，即*数组*。在这篇文章中，我将介绍常见的`Array`动作(`push`、`pop`等)，我们还将介绍创建我们自己的*数组*数据结构的过程！让我们开始吧。

# 什么是数组？

> 可以使用单个变量存储的多个值的集合

*   长度不能固定(除非是静态的)
*   值的类型不能是固定的
*   不能使用字符串作为元素的索引，必须使用整数

# 静态阵列与动态阵列

## 静态

> 大小固定

## 动态的

> 在具有更多内存的新位置复制并重建`Array`，随着您添加元素而扩展

# 常见阵列动作

## 按下 O(1)

> 在`Array`的末尾追加一个新值，并返回新长度

*   依赖于`length`属性来知道在哪里插入新值
*   如果`length`不存在或不能转换成数字，则使用`0`

首先，我们使用关键字`const`创建一个带有标识符`jediCouncil`的新变量。分配给`jediCouncil`的值是类型为`string`的值的`Array`。

接下来，我们用一个参数`anakin`调用`jediCouncil`上的`push`方法。

当我们在下一行记录我们的`jediCouncil`时，我们看到值`anakin`现在是我们的`jediCouncil` `Array`中的最后一个值。

因为只采取了一个动作，我们不必为这个操作迭代我们的`Array`,`push`方法的大 O 是`O(1)`。

## 流行 O(1)

> 删除`Array`中的最后一个值并返回该值

*   如果你调用一个空的`Array`，`pop`返回`undefined`

对于这个例子，我们想要从`jediCouncil`中取出`anakin`，我们可以使用`pop`方法:

首先，我们使用`const`关键字创建一个带有标识符`jediCouncil`的新变量。分配给`jediCouncil`的值是`string`类型值的`Array`。

接下来，我们调用`jediCouncil` `Array`上的`pop`方法，调用这个方法时我们不需要参数。

现在，当我们在下一行记录我们的`jediCouncil`时，我们应该看到值`anakin`不再在我们的`jediCouncil` `Array`中

后来，`anakin`👋🏻

使用`pop`可以非常快速、轻松地移除`Array`中的最后一个项目。因为这是唯一执行的操作，所以`pop`方法的大 O 是`O(1)`。

## 移位 O(n)

> 删除`Array`中的第一个值并返回该值

*   连续移动值及其索引

首先，我们使用关键字`const`声明一个带有标识符`jediCouncil`的新变量。分配给`jediCouncil`的值是`string`类型值的`Array`。

> 注意:我记下了每个值的索引位置，这将有助于稍后说明`shift`在幕后做什么

接下来，我在我们的`jediCouncil`变量上调用`shift`方法。

在下一行，我使用`console.log`来记录`jediCouncil`的新值。请注意索引位置的变化。这是为什么呢？

当在我们的`jediCouncil` `Array`上调用`shift`时，值`yoda`被移除。由于这个值位于索引位置`0`，我们必须遍历`Array`并更新每个值的索引位置。这就是为什么`shift`方法有一个`O(n)`的大 O。

现在我们可以看到`yoda`已经被移除，并且`jediCouncil`中的所有其他值已经被*移动到*更少的索引位置。

## 接合 O(n)

> 删除、替换或添加新值到`Array`

首先，我们使用关键字`const`创建一个标识符为`jediCouncil`的新变量。分配给`jediCouncil`的值是`string`类型值的`Array`。

接下来，我们在`jediCouncil`上调用`splice`方法`Array`。

> 注意:`splice`方法有 3 个参数:
> 
> `*start*` *—这是您想要开始更改* `*Array*`的索引
> 
> `*deleteCount*` *—这是您想要从* `*Array*` *中移除的值的数量(从* `*start*` *参数开始)*
> 
> `*items*` *—这是您想要添加到* `*Array*` *的值，从* `*start*` *参数*开始
> 
> *如果* `*items*` *参数为空，则* `*spice*` *方法将只删除项目*

我们将 3 个参数传递给`splice`:

*   `5`:我们想在索引位置`5`开始改变`jediCouncil` `Array`
*   `0`:我们不想从`jediCouncil`中删除任何东西；因此，这个值就是`0`
*   `”obi wan”`:这是我们想要添加到索引位置`5`的值

当我们在下一行记录我们的`jediCouncil`时，我们可以看到`obi wan`已经被添加到索引位置`5`的`jediCouncil`中；在这种情况下，这是最后一个位置。

欢迎登机，`obi wan`👍🏻，我认为你会很好地融入

虽然我们没有`shift`任何值或它们的索引位置，但我们在确定大 O 时总是采取最坏的情况；所以`splice`的大 O 就是`O(n)`

# 让我们创建一个数组数据结构

本节假设您对 JavaScript 的类的工作原理有所了解。如果课程对你来说是新的，不要害怕！在不久的将来，我会写一篇关于这些的文章。同时，你可以在这里阅读更多关于他们的信息[。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)

我们知道`Array`的核心部分是如何工作的，所以让我们构建我们自己的`Array`数据结构！

我们首先使用`class`关键字创建一个新的 JavaScript 类。我们给新的`class`命名为`MyOwnArray`。

## 构造器

在我们的`MyOwnArray` `class`中，我们编写了我们的`constructor`函数。`constructor`是一个负责为那个`class`创建对象的方法。

我们使用`this`关键字来创建两个字段并将其绑定到我们的`MyOwnArray`类的范围:

*   `length`:用`0`的值初始化的`number`
*   `data`:用空对象`{}`的值初始化的`object`

## 推

我们创建一个标识符为`push`的方法，它只有一个参数`item`。请记住，这个`item`参数可以是我们想要添加到我们的`Array`中的任何值。在我们的例子中，我们用值`’phantom menace’`作为唯一的参数(`myOwnArray.push(‘phantom menace’)`)来调用`push`方法。

在我们的`push`方法中，我们为`data`字段分配了一个键值对。

为了分配键值，我们使用括号符号`[]`中的`length`字段值。

接下来，我们给`item`赋值

我们用`1`增加`length`字段的值，用`length`的值增加`return`

> 注意:你注意到我增加了这个`MyOwnArray`类中的`length`字段了吗？这解释了为什么最后一个索引位置和您的长度总是有一个差异`1`

让我给你看一个例子:

可以看到，我们把`starWarsMovies` `Array`用了 6 项。当我们`console.log`长度时，它返回`6`如我们所料。当我们试图检索第 6 个索引位置的值时会发生什么？我们得到`undefined`。这是因为我们总是在向`Array`添加一个项目后递增`length`。

## 得到

接下来，我们创建一个标识符为`get`的方法。这个方法将负责从我们的`data`字段返回值。

我们的`get`方法只有一个参数`index`。在我们的`get`方法中，我们使用`index`参数和括号符号`[]`来`return`来自`data`字段的值。

在我们的例子中，我们想要检索索引位置`0` ( `myOwnArray.get(0)`)的值

## 流行音乐

接下来，我们用标识符`pop`创建一个方法。正如您可能怀疑的那样，这个方法将负责移除一个`Array`中的最后一个*项*。这个方法没有参数。

在我们的`pop`方法中，我们使用`const`关键字创建一个带有标识符`lastItem`的新变量。你大概能猜到我们会用这个做什么。我们使用括号符号`[]`和字段`length`的值(递减 1)来提取字段`data`中最后一项的值。

由于`data`是一个对象，我们可以使用`delete`操作符，后跟`data`对象中最后一项的属性来移除它。

我们希望确保将字段`length`的值减少`1`，然后返回`lastItem`的值。

# 概括起来

我希望你能和我一样，深入了解他们的工作方法和内幕。现在，我们对如何利用这些重要数据结构的能力有了更深刻的理解。下周我将讨论散列表。等不及了，到时候见！