# 近年来最受期待的 7 大 JS 特性

> 原文：<https://blog.devgenius.io/top-7-most-expected-js-features-in-recent-years-54b8ef775182?source=collection_archive---------6----------------------->

![](img/421f52763b8900905c63b8bddec05e05.png)

自从 ECMAScript 2015 发布以来，在 JS 中跟踪新特性变得越来越困难。

在本文中，我想谈谈社区最期待的 7 个特性。

让我们潜水吧！

# 1.动态导入

这个函数增加了一个`import(specifier)`句法形式，在很多方面都像一个函数(见下文)。它返回所请求模块的模块命名空间对象的承诺，该对象是在获取、实例化和评估所有模块的依赖关系以及模块本身之后创建的。

在这里您可以看到`import()`如何在一个非常简单的单页应用程序中通过导航实现延迟加载模块:

*   `import()`可以从脚本中使用，而不仅仅是从模块中。
*   如果`import()`用于一个模块中，则它可以在任何位置、任何级别出现，而不是被吊起。
*   `import()`接受任意字符串(这里显示了运行时确定的模板字符串)，而不仅仅是静态字符串。
*   模块中`import()`的存在不会建立依赖关系，在评估包含模块之前必须提取并评估该依赖关系。

多看:[https://github.com/tc39/proposal-dynamic-import](https://github.com/tc39/proposal-dynamic-import)

# 2.异步函数

有一种特殊的语法可以以更舒适的方式处理承诺，称为“异步/等待”。函数前的“async”一词有一个简单的含义:函数总是返回一个承诺。其他值会自动包装在已解决的承诺中。关键字`await`使 JavaScript 等待，直到承诺实现并返回结果。

## 错误处理

如果承诺正常达成，则`await promise`返回结果。但在被拒绝的情况下，它会抛出错误，就像有一条`throw`语句一样。在现实生活中，承诺可能需要一段时间才会被拒绝。在这种情况下，在`await`抛出错误之前会有一段延迟。我们可以使用`try..catch`捕捉到这个错误，就像常规的`throw`一样:

多看:[https://github.com/tc39/ecmascript-asyncawait](https://github.com/tc39/ecmascript-asyncawait)

# 3.异步迭代器

## 异步迭代器和异步 iterables

异步迭代器很像迭代器，除了它的`next()`方法返回一个对`{ value, done }`的承诺。如上所述，我们必须为迭代器结果对返回一个承诺，因为迭代器的下一个值和“完成”状态在迭代器方法返回时都是未知的。

## 异步迭代语句:`for` - `await` - `of`

引入了一个`for-of`迭代语句的变体，它迭代异步可迭代对象。

## 异步发电机功能

异步生成器函数类似于生成器函数，但有以下区别:

*   当被调用时，异步生成器函数返回一个对象，一个*异步生成器*，其方法(`next`、`throw`和`return`)返回对`{ value, done }`的承诺，而不是直接返回`{ value, done }`。这将自动生成返回的异步生成器对象*异步迭代器*。
*   允许使用`await`表达式和`for` - `await` - `of`语句。
*   修改了`yield*`的行为，以支持对异步可迭代对象的委托。

然后，该函数返回一个异步生成器对象，该对象可由前面的示例中所示的`for` - `await` - `of`使用。

更多阅读:[https://github.com/tc39/proposal-async-iteration](https://github.com/tc39/proposal-async-iteration)

# 4.对象静止/扩散属性

ECMAScript 6 为数组析构赋值引入了 [rest 元素](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)，为数组文字引入了 [spread 元素](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)。

这个特性为对象析构赋值引入了类似的 [rest 属性](https://github.com/tc39/proposal-object-rest-spread/blob/master/Rest.md)，为对象字面量引入了 [spread 属性](https://github.com/tc39/proposal-object-rest-spread/blob/master/Spread.md)。

## 剩余属性

Rest 属性收集剩余的自己的可枚举属性键，这些键还没有被析构模式挑选出来。这些键和它们的值被复制到一个新的对象上。

## 传播属性

在对象初始化器中传播属性将自己的可枚举属性从提供的对象复制到新创建的对象上。

更多阅读:[https://github.com/tc39/proposal-object-rest-spread](https://github.com/tc39/proposal-object-rest-spread)

# 5.可选链接

可选的链接操作符拼写为`?.`。它可能出现在三个位置:

如果`?.`运算符左侧的操作数计算结果为 undefined 或 null，则表达式计算结果为 undefined。否则，正常触发目标属性访问、方法或函数调用。

更多阅读:【https://github.com/tc39/proposal-optional-chaining 

# 6.无效合并

如果`??`运算符左侧的表达式计算结果为 undefined 或 null，则返回其右侧的表达式。

更多阅读:[https://github.com/tc39/proposal-nullish-coalescing](https://github.com/tc39/proposal-nullish-coalescing)

# 7.BigInt

`BigInt`是一个原语，它提供了一种表示大于 253 的整数的方法，253 是 Javascript 可以用`Number`原语可靠表示的最大数字

## 句法

通过将`n`追加到整数的末尾或者通过调用构造函数来创建`BigInt`。

## 经营者

你可以用`+`、`*`、`-`、`**`和`%`搭配`BigInt` s，就像搭配`Number` s 一样。

`/`操作符也像预期那样处理整数。但是，由于这些是`BigInt`而不是`BigDecimal`的，这个操作将向 0 舍入，也就是说，它不会返回任何小数。

## 比较和条件句

一个`BigInt`并不严格等于一个`Number`，但它是宽松的。

更多阅读:[https://github.com/tc39/proposal-bigint](https://github.com/tc39/proposal-bigint)

你可以在这里阅读更多关于 2015 年以来所有新功能的信息[https://github . com/tc39/proposals/blob/master/finished-proposals . MD](https://github.com/tc39/proposals/blob/master/finished-proposals.md)

P.S .如果你想知道最期待的特性——请阅读我的下一篇文章！