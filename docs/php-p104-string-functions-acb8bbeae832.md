# PHP — P104:字符串函数

> 原文：<https://blog.devgenius.io/php-p104-string-functions-acb8bbeae832?source=collection_archive---------6----------------------->

![](img/2627847000c09a928ea65ac49fd0c694.png)

这篇文章是关于字符串函数的。这些都是简单的内置函数，几乎不需要掌握。它们的命名很方便，以简化操作要完成的内容。在我们完成字符串函数之后，我们将继续学习数组函数，它也一样简单。

*   `[trim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9),` `[ltrim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`，`[rtrim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `[htmlspecialchars()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `[__call()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `[preg_match()](https://medium.com/dev-genius/php-p102-preg-match-and-regular-expressions-b4b23869261a)`
*   `[filter_var()](https://medium.com/dev-genius/php-p103-filter-var-built-in-function-b21c186a24e9)`
*   `**addslashes()**`
*   `**str_replace()**`
*   `**strlen()**`
*   `**strtolower()**`
*   `**strtoupper()**`
*   `**ucfirst()**`
*   `**strpos()**` **、** `**stripos()**` **、** `**strrpos()**` **、** `**strripos()**`
*   数组函数有:`array_chunk()`、`array_diff()`、`array_key_exists()`、`array_key_first()`、`array_map()`、`array_merge()`、`array_push()`、`array_sum()`、`asort()`、`arsort()`、`count()`、`in_array()`、`ksort()`、`krsort()`、`sort()`、`rsort()`、`shuffle()`、`sizeof()`、`is_array()`、`explode()`、`implode()`
*   神奇的方法有:`__invoke()`、`__toString()`

# `**addslashes()**`

从这个列表开始是`addslashes`功能。它会查看一个字符串，并在以下字符中添加反斜杠:

*   单引号(`'`)
*   双引号(`"`)
*   反斜杠(`\`)
*   NUL(NUL 字节)

这些都是需要转义的字符。

```
<?php
// AddSlashes
var_dump( addslashes("Hello there. What's up?") );
var_dump( addslashes('She said, "I am not interested."') );
var_dump( addslashes('c:\users\dino"') );
```

```
/app/98 Functions/index.php:24:string 'Hello there. What\'s up?' (length=24)
/app/98 Functions/index.php:25:string 'She said, \"I am not interested.\"' (length=34)
/app/98 Functions/index.php:26:string 'c:\\users\\dino\"' (length=17)
```

对`addslashes`功能没有太多要求。

# str_replace()

这是一个经常使用的功能。它在字符串中搜索一个特定的值，并用其他值替换它。

```
str_replace(
    array|string $search,
    array|string $replace,
    string|array $subject,
    int &$count = null
): string|array
```

`$search`和`$replace`参数可以是字符串或数组。让我们来看几个例子。

```
<?php
var_dump( str_replace("Hello", "Hey", "Hello, what's going on?") );
```

上面的代码搜索`Hello`并替换为`Hey`。

```
/app/98 Functions/index.php:29:string 'Hey, what's going on?' (length=21)
```

让我们看一个只有一个数组的例子。

```
<?php
$search = ["Hello", "Hey", "Hi"];
var_dump( str_replace($search, "Good Morning", "Hey There. How are you? I said Hello.") );
```

我们正在寻找`Hello`、`Hey`和`Hi`。如果这些搜索匹配，用`Good Morning`替换它们。

```
/app/98 Functions/index.php:32:string 'Good Morning There. How are you? I said Good Morning.' (length=53)
```

我们还可以进行一对一的阵列替换。我的意思是你有两个数组。搜索数组中的索引 0 将替换替换数组中的索引 0。

```
<?php
$search  = ["Hello", "Hey", "Hi"];
$replace = ["Good Morning", "Good Day", "Good Evening"];
var_dump( str_replace($search, $replace, "Hey There. How are you? I said Hello.") );
```

```
<?php
/app/98 Functions/index.php:36:string 'Good Day There. How are you? I said Good Morning.' (length=49)
```

# 斯特伦()

`str_len`函数只是计算字符串的长度。记住这不是一个数组，所以如果你有一个字符，长度将是 1。我不知道为什么会经常混淆。

```
<?php
var_dump( strlen("Dino") ); // 4
```

# strtolower()

函数的作用是:将一个字符串转换成小写字符。

```
<?php
// strtolower
var_dump( strtolower("Dino") ); //dino
var_dump( strtolower("DINO") ); //dino
```

# strtoupper()

和`strtolower`正好相反。它只是将所有内容转换成大写字符。

```
<?php
// strtoupper
var_dump( strtoupper("Dino") ); //DINO
var_dump( strtoupper("dino") ); //DINO
```

# ucfirst()

将字符串中的第一个字符转换为大写字符。一个常见的错误是，假设每个句子的开头都被转换为大写字符。不是这样的。只有字符串中的第一个字符大写。

```
<?php
// ucfirst
var_dump( ucfirst("hey there. how's it going?") );
```

```
/app/98 Functions/index.php:50:string 'Hey there. how's it going?' (length=26)
```

# strpos()、stripos()、strrpos()、strripos()

PHP 中常用的字符串函数的最后一位。`strpos`功能是面试中最受欢迎的功能。`strpos`查找子字符串在字符串中第一次出现的位置。它将返回子串或`false`的索引值。

```
<?php
// strpos
var_dump( strpos("Hi there.", "Hi") );    // 0
var_dump( strpos("Hi there.", "there") ); // 3
var_dump( strpos("Hi there.", "ere") );   // 5
var_dump( strpos("Hi there.", "frank") ); // false
```

指针`Hi`位于索引 0 处，因此它返回`0`。如果你记得的话，值`0`是 falsy。检查子字符串是否存在的正确方法是使用 identity 运算符。我们先来看错误的方式。

```
<?php
if ( strpos("Hi there", "Hi") != false ) {
    echo "Found a match";
} else {
    echo "Didn't find a match";
}
```

在这个例子中，我们得到了`Didn’t find a match`，即使我们已经得到了。记住，返回的是索引 0，它是 falsy。现在，让我们来看看这样做的正确方法。

```
<?php
if ( strpos("Hi there", "Hi") !== false ) {
    echo "Found a match";
} else {
    echo "Didn't find a match";
}
```

使用标识操作符`!==`，函数必须返回布尔值`false`才能失败。

`strpos`区分大小写。

```
<?php
var_dump( strpos("Hi there.", "hi") );
```

上面的例子返回`false`。为了不区分大小写，请使用`stripos`。

```
<?php
var_dump( stripos("Hi there.", "hi") );
```

这一次，我们得到`0`。

`strrpos`在字符串中搜索子字符串的最后一个匹配项。

```
<?php
var_dump( strrpos("Hi there. Hi.", "Hi") ); // Returns 10 instead of 0.
```

它区分大小写。如果你想让它不区分大小写，你可以使用`strripos`。

```
<?php
var_dump( strrpos("Hi there. Hi.", "hi") );  // false
var_dump( strripos("Hi there. Hi.", "hi") ); // 10
```

这就是字符串函数。接下来，数组函数。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/1d9846b6e7fabf79a5a127e7dc40e664.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 负责人，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。