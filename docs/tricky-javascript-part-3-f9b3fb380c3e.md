# 棘手的 JavaScript:第 3 部分

> 原文：<https://blog.devgenius.io/tricky-javascript-part-3-f9b3fb380c3e?source=collection_archive---------6----------------------->

到目前为止，我们已经看到了很多例子和解释，每个例子都向我们展示了 JavaScript 是多么令人困惑。

今天我们将会看到更多这样的例子。我知道，这些例子会给你带来困难，但是不要担心，这些解释会让你有一个清晰的理解。

![](img/22c2b6818b2849f389e690a618239897.png)

由[麦克斯韦·纳尔逊](https://unsplash.com/@maxcodes?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 有趣的数学

你可能知道这些类型的数学运算

```
 3  - 1  // -> 2
 3  + 1  // -> 4
'3' - 1  // -> 2
'3' + 1  // -> '31'
```

但是你了解这些手术吗..？？？？

```
'' + '' // -> ''
[] + [] // -> ''
{} + [] // -> 0
[] + {} // -> '[object Object]'
{} + {} // -> '[object Object][object Object]'

'222' - -'111' // -> 333

[4] * [4]       // -> 16
[] * []         // -> 0
[4, 4] * [4, 4] // NaN
```

让我们来理解一下数字加法。这里有一个小表格，可以用来理解 JavaScript 中的加法:

```
Number  + Number  -> addition
Boolean + Number  -> addition
Boolean + Boolean -> addition
Number  + String  -> concatenation
String  + Boolean -> concatenation
String  + String  -> concatenation
```

但是`[]`和`{}`呢。在添加之前，`[]`和`{}`的`ToPrimitive`和`ToString`方法被隐式调用。

值得注意的是，`{} + []`这里是个例外。之所以和`[] + {}`不一样，是因为没有括号，先解释为代码块，再解释为一元+，把`[]`转换为数字。它看到以下内容:

```
{
  // a code block here
}
+[]; // -> 0
```

为了获得与`[] + {}`相同的输出，我们可以用括号将其括起来。

```
({} + []); // -> [object Object]
```

非常令人困惑，对吗..？这有时会造成 JavaScript 的信任问题。

要了解更多，请看看 ECMA 脚本标准。

# 点和扩散

众所周知，当一个对象或数组中的所有元素都需要包含在某种列表中时，可以使用 spread `...`语法。

那么下面代码的输出是什么呢？？

```
[...'...']Answer : ['.' , '.' , '.'] 
```

答案很简单，spread 操作符将字符串`'...'`扩展到字符`'.'`的数组中。这是一个简单的例子。那么下面这个例子的输出是什么呢？

```
[...[...[...[...'...']]]]   ->  [".", ".", "."]
```

是不是看起来不对？甚至经过很多传播运营商，答案都是一样的！！

先试试，再回来解释。

我们一步一步来解决吧。

```
[...[...[...[...'...']]]]Step 1 : [...'...'] 
We have already seen how it is solved. So we can direactly replace this with ['.' , '.' , '.'].Output After Step 1:
[...[...[...['.' , '.' , '.']]]]Step 2 :If we look at the [...['.' , '.' , '.']] again it is spead operator.
Again we are going to replace with ['.' , '.' , '.'].Output After Step 1:
[...[...['.' , '.' , '.']]]Now if we look closely next two steps are as same as step 2.There for the answer is ['.' , '.' , '.'].
```

# 狡猾的回报

考虑下面的例子:

```
(function() {
  return
  {
    b: 10;
  }
})(); Answer : undefined
```

答案是错的，不是吗？

这是因为**自动分号插入。**这个 JavaScript 概念会自动添加一个分号。

正因为如此，return 关键字后面直接加了一个分号。更正后的代码:

```
(function() {
  return {
    b: 10
  };
})(); // -> { b: 10 }
```

# 巧妙的箭头功能

请考虑以下情况:

```
Example 1 :
let f = () => 10;
f(); // -> 10Example 2:
let f = () => {};
f(); // -> undefined
```

第一个示例返回正确的值 10，这意味着函数执行正确。

在第二个例子中，我们期望函数返回一个空对象，但是它返回了`undefined`。

这是因为`{}`是箭头函数语法的一部分。为了避免这种情况，我们可以使用`()`。

```
let f = () => ({});
f(); // -> {}
```

# 箭头函数不能是构造函数

这是常规的函数声明。

```
let f = function() {
  this.a = 1;
};
new f(); // -> f { 'a': 1 }
```

这是箭头函数声明。

```
let f = () => {
  this.a = 1;
};
new f(); // -> TypeError: f is not a constructor
```

当你使用一个常规函数时，它可以访问`this`。因此它们被用来创建构造函数。

但是使用箭头功能，它不能访问`this`。导致引发异常。

伙计们，这部分到此为止。如果您错过了第 1 部分和第 2 部分。链接在下面。

[](https://pratik-das.medium.com/tricky-javascript-part-1-bafb9694ec87) [## 棘手的 JavaScript:第 1 部分

### JavaScript 是一种非常流行的语言。它很容易学习和工作。大多数 web 开发人员的职业生涯始于…

pratik-das.medium.com](https://pratik-das.medium.com/tricky-javascript-part-1-bafb9694ec87) [](https://pratik-das.medium.com/tricky-javascript-part-2-82fad7adfbd9) [## 棘手的 JavaScript:第 2 部分

### 嗨，伙计们，欢迎回到 Ticky JavaScript 的第二部分。如果您是新用户，请查看第 1 部分。下面的链接⏬

pratik-das.medium.com](https://pratik-das.medium.com/tricky-javascript-part-2-82fad7adfbd9) 

请分享给你的朋友。让他们知道 JavaScript 如何欺骗你或欺骗你。让他们意识到。

如果你对 JavaScript 感兴趣，请订阅我，我会通知你新的博客。

在那之前，继续学习。