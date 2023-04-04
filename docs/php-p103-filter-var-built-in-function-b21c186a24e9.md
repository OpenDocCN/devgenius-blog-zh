# PHP — P103: filter_var 内置函数

> 原文：<https://blog.devgenius.io/php-p103-filter-var-built-in-function-b21c186a24e9?source=collection_archive---------3----------------------->

![](img/dd934a94092a8b8ea124c232d05ea53f.png)

我们刚刚大致看完了`preg_match`和正则表达式。有一点你可能很快就意识到了，那就是我们过去是如何验证一封电子邮件是否有效的。现在有一个更简单的方法，就是使用`filter_var`函数。我们不会深入所有的细节，但是让我们探索一下`filter_var`内置函数。

*   `[trim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9),` `[ltrim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`，`[rtrim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `[htmlspecialchars()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `[__call()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `[preg_match()](https://medium.com/dev-genius/php-p102-preg-match-and-regular-expressions-b4b23869261a)`
*   `**filter_var()**`
*   `addslashes()`
*   `str_replace()`
*   `strlen()`
*   `strtolower()`
*   `strtoupper()`
*   `ucfirst()`
*   `strpos()`、`stripos()`、`strrpos()`、`strripos()`
*   数组函数有:`array_chunk()`、`array_diff()`、`array_key_exists()`、`array_key_first()`、`array_map()`、`array_merge()`、`array_push()`、`array_sum()`、`asort()`、`arsort()`、`count()`、`in_array()`、`ksort()`、`krsort()`、`sort()`、`rsort()`、`shuffle()`、`sizeof()`、`is_array()`、`explode()`、`implode()`
*   神奇的方法有:`__invoke()`、`__toString()`

# filter_var()

`filter_var`返回类型是`mixed`,因为它在无效时返回`false`,或者在有效时返回值。第一个参数是我们试图清理或验证的值，第二个参数是过滤器名称。还有第三种说法，但我们不会在这里详细讨论。

```
<?php
var_dump( filter_var("dinoaexample.com", FILTER_VALIDATE_EMAIL) );
```

上面的代码返回`false`，因为电子邮件`dinoaexample.com`不是有效的电子邮件。

```
<?php
var_dump( filter_var("dino@example.com", FILTER_VALIDATE_EMAIL) ); 
```

因为我们有一个有效的电子邮件地址，所以我们得到返回给我们的电子邮件地址:`dino@example.com`。

# 过滤

有大量的内置过滤器。要获得完整的名单，请访问 https://www.php.net/manual/en/filter.filters.php。

## 验证过滤器

`FILTER_VALIDATE_BOOLEAN`为`1`、`true`、`on`和`yes`返回`true`。否则返回`false`。与`FILTER_VALIDATE_BOOL`同义。

```
<?php
var_dump( filter_var(true, FILTER_VALIDATE_BOOLEAN) ); // true
```

`FILTER_VALIDATE_EMAIL`验证该值是否是有效的电子邮件地址。

```
<?php
var_dump( filter_var("dino@example.com", FILTER_VALIDATE_EMAIL) ); // dino@example.com
```

`FILTER_VALIDATE_IP`验证 IP 地址，仅可选 IPv4 或 IPv6。

```
<?php
var_dump( filter_var("192.168.1.10", FILTER_VALIDATE_IP) ); // 192.168.1.10
var_dump( filter_var("78.123.186.65", FILTER_VALIDATE_IP) ); // 78.123.186.65
var_dump( filter_var("78.123.186", FILTER_VALIDATE_IP) ); // false
var_dump( filter_var("6AAA:1111:222:3333:4444:5555:678:778", FILTER_VALIDATE_IP) ); // false
```

`FILTER_VALIDATE_MAC`验证 MAC 地址。

```
<?php
var_dump( filter_var("12:34:F4:90:4F:9B", FILTER_VALIDATE_MAC) ); // 12:34:F4:90:4F:9B
```

`FILTER_VALIDATE_URL`验证 URL。

```
<?php
var_dump( filter_var("https://dinocajic.com", FILTER_VALIDATE_URL) ); // https://dinocajic.com
var_dump( filter_var("https:/dinocajic.com", FILTER_VALIDATE_URL) ); // false
```

## 净化过滤器

净化过滤器将实际尝试净化输入的值。我们来看几个。

`FILTER_SANITIZE_EMAIL`删除除字母、数字和！#$% & '*+-=？^_`{|}~@.[].

```
<?php
var_dump( filter_var("dino@example.com", FILTER_SANITIZE_EMAIL) ); // dino@example.com
var_dump( filter_var("(dino@example.com)", FILTER_SANITIZE_EMAIL) ); // dino@example.com
var_dump( filter_var("dino#example.com", FILTER_SANITIZE_EMAIL) ); // dino#example.com
```

清理只去除特殊字符，而不是允许的字符。因此，在最后一个示例中，它返回经过净化的字符串，即使它不是有效的电子邮件地址。要验证电子邮件地址，您需要在之后使用`FILTER_VALIDATE_EMAIL`。

```
<?php
$sanitized = filter_var("dino#example.com", FILTER_SANITIZE_EMAIL);

if ( filter_var($sanitized, FILTER_VALIDATE_EMAIL) !== false) {
    echo "Valid email address";
}
```

`FILTER_SANITIZE_ADD_SLASHES`应用了我们将在下一篇文章中讨论的`addslashes()`函数。没什么；它只是给需要转义的字符加了反斜杠。

```
<?php
var_dump( filter_var("Dino's Story", FILTER_SANITIZE_ADD_SLASHES) ); 

// Dino\'s Story
```

`FILTER_SANITIZE_STRING`比上面的添加斜线滤镜多做一点工作。它去掉标签并对双引号和单引号进行 HTML 编码。

```
<?php
var_dump( filter_var("Dino's <strong>Story</strong>", FILTER_SANITIZE_STRING) ); 

// Dino&#39;s Story
```

`FILTER_SANITIZE_URL`去除除字母、数字和$-_ 以外的所有字符。+!*'(),{}|\\^~[]`<># %”；/?:@ & =。

```
<?php
var_dump( filter_var("https://dinocajic.com", FILTER_SANITIZE_URL) ); 

// https://dinocajic.com
```

## 旗帜

让我们来看一个标志的例子:第三个参数。看一看`FILTER_VALIDATE_BOOLEAN`，我们可以在`filter_var`中将`FILTER_NULL_ON_FAILURE`标志作为第三个参数传递。如果设置了`FILTER_NULL_ON_FAILURE`，则`false`仅针对`0`、`false`、`off`、`no`和空字符串返回，而`null`针对所有非布尔值返回。

```
<?php
var_dump( filter_var(true, FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE) ); // true
var_dump( filter_var(false, FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE) ); // false
var_dump( filter_var(0, FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE) ); // false
var_dump( filter_var("off", FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE) ); // false
var_dump( filter_var("no", FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE) ); // false
var_dump( filter_var("yes", FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE) ); // true
var_dump( filter_var(35, FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE) ); // null
```

我们可以将其传递给`FILTER_VALIDATE_EMAIL`过滤器。

```
<?php
var_dump( filter_var("dino@example.com", FILTER_VALIDATE_EMAIL, FILTER_NULL_ON_FAILURE) ); // dino@example.com
var_dump( filter_var("dinoexample.com", FILTER_VALIDATE_EMAIL, FILTER_NULL_ON_FAILURE) ); // null
```

`filter_var`的基础就到此为止。你可以用它做一些额外的事情，但这是你将花费大部分时间的地方。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/55bb73b49816a29c45f154c43612f443.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 负责人，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。