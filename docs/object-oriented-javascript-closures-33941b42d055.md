# 面向对象的 JavaScript——闭包

> 原文：<https://blog.devgenius.io/object-oriented-javascript-closures-33941b42d055?source=collection_archive---------7----------------------->

![](img/0f81afb1ea0e137a32131f4252b293b5.png)

照片由[画框哈里拉克](https://unsplash.com/@framemily?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在这篇文章中，我们将看看函数。

# 返回函数的函数

函数可以返回函数。

例如，我们可以写:

```
function a() {
  console.log('foo');
  return function() {
    console.log('bar');
  };
}
```

我们返回一个函数，然后我们可以调用我们返回的函数。

例如，我们可以写:

```
a()();
```

然后首先调用`a`，所以`'foo'`被记录。

然后我们用第二个圆括号调用返回的函数，这样我们就记录了`'bar'`。

# 自我重写的功能

我们应该意识到函数可以自我重写。

例如，我们可以写:

```
function a() {
  console.log('foo');
  a = function() {
    console.log('bar');
  };
}
```

然后我们得到`'foo'`日志。

运行第一个控制台日志，然后重新分配该功能。

我们不应该这样做，如果我们这样做，棉绒会提醒我们。

# 关闭

闭包是内部有其他函数的函数。

内部函数可以访问外部函数的范围。

例如，如果我们有:

```
function outer() {
  let outerLocal = 2; function inner() {
    let innerLocal = 3;
    return outerLocal + innerLocal;
  }
  return inner();
}
```

我们有带变量的`inner`函数。

这仅在`inner`功能中可用。

它还可以访问`outer`函数的`outerLocal`变量。

这很有用，因为我们希望在代码中有私有变量。

我们有`outerLocal`和`innerLocal`只有`inner`才有。

`outer`无法访问`inner`的变量。

因此，我们对这些功能有不同的隐私级别。

# 循环中的闭包

如果我们有一个如下所示的循环:

```
function foo() {
  var arr = [],
    i;
  for (i = 0; i < 3; i++) {
    arr[i] = () => i
  }
  return arr;
}
```

如果我们称之为:

```
const arr = foo();
for (const a of arr){
 console.log(a());
}
```

然后我们从所有控制台日志中得到 3。

这是因为当我们将添加的函数分配给`arr`时`i`是 3。

`var`不是块范围的，所以我们将得到`i`的最后一个值，而不是循环中的值。

为了从我们分配的函数的循环标题中得到`i`的值，我们写:

```
function foo() {
  var arr = [],
    i;
  for (i = 0; i < 3; i++) {
    ((j) => {
      arr[i] = () => j
    })(i)
  }
  return arr;
}
```

然后我们得到 0，1，和 2，如我们所料。

我们也可以用`let`替换`var`，使`i`成为块范围的，从而避免这个问题。

# Getter 和 Setter

Getter 函数返回值，setter 函数设置值。

我们可以把 getter 和 setter 函数放在一个闭包里，这样我们就可以保持它们的私有性。

例如，我们可以写:

```
let getValue, setValue;
(() => {
  let secretVal = 0;
  getValue = () => {
    return secretVal;
  }; setValue = (v) => {
    if (typeof v === "number") {
      secretVal = v;
    }
  };
}());
```

我们运行一个函数来给`getValue`和`setValue`函数赋值。

`getValue`返回`secretVal`的值，`setValue`设置它。

这样，我们就能保守`secretVal`的秘密。

![](img/32244ccc944f754d4da306244c8955a5.png)

照片由[李中清](https://unsplash.com/@picsbyjameslee?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

闭包适用于各种应用程序。

这主要是为了保持变量私有，我们也可以用它们按照我们期望的方式绑定变量值。