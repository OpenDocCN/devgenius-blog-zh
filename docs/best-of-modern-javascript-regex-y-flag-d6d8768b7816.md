# 现代 JavaScript 的精华—正则表达式 y 标志

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-regex-y-flag-d6d8768b7816?source=collection_archive---------5----------------------->

![](img/8127b57918600d4fecbb3fa9bc504892.png)

斯蒂芬·莱昂纳迪在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript regex 的新特性。

# 搜索和 y 标志

`String.prototype.search`方法忽略了`y`标志。

`lastIndex`不为它所改变。

例如，如果我们有:

```
const REGEX = /foo/y;REGEX.lastIndex = 8;
const match = 'barfoobarfoo'.search(REGEX);
console.log(match.index);
console.log(REGEX.lastIndex);
```

然后`match`美国`undefined`和`lastIndex`停留在 8。

我们只能用它从字符串的开头开始搜索。

根据`g`标志是否添加了`y`,`String.prototype.match`的工作方式不同。

如果`g`不是，就像`exec`一样工作。

如果设置了`g`，那么它返回一个包含匹配的字符串部分的数组或`null`。

例如，如果我们写:

```
const REGEX = /a/;REGEX.lastIndex = 7;
console.log('abab'.match(REGEX).index); 
console.log(REGEX.lastIndex);
```

然后`index`为 0，`lastIndex`为 7。

如果没有添加标志，它将获得第一个。

如果我们添加`y`标志:

```
const REGEX = /a/y;REGEX.lastIndex = 2;
console.log('abab'.match(REGEX).index); 
console.log(REGEX.lastIndex);
```

那么只有当搜索从`lastIndex`开始时才返回匹配。

索引必须准确。

`lastIndex`找到匹配后将被更新。

如果设置了`g`标志，那么`match`将匹配所有子字符串，并将它们放入数组中。

例如，我们可以写:

```
const REGEX = /a|b/g;REGEX.lastIndex = 0;
console.log('abab'.match(REGEX)); 
console.log(REGEX.lastIndex);
```

并且`match`返回`[“a”, “b”, “a”, “b”]`。

`lastIndex`仍为 0。

`lastIndex`被忽略，启用`g`标志。

添加`gy`标志后，结果相同。

`match`和`lastIndex`都给我们同样的结果。

# `Split Strings`

方法让我们拆分一个字符串。

`y`标志将改变方法的行为。

例如，我们可以写:

```
console.log('abb'.split(/b/y))
console.log('bba'.split(/b/y))
```

字符串将以`b`为分隔符进行拆分。

所以我们得到:

```
["a", "", ""]
```

和

```
["", "", "a"]
```

无论旗帜在什么位置，`y`旗帜都起作用。

我们也可以将模式放在一个捕获组中:

```
console.log('abb'.split(/(b)/y))
```

然后我们得到:

```
["a", "b", "", "b", ""]
```

结果。

# 替换字符串

`String.prototype.replace`方法让我们通过用正则表达式匹配字符串来替换它们。

例如，如果我们写:

```
const REGEX = /a/;
console.log('baba'.replace(REGEX, 'x'));
```

然后我们让`'bxba’`登录。

它只记录第一个匹配的条目。

如果添加了`y`标志，我们也最多得到一个匹配。

但是匹配总是从字符串的开头开始搜索。

例如，如果我们有:

```
const REGEX = /a/y;
REGEX.lastIndex = 2;
console.log('baba'.replace(REGEX, 'x'));
```

然后我们让`'baba’`登录。

如果`lastIndex`超出了第一个匹配，那么它将被忽略，即使有匹配可用。

通过`g`标志，`replace`替换所有匹配。

所以，如果我们有:

```
const REGEX = /a/g;
REGEX.lastIndex = 2;
console.log('baba'.replace(REGEX, 'x'));
```

然后我们得到`'bxbx'`日志。

如果我们组合`g`和`y`标志:

```
const REGEX = /a/gy;
console.log('baba'.replace(REGEX, 'x'));
```

然后从`replace`方法返回`'baba’`。

但是如果我们有:

```
const REGEX = /a/gy;
console.log('aaba'.replace(REGEX, 'x'));
```

然后我们得到`‘xxba’`。这意味着第一个匹配必须在字符串的开头，这样`replace`才能工作。

![](img/930c9d37bea21e924db3793617e1cfa2.png)

阿什利·罗斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

`y`标志是 ES6 中引入的新标志，使用不同的方法会有不同的效果。