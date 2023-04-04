# PHP — P102: preg_match 和正则表达式

> 原文：<https://blog.devgenius.io/php-p102-preg-match-and-regular-expressions-b4b23869261a?source=collection_archive---------13----------------------->

![](img/1484542ac27c73738c998a9368cb7524.png)

继续使用`preg_match`的内置函数。要回顾前面的功能，请阅读 P101。

[](/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9) [## PHP — P101: trim、htmlspecialchars 和 __call 内置函数

### 在过去的 100 篇 PHP 文章中，我们使用了各种内置函数。是时候把它们收集到一个地方了…

blog.devgenius.io](/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9) 

*   `[trim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9),` `[ltrim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`，`[rtrim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `[htmlspecialchars()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `[__call()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `**preg_match()**`
*   `filter_var()`
*   `addslashes()`
*   `str_replace()`
*   `strlen()`
*   `strtolower()`
*   `strtoupper()`
*   `ucfirst()`
*   `strpos()`、`stripos()`、`strrpos()`、`strripos()`
*   数组函数有:`array_chunk()`、`array_diff()`、`array_key_exists()`、`array_key_first()`、`array_map()`、`array_merge()`、`array_push()`、`array_sum()`、`asort()`、`arsort()`、`count()`、`in_array()`、`ksort()`、`krsort()`、`sort()`、`rsort()`、`shuffle()`、`sizeof()`、`is_array()`、`explode()`、`implode()`
*   神奇的方法有:`__invoke()`、`__toString()`

# 正则表达式简介

我一直在争论是否要讨论这个问题，因为它需要正则表达式的知识。我们希望尽可能全面，所以我们需要包括一些正则表达式的基础知识。正则表达式只是用来在字符串中搜索特定模式的字符序列。

一个简单的模式可能是`/Dino/`，我们在类似于`Hey there Dino. How's it going?`的字符串中搜索`Dino`

什么是正斜杠？这就是所谓的测力计。每个模式都必须以限定符开始和结束。您可以选择自己喜欢的指示器，但它不能是:

*   字母数字字符，即`A`或`3`
*   反斜杠，`\`
*   空白

常用的指示器有:

*   正斜杠，`/`
*   散列符号，`#`
*   波浪号，`~`

指示器的一些例子:

*   `/Dino/`
*   `#Dino#`
*   `%Dino%`
*   `{Dino}`
*   `~Dino~`

为什么不选一个坚持下去呢？简单。可读性。如果你在你的模式中使用了定界符，你将不得不逃离它。假设我们要寻找`Dino/Jeff`。如果你以正斜杠开始，PHP 会认为你的模式中的正斜杠是结束符号。你需要逃离它:`/Dino\/Jeff/`。或者你可以使用另一个分隔符:`#Dino/Jeff#`。这样你就不用转义正斜杠了。

# preg_match()

下面介绍一下 preg_match 函数。我们将查看前两个参数，分别是模式和搜索字符串。了解到目前为止我们所知道的，我们来看一个例子。

```
<?php

$pattern = "/Dino/";
$search_string = "Hey. My name is Dino. How are you?";

var_dump( preg_match($pattern, $search_string) );
```

如果模式在字符串内匹配，结果将是`1`，如果没有匹配，结果将是`0`，如果有错误，结果将是`false`。

```
/app/98 Functions/PregMatch.php:6:int 1
```

我们得到了预期的`1`。模式区分大小写吗？如果我们搜索`/dino/`而不是`/Dino/`，会得到什么？

```
<?php

$pattern = "/dino/";
$search_string = "Hey. My name is Dino. How are you?";

var_dump( preg_match($pattern, $search_string) );
```

结果实际上会是一个`0`。事实上，它是区分大小写的。

第三个参数是存储匹配结果的位置。如果我们在那里包含一个名为`$matches`的变量，它将在那里存储我们所有的匹配。我们不必初始化`$matches`变量。

```
<?php

$pattern = "/Dino/";
$search_string = "Hey. My name is Dino. How are you?";

var_dump( preg_match($pattern, $search_string, $matches) );
var_dump( $matches );
```

函数`preg_match`仍然返回`1`，但是它也将结果存储在`$matches`数组中。

```
/app/98 Functions/PregMatch.php:6:int 1
```

```
/app/98 Functions/PregMatch.php:7:
array (size=1)
  0 => string 'Dino' (length=4)
```

# 常见的正则表达式模式

这里是一些常见正则表达式模式的快速备忘单。我们将在经历这些模式时，看看它们是如何表现的。如果你需要一个完整的角色列表，看看这个可怕的正则表达式备忘单。

[](https://quickref.me/regex) [## 正则表达式备忘单和快速参考

### 正则表达式(regex)的快速参考，包括符号、范围、分组、断言和一些 sa

quickref.me](https://quickref.me/regex) 

## 单个字符:A、b 或 c

方括号表示范围。在第一个例子中，我们将匹配任何字符，不是 a，b，就是 c。

```
$pattern = "/[abc]/";
$search_string = "Hey. My name is Dino. How are you?";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:14:
array (size=1)
  0 => string 'a' (length=1)
```

我们得到一个响应，有一个字符`a`匹配。如果我们做类似`[xyz]`的事情会怎么样？

```
$pattern = "/[xyz]/";
$search_string = "Hey. My name is Dino. How are you?";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

没有`x`，但是有一个`y`，而且是匹配的。

```
/app/98 Functions/PregMatch.php:14:
array (size=1)
  0 => string 'y' (length=1)
```

## [^abc]除 a、b 或 c 之外的任何单个字符

接下来，为了搜索除指定字符之外的任何其他字符，我们可以添加`^`符号。这实际上意味着“不”

```
$pattern = "/[^xyz]/";
$search_string = "Hello there.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:21:
array (size=1)
  0 => string 'H' (length=1)
```

`H`是匹配模式的第一个字母。

## [a-z]a-z 范围内的任何单个字符

我们可以用一个`-`来指定范围。该模式将匹配`a`和`z`之间的任何小写字母。

```
$pattern = "/[a-z]/";
$search_string = "Hello there.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:28:
array (size=1)
  0 => string 'e' (length=1)
```

它跳过了第一个字母`H`，因为它不在我们要寻找的范围内。人们经常这样做，但并不一定要这样做。如果我们只想搜索从`s`到`y`的范围呢？我们可以做到。

```
$pattern = "/[s-y]/";
$search_string = "Hello there.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:34:
array (size=1)
  0 => string 't' (length=1)
```

`t`存在于`there`中，所以我们得到一个肯定的匹配。`b`到`e`和`s`到`y`怎么样？

```
$pattern = "/[b-es-y]/";
$search_string = "Hello there.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

这次`e`与`Hello`匹配。

```
/app/98 Functions/PregMatch.php:34:
array (size=1)
  0 => string 'e' (length=1)
```

## [A-zA-Z]A-Z 或 A-Z 范围内的任何单个字符

大写和小写的字符序列呢？没有太大不同。

```
$pattern = "/[a-zA-Z]/";
$search_string = "Hello there.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:47:
array (size=1)
  0 => string 'H' (length=1)
```

同样，我们不必使用所有的大写和小写字母。

```
$pattern = "/[b-dD-G]/";
$search_string = "My name is Dino.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:47:
array (size=1)
  0 => string 'D' (length=1)
```

## ^线起点

我们刚才不是说了`^`的意思不是吗？我们做了，但那是在括号里面的时候。当它在括号外时，它表示该行的开始。例如，假设我们想要搜索字符串是否以单词`Hello`开头。

```
$pattern = "/Hello/";
$search_string = "Hello there.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

在这个场景中，我们没有使用`^`符号。我们找到匹配的了吗？是的。

```
/app/98 Functions/PregMatch.php:60:
array (size=1)
  0 => string 'Hello' (length=5)
```

如果我们有下面的字符串。`My name is Dino. Hello there.`会搭配吗？是的，再一次。但是如果我们坚持这个字符串以`Hello`开头呢？通常你会打招呼，然后自我介绍。你需要使用`^`符号。

```
$pattern = "/^Hello/";
$search_string = "My name is Dino. Hello there.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

在这种情况下，没有匹配项。

```
/app/98 Functions/PregMatch.php:60:
array (size=0)
  empty
```

如果我们的字符串以`Hello`开头，您将得到一个匹配。

```
$pattern = "/^Hello/";
$search_string = "Hello there. My name is Dino.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:66:
array (size=1)
  0 => string 'Hello' (length=5)
```

## $行尾

`^`的反义词是`$`。这表示匹配出现在字符串的末尾。例如，让我们看看字符串的结尾是否是一个数字。我们将在没有`$`标志的情况下开始。

```
$pattern = "/[0-9]/";
$search_string = "Hello there. I'm 32\. My name is Dino.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

我们找到匹配的了吗？当然了。

```
/app/98 Functions/PregMatch.php:73:
array (size=1)
  0 => string '3' (length=1)
```

但是我们需要这个数字出现在字符串的末尾。

```
$pattern = "/[0-9]$/";
$search_string = "Hello there. I'm 32\. My name is Dino.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

这一次，我们没有匹配。如果我们将数字移动到最后，我们将得到适当的响应。

```
$pattern = "/[0-9]$/";
$search_string = "Hello there. My name is Dino. I'm 32";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:73:
array (size=1)
  0 => string '2' (length=1)
```

您可以看到这次匹配了`2`,因为它正在查找字符串中的最后一个数字。

你可能已经注意到我去掉了句号(`.`)，这是一个很好的观察。我们将在接下来讨论这个问题。

## 。任何单个字符

没错。句点匹配任何单个字符。让我们再看一次同样的例子，看看如果我们把句号放进去会发生什么。

```
$pattern = "/[0-9].$/";
$search_string = "Hello there. My name is Dino. I'm 32.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

它似乎在做它想做的事。它匹配的是行尾的句号`[0-9]`。我们应该得到`2.`，我们做到了。

```
/app/98 Functions/PregMatch.php:86:
array (size=1)
  0 => string '2.' (length=2)
```

但这不是匹配字符串末尾句点的方法。如果我们遗漏了字符串中的句号呢？什么比赛？

```
$pattern = "/[0-9].$/";
$search_string = "Hello there. My name is Dino. I'm 32";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:92:
array (size=1)
  0 => string '32' (length=2)
```

是`32`？那`32`怎么样？记住，`.`匹配任何字符。因此，我们匹配字符串末尾的任何其他字符的`3`。这意味着，如果我们犯了一个错误，并在一个字符串的末尾添加了后面的`a`，我们仍然可以正确匹配。

```
$pattern = "/[0-9].$/";
$search_string = "Hello there. My name is Dino. I'm 32a";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

在这种情况下，它将匹配`2a`。

```
/app/98 Functions/PregMatch.php:98:
array (size=1)
  0 => string '2a' (length=2)
```

但是，我们需要匹配的时期。它需要以一个数字结尾，后面跟一个实际的句点。我们只需要逃离那个角色。

```
/app/98 Functions/PregMatch.php:110:
array (size=1)
  0 => string '2.' (length=2)
```

## \d 任何数字

我们不必使用模式`[0–9]`来匹配所有的数字。我们可以用`\d`来简化它。为了重新创建上面的例子，我们搜索字符串末尾跟有句点的任何数字，我们可以使用下面的模式:`/\d\.$/`

```
// \d Any digit
$pattern = "/\d\.$/";
$search_string = "Hello there. My name is Dino. I'm 32.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

为了可读性，让我们将分隔符改为`#`。

```
// \d Any digit
$pattern = "#\d\.$#";
$search_string = "Hello there. My name is Dino. I'm 32.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:117:
array (size=1)
  0 => string '2.' (length=2)
```

## \D 任何非数字

我们也可以匹配任何非数字字符。假设我们想接受一个电话号码，但它不能包含除数字以外的任何其他内容。即`5554445555`有效而`555–444–5555`无效。

```
$pattern = "#\D#";
$search_string = "5554445555";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

这将返回一个空的`$matches`数组，这就是我们想要的。

```
/app/98 Functions/PregMatch.php:124:
array (size=0)
  empty
```

如果我们在字符串中添加一些非数字字符，我们的模式将会匹配。

```
$pattern = "#\D#";
$search_string = "555-444-5555";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:130:
array (size=1)
  0 => string '-' (length=1)
```

## \s 任何空白字符

我们能匹配空白字符吗？是的，我们可以。

```
$pattern = "#\s#";
$search_string = "555 444-5555";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:137:
array (size=1)
  0 => string ' ' (length=1)
```

我们可以继续。例如，一个数字后跟一个空格。

```
$pattern = "#\d\s#";
$search_string = "555 444-5555";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:143:
array (size=1)
  0 => string '5 ' (length=2)
```

## \S 任何非空白字符

这将匹配任何不是空格的字符。

```
$pattern = "#\d\S#";
$search_string = "555 444-5555";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

它不会匹配`5`，但会匹配`55`，因为`5`是非空白字符。

```
/app/98 Functions/PregMatch.php:150:
array (size=1)
  0 => string '55' (length=2)
```

记住，它将匹配`44`和`4-`，但它只返回第一个匹配。

## \w 任何单词字符(字母、数字、下划线)

这相当于下面的模式:`[a-zA-Z0-9_]`。这只是一个快捷方式，因为它经常被使用。

```
$pattern = "#\w#";
$search_string = "Hello there. My name is Dino. I'm 32.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:157:
array (size=1)
  0 => string 'H' (length=1)
```

## \W 任何非单词字符

你猜对了。和`\w`正好相反。相当于:`[^a-zA-Z0-9_]`

```
$pattern = "#\W#";
$search_string = "Hello there. My name is Dino. I'm 32.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:164:
array (size=1)
  0 => string ' ' (length=1)
```

## (……)捕捉所附的一切

这就是有趣的地方。我们已经研究了括号中的模式，但是如果我们想要搜索出现的每个实例呢？让我们看看当我们查看我们的`$matches`数组时会发生什么。

```
$pattern = "#(th)#";
$search_string = "Hello there. I'm one of the people here.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:170:
array (size=2)
  0 => string 'th' (length=2)
  1 => string 'th' (length=2)
```

我们找到了两个匹配的。第一个是完整模式，第二个是子模式。在下一节中会更容易解释这一点。

## (a|b) a 或 b

这是这种模式我最喜欢的用法之一。`|`表示或。这款会搭配`a`或者`b`。如果我们想同时匹配`Dino`和`dino`，我们可以使用`(D|d)ino`。完整模式是`Dino`，子模式的第一次出现是`D`。完整模式存储在`$matches[0]`中，第一个匹配的子模式将存储在`$matches[1]`中。

```
$pattern = "/(D|d)ino/";
$search_string = "Hello there. My name is Dino. I'm 32.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:183:
array (size=2)
  0 => string 'Dino' (length=4)
  1 => string 'D' (length=1)
```

完整模式是`Dino`，所以它存储在`$matches[0]`中，第一个出现的子模式是`D`，所以它存储在`1`中。记住`(`表示子模式的开始，而`)`表示子模式的结束。

全模式不匹配怎么办？比如我拼错了自己的名字，在字符串中输入了`Din`。

```
$pattern = "/(D|d)ino/";
$search_string = "Hello there. My name is Din. I'm 32.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:189:
array (size=0)
  empty
```

结果是一个空数组。它没有找到完整的模式，所以也没有找到完整模式中的子模式。字母`D`确实存在，但在字符串中不存在。

我一直说第一子模式，但它是第一完整模式中的子模式。让我们来看看。我将使用一个既有名字`Dino`又有`dino`的字符串。

```
$pattern = "/(D|d)ino/";
$search_string = "Hello there. My name is Dino or dino. I'm 32.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:195:
array (size=2)
  0 => string 'Dino' (length=4)
  1 => string 'D' (length=1)
```

结果还是一样。即使存在带有`Dino`和`dino`的模式，我们也只寻找第一个子模式。我们可以举例说明如何在完整模式中包含多个子模式。

```
$pattern = "/(D|d)(i)no/";
$search_string = "Hello there. My name is Dino or dino. I'm 32.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

我们的全格局是`/(D|d)(i)no/`。意思是，寻找`Dino`或者`dino`。第一个子模式将是`D`或`d`。第二子模式将是`i`。

```
/app/98 Functions/PregMatch.php:201:
array (size=3)
  0 => string 'Dino' (length=4)
  1 => string 'D' (length=1)
  2 => string 'i' (length=1)
```

您现在可以看到，`$matches[0]`包含完全模式匹配，`$matches[1]`包含用于`D`或`d`的第一子模式匹配，`$matches[2]`包含第二子模式匹配`i`。

如果你想知道，顺序很重要。以下模式`/(i)(D|d)no/`与`/(D|d)(i)no/`不同。

## 量词

您可能已经注意到，当我们使用[a-z]时，这意味着只匹配一个字符一次。如果我们想知道一个数字是否重复多次，我们需要不断重复这个模式

美国典型的电话号码遵循以下模式:`3 digits`，接着是`-`，接着是另一个`3 digits`，接着是另一个`-`，最后是`4 digits`。还有更多的东西，但这就是我们将要尝试的，并与我们目前所知道的相匹配。

`555–555–5555`

我们的模式将如下所示:

`#\d\d\d-\d\d\d-\d\d\d\d#`

```
$pattern = "#\d\d\d-\d\d\d-\d\d\d\d#";
$search_string = "Hello there. My name is Dino. My number is 555-444-5555.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:214:
array (size=1)
  0 => string '555-444-5555' (length=12)
```

那有点长。在我们开始简化它之前，我们先来看看量词。

*   `a?`零或一`a`
*   `a*`零个或多个`a`
*   `a+`一个或多个`a`
*   `a{3}`正好是`a`的 3
*   `a{3,}` 3 个以上的`a`
*   `a{3,6}`在`a`的`3`和`6`之间

是时候简化我们的例子了。

```
$pattern = "#\d{3}-\d{3}-\d{4}#";
$search_string = "Hello there. My name is Dino. My number is 555-444-5555.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

我们得到同样的结果。

```
/app/98 Functions/PregMatch.php:220:
array (size=1)
  0 => string '555-444-5555' (length=12)
```

`#\d{3}-\d{3}-\d{4}#`表示正好是`3`位数，后面是`-`，后面是正好是`3`位数，后面是`-`，再后面是另一个`4`位数。

作为本文的结尾，我们将再举一个包含这些内容的例子。

`/[G-I].{4}\s\w*\.\W?\D{1,}\d*-\w*\s.*/`

我知道。有点疯狂。但是我们可以走过它，它应该是简单的。

它说的是:

*   `[G-I]`以 G 和 I 之间的大写字符开始
*   `.{4}`意味着我们接下来要寻找任意 4 个字符。
*   `\s`正在努力寻找下一个空间。
*   `\w*`零次或多次查找任何单词字符
*   `\.`表示我们试图找到一个真实的时期。
*   `\W?`然后继续任何非单词字符，如空格，零次或多次。
*   `\D{1,}`正在搜索至少出现一次的非数字。
*   `\d*`表示我们接下来要搜索一个出现零次或多次的数字。
*   `-`只是一个简单的破折号。
*   `\w*`再次查找任意单词字符零次或更多次。
*   `\s`搜索空格。
*   `.*`零次或多次查找任意字符。

该模式将匹配以下字符串:

`Hello there. My name is Dino. I’m 32-years old. Pretty cool.`

*   `[G-I]`匹配 **H**
*   `.{4}`匹配 **ello**
*   `\s`匹配*(Hello 后空格)*
*   `\w*`匹配**那里**
*   `\.`匹配**。**
*   `\W?`匹配*(空格后有。)*
*   `\D{1,}`火柴**我是 *(*** *空格后是我)*
*   `\d*`火柴 **32**
*   `-`匹配 **-**
*   `\w*`匹配**年**
*   `\s`匹配*(年后空格)*
*   `.*`火柴**旧。相当酷。**

```
$pattern = "#[G-I].{4}\s\w*\.\W?\D{1,}\d*-\w*\s.+#";
$search_string = "Hello there. My name is Dino. I'm 32-years old. Pretty cool.";

preg_match($pattern, $search_string, $matches);
var_dump( $matches );
```

```
/app/98 Functions/PregMatch.php:226:
array (size=1)
  0 => string 'Hello there. My name is Dino. I'm 32-years old. Pretty cool.' (length=60)
```

我认为我们已经涵盖了所有重要的事情。现在，您可以查找任何您喜欢的正则表达式模式，并且能够理解它们，或者更好的是，创建您自己的模式。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/f0e8f84122a3acbee7b0c47ad25a3b65.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 负责人，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。