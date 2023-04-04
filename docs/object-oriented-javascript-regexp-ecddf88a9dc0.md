# 面向对象的 JavaScript — RegExp

> 原文：<https://blog.devgenius.io/object-oriented-javascript-regexp-ecddf88a9dc0?source=collection_archive---------2----------------------->

![](img/f56ca3dd0844d567bfccb56ad758c352.png)

Todd Quackenbush 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将看看`RegExp` 对象。

# 正则表达式

`RegExp`构造函数让我们搜索和操作文本。

JavaScript 使用 Perl 5 语法来定义 regex。

正则表达式由使用和匹配文本的模式组成。

它可以有选择地包含零个或多个修饰符，以提供更多关于应该如何使用模式的说明。

例如，我们可以通过编写以下代码来使用`RegExp`构造函数:

```
const re = new RegExp("c.*t");
```

我们有一个正则表达式，它匹配一个单词和在`c`和`t`之间的一些字母。

`.`表示任何字符。

`*`意为在其后的事情之前。

我们可以添加一些称为标志的修饰符来改变正则表达式。

一些标志包括:

*   `g`为全局。
*   `i`用于忽略大小写
*   `m`对于多线

所以如果我们有:

```
const re = new RegExp("c.*t", 'gmi');
```

我们可以检查它是否与`global`属性全局匹配。

所以`re.global`应该返回`true`。

无法设置该属性，因此为其赋值不会改变值。

# 正则表达式方法

`RegExp`实例有一些方法。

它们包括`test`和`exec`方法。

`test`检查字符串是否匹配正则表达式模式。

并且`exec`返回匹配正则表达式的字符串。

我们可以通过运行调用`test`；

```
/c.*t/.test("CoffeeScript");
```

然后我们得到`false`。

但是我们称之为:

```
/c.*t/i.test("CoffeeScript");
```

然后由于不区分大小写的匹配，我们得到`true`。

为了给`exec`打电话，我们可以写:

```
/c.*t/i.exec("CoffeesScript")[0];
```

然后我们得到:

```
"CoffeesScript"
```

# 接受正则表达式作为参数的字符串方法

一些字符串方法接受正则表达式作为参数。

它们包括:

*   `match` —返回匹配项的数组
*   `search` —返回第一次搜索的位置
*   `replace` —用另一个字符串替换匹配的文本
*   `split` —将字符串拆分为元素数组时接受正则表达式

我们可以向`search`和`match`传递一个正则表达式。

例如，我们可以写:

```
'JavaScript'.match(/a/);
```

然后我们得到`['a']`。

如果我们有”

```
'JavaScript'.match(/a/g);
```

然后我们得到:

```
["a", "a"]
```

如果我们写:

```
'JavaScript'.match(/J.*a/g);
```

然后我们得到:

```
["Java"]
```

如果我们叫`search`:

```
'JavaScript'.search(/J.*a/g);
```

然后我们得到匹配字符串的位置，为 0。

`replace`方法让我们用其他字符串替换匹配的文本。

例如，我们可以通过编写以下内容来删除所有小写字母:

```
'JavaScript'.replace(/[a-z]/g, '')
```

然后我们得到:

```
"JS"
```

我们还可以使用`$&`占位符来给匹配添加内容。

例如，我们可以写:

```
'JavaScript'.replace(/[A-Z]/g, '-$&')
```

然后我们得到:

```
"-Java-Script"
```

可以接受一个回调，让我们返回我们处理过的匹配。

例如，我们可以写:

```
'JavaScript'.replace(/[A-Z]/g, (match) => {
  return `-${match.toLowerCase()}`;
})
```

然后我们得到:

```
"-java-script"
```

我们用小写字母替换了大写字母，并在它前面加了一个破折号。

回调的第一个参数是`match.`

最后是被搜索的字符串。

倒数第二个是`match`的位置。

其余的参数包含与我们的 regex 模式中的任何组匹配的任何字符串。

方法让我们分割一个字符串。

它需要一个带有[模式的正则表达式来进行拆分。

例如，我们写道:

```
const csv = 'one, two,three ,four'.split(/\s*,\s*/);
```

然后我们得到:

```
["one", "two", "three", "four"]
```

我们用空格分割字符串来得到它。

![](img/b467de58cceb99f814d6fe83ac30ea2b.png)

Victor Grabarczyk 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

`RegExp`构造函数让我们找到字符串中的模式并使用它们。