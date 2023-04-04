# 更好的 JavaScript——浮点和强制

> 原文：<https://blog.devgenius.io/better-javascript-floating-point-and-coercions-b16ff3a48443?source=collection_archive---------4----------------------->

![](img/8cabcf66ba5d32e6808996f8946a37eb.png)

照片由[利奥·里瓦斯](https://unsplash.com/@leorivas?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 浮点数

由于舍入的原因，浮点数在 JavaScrtipt 中很棘手。

例如，如果我们有:

```
0.1 + 0.2;
```

然后我们得到:

```
0.30000000000000004
```

双精度浮点数只能表示有限的一组数，而不能表示无限的一组实数。

这意味着浮点运算只能产生有限的结果。

当我们在一个或多个操作中舍入时，舍入误差会累积。

这使得结果越来越不准确。

像加法这样的数学运算是结合律，所以(x + y) + z = x + (y + z)。

但是浮点运算却不是这样。

所以如果我们有:

```
(0.1 + 0.2) + 0.3; 
```

我们得到:

```
0.6000000000000001
```

但是:

```
0.1 + (0.2 + 0.3);
```

返回 0.6。

对于整数，我们必须确保每个结果的范围在`-2 ** 53`和`2 ** 53`之间。

# 隐性胁迫

我们必须意识到隐性胁迫。

当我们写 JavaScript 时，它们会成为我们的一个问题。

例如，我们可以写:

```
2 + true;
```

得到 3。

`true`被转换成 1 所以我们得到 3。

有些情况下，如果我们使用了错误的类型，JavaScript 会给出错误。

例如，如果我们有:

```
"foo"(1);
```

我们得到“未捕获的类型错误:“foo”不是函数”。

并且`undefined.x`得到我们‘未捕获的类型错误:无法读取未定义的属性‘x’。

在许多其他情况下，JavaScript 将值强制为它认为应该是的值。

所以如果我们写:

```
2 + 1;
```

然后 JavaScritp 假设我们正在添加 2 个数字。

如果我们写下:

```
"foo" + " bar";
```

然后我们连接两个字符串。

如果我们把一个数字和一个字符串结合起来，那就更混乱了。

如果我们有:

```
1 + 2 + "3";
```

或者

```
(1 + 2) + "3";
```

然后我们得到`'33'`。

但是如果我们有:

```
1 + "2" + 3;
```

我们得到`“123”`。

它只是根据自己的规则转换成类型，所以不太容易预测。

类型强制隐藏了错误，所以只有当代码没有达到我们的预期时，我们才知道我们有一个 bug。

我们没有抛出异常，而是看到了我们没有预料到的结果。

还有，`NaN`被认为不等于自身，比较混乱。

我们必须调用`isNaN`来检查`NaN`。

所以:

```
isNaN(NaN); 
```

返回`true`。

但是:

```
let x = NaN; 
x === NaN;
```

返回`false`。

静默强制使调试变得更加困难，因为它掩盖了错误，使错误更难被发现。

当计算出错时，我们必须检查每个变量，检查哪里出错了。

对象也可以被强制为原语。

所以如果我们有:

```
'' + Math
```

我们得到`“[object Math]”`。

并且:

```
'' + JSON
```

返回`“[object JSON]”`。

![](img/98c5c2219c96e4f87705f3a6daaf61a0.png)

[ahin yesilyaprak](https://unsplash.com/@byadonia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

浮点运算和数据类型强制对 JavaScript 来说都很棘手。

我们必须确保尽可能少地强制数据类型，以减少令人沮丧的错误的机会。