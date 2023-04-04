# 在 JavaScript 中执行数字转换

> 原文：<https://blog.devgenius.io/performing-number-conversions-in-javascript-9b172f541e7?source=collection_archive---------11----------------------->

## 了解如何在 JavaScript 中转换数字

![](img/624a4a7b6077c9bf4237d4adfe8bc284.png)

## 介绍

当我们在 JavaScript 中处理数字时，当我们希望以某种方式转换数字时，我们有三个选项可用。我之前写过一篇关于使用 [parseInt](https://javascript.plainenglish.io/what-is-parseint-in-javascript-5d1aba3fdf9c) ()的文章。另外两个选项是 parseFloat()和 Number()函数。

## parseInt() & parseFloat()

parseInt()和 parseFloat()函数专门用于将字符串转换成数字。使用字符串时，通常最好使用 parseInt()或 parseFloat()，因为它们更简单。下面是一个使用 parseInt()的简单例子。

```
parseInt("100");
//Returns ---> 100
```

parseFloat()的工作方式与 parseInt()类似。字符串将被转换，直到它达到一个非浮点值或到达字符串的末尾。如果在字符串中找不到数值，或者字符串中的第一个字符不是数字，则返回 NaN(不是数字)。让我们看一个例子:

```
parseFloat("100");
//Returns ---> 100parseFloat("10.1");
//Returns ---> 10.1parseFloat("a1");
//Returns ---> NaN
```

我前面提到过，如果字符串包含一个非浮点值，那么转换将会停止。这意味着如果字符串包含两位小数，则只返回第一位。让我们来看一个例子。

```
parseFloat("10.1.4");
//Returns ---> 10.1
```

使用 parseFloat()时的另一个考虑是，如果不使用小数，初始零值将被忽略。

```
parseFloat("0.1");
//Returns ---> 0.1parseFloat("01");
//Returns ---> 1
```

现在我们已经看了 parseFloat()，让我们继续看如何使用 Number()函数。

## 数字()

使用 Number()函数转换数字有各种各样的复杂性。我们将从一些基本的数字转换开始。当你向函数传递一个数值时，它将简单地返回这个数字。

```
Number(1);
//Returns ---> 1Number(80);
//Returns ---> 80Number(1000);
//Returns ---> 1000
```

如果传入 Null 或 False，则返回零。

```
Number(false);
//Returns ---> 0Number(null);
//Returns ---> 0 
```

如果您传入布尔值 True，那么将返回一个。

```
Number(true);
//Returns ---> 1
```

如果传入未定义的值或未定义的值，则返回 NaN。

```
Number(undefined);
//Returns ---> NaNlet he;
Number(he);
//Returns ---> NaN
```

当你在处理字符串时，如果你传入一个基本的数字，转换将按预期进行。

```
Number("1");
//Returns ---> 1Number("100");
//Returns ---> 100Number("18");
//Returns 18
```

如果字符串以零开始，那么它将被忽略。

```
Number("01");
//Returns ---> 1
```

十进制值也以类似的方式工作。

```
Number("1.1");
//Returns ---> 1.1Number("1.1.1");
//Returns ---> NaNNumber("0.1");
//Returns ---> 0.1
```

如果你传入一个空字符串，那么你将得到零。

```
Number("");
//Returns ---> 0Number(" ");
//Returns 0
```

十六进制被转换成十六进制等效值。

```
Number("0xa");
10
```

我希望你喜欢这篇文章。请随时发表任何评论、问题或反馈，并关注我以获取更多内容！