# PHP — P106: __invoke、__toString、__get 和 __set 魔术方法

> 原文：<https://blog.devgenius.io/php-p106-invoke-tostring-get-and-set-magic-methods-eda38d07bc0a?source=collection_archive---------9----------------------->

![](img/42866714094d920ab9acfbc99e5c9536.png)

我们正在讨论 PHP 必须提供的最后一个内置函数/方法。我们还没有接近 PHP 提供的内置函数的全部范围，但是我认为我们已经涵盖了最常用的函数。在本文中，我们将看看`__invoke`、`__toString`、`__get`和`__set`方法。

PHP 有几个“神奇的方法”，如`__construct`、`__destruct`、`__call`、`__callStatic`、`__get`、`__set`、`__isset`、`__unset`、`__sleep`、`__wakeup`、`__serialize`、`__toString`、`__invoke`、`__setState`、`__clone`和`__debuginfo`。我们已经介绍了`__call`、`__construct`和`__destruct`魔法方法。

[](/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9) [## PHP — P101: trim、htmlspecialchars 和 __call 内置函数

### 在过去的 100 篇 PHP 文章中，我们使用了各种内置函数。是时候把它们收集到一个地方了…

blog.devgenius.io](/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9) 

*   `[trim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9),`、`[ltrim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`、`[rtrim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `[htmlspecialchars()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `[__call()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
*   `[preg_match()](https://medium.com/dev-genius/php-p102-preg-match-and-regular-expressions-b4b23869261a)`
*   `[filter_var()](https://medium.com/dev-genius/php-p103-filter-var-built-in-function-b21c186a24e9)`
*   `[addslashes()](https://medium.com/dev-genius/php-p104-string-functions-acb8bbeae832)`
*   `[str_replace()](https://medium.com/dev-genius/php-p104-string-functions-acb8bbeae832)`
*   `[strlen()](https://medium.com/dev-genius/php-p104-string-functions-acb8bbeae832)`
*   `[strtolower()](https://medium.com/dev-genius/php-p104-string-functions-acb8bbeae832)`
*   `[strtoupper()](https://medium.com/dev-genius/php-p104-string-functions-acb8bbeae832)`
*   `[ucfirst()](https://medium.com/dev-genius/php-p104-string-functions-acb8bbeae832)`
*   `[strpos()](https://medium.com/dev-genius/php-p104-string-functions-acb8bbeae832)`、`[stripos()](https://medium.com/dev-genius/php-p104-string-functions-acb8bbeae832)`、`[strrpos()](https://medium.com/dev-genius/php-p104-string-functions-acb8bbeae832)`、`[strripos()](https://medium.com/dev-genius/php-p104-string-functions-acb8bbeae832)`
*   [数组函数有:](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1) `[array_chunk()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)` [，](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1) `[array_diff()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)` [，](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1) `[array_key_exists()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)` [，](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1) `[array_key_first()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)` [，](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1) `[array_key_last()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)` [，](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1) `[array_map()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)` [，](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1) `[array_merge()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)` [，](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1) `[array_push()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)` [，](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)`[array_sum()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)` [，](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1) `[asort()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)` [，](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1) `[arsort()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)`，`[count()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)`，`[in_array()](https://medium.com/dev-genius/php-p105-array-functions-54f682a76c1)`
*   神奇的方法有:`**__invoke()**` **、**、`**__toString()**`、**、**、`**__get()**`、**、**、`**__set()**`

# __invoke()

当脚本像调用函数一样调用对象时，就会调用`__invoke`魔法方法。

```
<?php

class InvokeTest
{
    public function __invoke( $name )
    {
        echo "Hey " . $name;
    }
}

$invoke = new InvokeTest;
$invoke("Dino");
```

有意思。我们实例化 InvokeTest 对象，然后像调用函数一样调用该对象。我们确实得到了回应，那就是:`Hey Dino`。

# __toString()

如果你曾经想要`echo`对象本身，你只需要实现`__toString`方法。

```
<?php

class ToStringTest
{
    private string $name = "Dino Cajic";

    public function __toString() {
        return $this->name;
    }
}

$test = new ToStringTest;
echo $test;
```

我们得到的回应是`Dino Cajic`。

# __set()，__get()

在我们结束的时候，我们也可以讨论一下`__set`和`__get`方法。类似于`__call`魔术方法，当属性不可访问时，调用`__set`和`__get`，因为私有/受保护修饰符被应用到属性，或者因为属性不存在。

`__set`魔术方法接受两个参数，`$name`和`$value`。`$name`是您试图访问的属性，`$value`是您试图分配的值。

对于`__get`魔法方法，我们将只传递您试图访问的属性。

```
<?php

class GetAndSetTest
{
    private array $data;

    public function __set($name, $value) {
        $this->data[$name] = $value;
    }

    public function __get($name) {
        return $this->data[$name];
    }
}

$test = new GetAndSetTest;

$test->name = "Dino Cajic";
echo $test->name;

$test->age = 55;
echo $test->age;
```

尽管我们没有那些属性，但我们有一个`__set`魔法方法。它先用`$data[‘name’] = ‘Dino Cajic’`设置我们的`$data`数组，然后再用`$data[‘age’] = 55`设置。然后我们可以用`__get`方法调用这些属性。所以我们得到的回应是，一旦我们调用我们的属性:`Dino Cajic`和`55`。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/e72714a9d19ff78da0aaa3cf5d968293.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 负责人，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。