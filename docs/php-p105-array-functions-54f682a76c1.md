# PHP — P105:数组函数

> 原文：<https://blog.devgenius.io/php-p105-array-functions-54f682a76c1?source=collection_archive---------4----------------------->

![](img/3e95244658958587a8266f4a04f024fc.png)

接近完成。在上一篇文章中，我们研究了字符串函数，在这一篇文章中，我们将看看所有不同的和常见的数组函数。

[](/php-p104-string-functions-acb8bbeae832) [## PHP — P104:字符串函数

### 这篇文章是关于字符串函数的。这些都是简单的内置函数，几乎不需要掌握。他们是…

blog.devgenius.io](/php-p104-string-functions-acb8bbeae832) 

*   `[trim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9),` `[ltrim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`，`[rtrim()](https://medium.com/dev-genius/php-p101-trim-htmlspecialchars-and-call-built-in-functions-7b4f25728db9)`
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
*   数组函数有:`**array_chunk()**` **、**、`**array_diff()**`、**、**、`**array_key_exists()**`、、`**array_key_first()**`、**、**、`**array_map()**`、**、**、`**array_merge()**`、**、**、`**array_push()**`、**、**、`**asort()**`、**、**、`**arsort()**`、**、**、`**count()**`、**、**
*   **神奇的方法有:`__invoke()`、`__toString()`**

# **array_chunk()**

**如果你有一个数组，并且你想把它做成多个更小的数组，你可以使用`array_chunk`方法。第一个参数是大数组，第二个参数是每个块中的元素数量，第三个参数决定是否要保留键。**

```
<?php
$array = ["Hello", "There", "How", "Are", "You", "Doing", "Today"];
var_dump( array_chunk($array, 2) );
```

```
/app/98 Functions/ArrayFunctions.php:4:
array (size=4)
  0 => 
    array (size=2)
      0 => string 'Hello' (length=5)
      1 => string 'There' (length=5)
  1 => 
    array (size=2)
      0 => string 'How' (length=3)
      1 => string 'Are' (length=3)
  2 => 
    array (size=2)
      0 => string 'You' (length=3)
      1 => string 'Doing' (length=5)
  3 => 
    array (size=1)
      0 => string 'Today' (length=5)
```

```
<?php
$array = ["Hello", "There", "How", "Are", "You", "Doing", "Today"];
var_dump( array_chunk($array, 3) );
```

```
/app/98 Functions/ArrayFunctions.php:5:
array (size=3)
  0 => 
    array (size=3)
      0 => string 'Hello' (length=5)
      1 => string 'There' (length=5)
      2 => string 'How' (length=3)
  1 => 
    array (size=3)
      0 => string 'Are' (length=3)
      1 => string 'You' (length=3)
      2 => string 'Doing' (length=5)
  2 => 
    array (size=1)
      0 => string 'Today' (length=5)
```

```
<?php
$array = ["Hello", "There", "How", "Are", "You", "Doing", "Today"];
var_dump( array_chunk($array, 3, true) );
```

```
/app/98 Functions/ArrayFunctions.php:6:
array (size=3)
  0 => 
    array (size=3)
      0 => string 'Hello' (length=5)
      1 => string 'There' (length=5)
      2 => string 'How' (length=3)
  1 => 
    array (size=3)
      3 => string 'Are' (length=3)
      4 => string 'You' (length=3)
      5 => string 'Doing' (length=5)
  2 => 
    array (size=1)
      6 => string 'Today' (length=5)
```

```
<?php
$array = [
    "first_key" => "Hello",
    "second_key" => "There",
    "third_key" => "How",
    "fourth_key" => "Are",
    "fifth_key" => "You",
    "sixth_key" => "Doing",
    "seventh_key" => "Today"
];
var_dump( array_chunk($array, 2) );
```

```
/app/98 Functions/ArrayFunctions.php:17:
array (size=4)
  0 => 
    array (size=2)
      0 => string 'Hello' (length=5)
      1 => string 'There' (length=5)
  1 => 
    array (size=2)
      0 => string 'How' (length=3)
      1 => string 'Are' (length=3)
  2 => 
    array (size=2)
      0 => string 'You' (length=3)
      1 => string 'Doing' (length=5)
  3 => 
    array (size=1)
      0 => string 'Today' (length=5)
```

```
<?php
$array = [
    "first_key" => "Hello",
    "second_key" => "There",
    "third_key" => "How",
    "fourth_key" => "Are",
    "fifth_key" => "You",
    "sixth_key" => "Doing",
    "seventh_key" => "Today"
];
var_dump( array_chunk($array, 3) );
```

```
/app/98 Functions/ArrayFunctions.php:18:
array (size=3)
  0 => 
    array (size=3)
      0 => string 'Hello' (length=5)
      1 => string 'There' (length=5)
      2 => string 'How' (length=3)
  1 => 
    array (size=3)
      0 => string 'Are' (length=3)
      1 => string 'You' (length=3)
      2 => string 'Doing' (length=5)
  2 => 
    array (size=1)
      0 => string 'Today' (length=5)
```

```
<?php
$array = [
    "first_key" => "Hello",
    "second_key" => "There",
    "third_key" => "How",
    "fourth_key" => "Are",
    "fifth_key" => "You",
    "sixth_key" => "Doing",
    "seventh_key" => "Today"
];
var_dump( array_chunk($array, 3, true) );
```

```
/app/98 Functions/ArrayFunctions.php:19:
array (size=3)
  0 => 
    array (size=3)
      'first_key' => string 'Hello' (length=5)
      'second_key' => string 'There' (length=5)
      'third_key' => string 'How' (length=3)
  1 => 
    array (size=3)
      'fourth_key' => string 'Are' (length=3)
      'fifth_key' => string 'You' (length=3)
      'sixth_key' => string 'Doing' (length=5)
  2 => 
    array (size=1)
      'seventh_key' => string 'Today' (length=5)
```

# **array_diff()**

**当你有两个数组，并且想知道哪些值在两个数组中都不存在时，你可以使用`array_diff`函数。它只查看值，所以即使键不同，只要值相同，它就认为元素出现在两个数组中。**

```
<?php
$first  = ["first_key"  => "value", "Hey", "How", "Are", "You"];
$second = ["second_key" => "value", "Hey", "How", "You"];

var_dump( array_diff($first, $second) );
```

```
/app/98 Functions/ArrayFunctions.php:26:
array (size=2)
  'first_key' => string 'value' (length=5)
  2 => string 'Are' (length=3)
```

# **数组关键字存在()**

**如果我们需要检查一个特定的数组键是否存在于数组内部，我们可以用`array_key_exists`函数来检查它。**

```
<?php
$array = [
    "first_key" => "Hello",
    "second_key" => "There",
    "third_key" => "How",
];

var_dump( array_key_exists("fourth_key", $array) ); // false
var_dump( array_key_exists("second_key", $array) ); // true
```

# **array_key_first()，array_key_last()**

**`array_key_first`函数获取数组的第一个键。而`array_key_last`函数获取数组中的最后一个数组键。**

```
<?php
$array = [
    "first_key" => "Hello",
    "second_key" => "There",
    "third_key" => "How",
];

var_dump( array_key_first($array) );
var_dump( array_key_last($array) );
```

```
/app/98 Functions/ArrayFunctions.php:45:string 'first_key' (length=9)
/app/98 Functions/ArrayFunctions.php:46:string 'third_key' (length=9)
```

# **array_merge()**

**`array_merge`函数合并一个或多个数组。我们将看看如何将两个和三个数组合并在一起。**

```
<?php
$first  = ["first_key"  => "value", "Hey", "How", "Are", "You"];
$second = ["second_key" => "value", "Hey", "How", "You"];
$third  = ["third_key"  => "value", "Hey", "How", "You"];

var_dump( array_merge($first, $second) );
var_dump( array_merge($first, $second, $third) );
```

```
/app/98 Functions/ArrayFunctions.php:53:
array (size=9)
  'first_key' => string 'value' (length=5)
  0 => string 'Hey' (length=3)
  1 => string 'How' (length=3)
  2 => string 'Are' (length=3)
  3 => string 'You' (length=3)
  'second_key' => string 'value' (length=5)
  4 => string 'Hey' (length=3)
  5 => string 'How' (length=3)
  6 => string 'You' (length=3)
```

```
/app/98 Functions/ArrayFunctions.php:54:
array (size=13)
  'first_key' => string 'value' (length=5)
  0 => string 'Hey' (length=3)
  1 => string 'How' (length=3)
  2 => string 'Are' (length=3)
  3 => string 'You' (length=3)
  'second_key' => string 'value' (length=5)
  4 => string 'Hey' (length=3)
  5 => string 'How' (length=3)
  6 => string 'You' (length=3)
  'third_key' => string 'value' (length=5)
  7 => string 'Hey' (length=3)
  8 => string 'How' (length=3)
  9 => string 'You' (length=3)
```

**我们能看到的是价值观并没有被消除。它不关心多个数组中是否存在完全相同的值。它只是把一切都融合在一起。**

**如果我们有两个同名的数组键，会发生什么？**

```
<?php
$first  = ["first_key"  => "value one"];
$second = ["first_key"  => "value two"];

var_dump( array_merge($first, $second) );
```

**在这种情况下，最后一个值被指定给数组键。所以，如果你使用命名索引的话，要小心使用`array_merge`。**

```
/app/98 Functions/ArrayFunctions.php:59:
array (size=1)
  'first_key' => string 'value two' (length=9)
```

# **array_push()**

**`array_push`函数将一个元素添加到数组的末尾。**

```
<?php
$array = ["Hello", "There"];
array_push($array, "How", "Are", "You");

var_dump($array);
```

**一个常见的错误是将`array_push`赋回数组。第一个参数数组作为引用传递，这意味着对它的任何修改都会自动反映到数组中。**

```
/app/98 Functions/ArrayFunctions.php:65:
array (size=5)
  0 => string 'Hello' (length=5)
  1 => string 'There' (length=5)
  2 => string 'How' (length=3)
  3 => string 'Are' (length=3)
  4 => string 'You' (length=3)
```

**您也可以使用不带索引值的开/闭括号来完成此操作。**

```
<?php
$array = ["Hello", "There"];
$array[] = "How";
$array[] = "Are";
$array[] = "You";

var_dump($array);
```

```
/app/98 Functions/ArrayFunctions.php:72:
array (size=5)
  0 => string 'Hello' (length=5)
  1 => string 'There' (length=5)
  2 => string 'How' (length=3)
  3 => string 'Are' (length=3)
  4 => string 'You' (length=3)
```

# **array_sum()**

**`array_sum`函数计算数组中所有整数/浮点值之和的值。**

```
<?php
$array = [1, 2.2, 3, 4, "named_key" => 6];
var_dump( array_sum($array) );
```

```
/app/98 Functions/ArrayFunctions.php:76:float 16.2
```

# **asort()**

**按值对数组排序。它不关心数组是否有命名键或数字键，它只看值。`asort`函数保持索引关联。**

```
<?php
$first  = ["E", "C", "B", "A"];
$second = ["E", "c", "B", "A"];
$third  = ["E", 1, "B", 63];
$fourth = ["E", "first_key" => 1, "second_key" => "B", 63];

asort($first);
asort($second);
asort($third);
asort($fourth);

var_dump( $first );
var_dump( $second );
var_dump( $third );
var_dump( $fourth );
```

```
/app/98 Functions/ArrayFunctions.php:89:
array (size=4)
  3 => string 'A' (length=1)
  2 => string 'B' (length=1)
  1 => string 'C' (length=1)
  0 => string 'E' (length=1)
```

```
/app/98 Functions/ArrayFunctions.php:90:
array (size=4)
  3 => string 'A' (length=1)
  2 => string 'B' (length=1)
  0 => string 'E' (length=1)
  1 => string 'c' (length=1)
```

```
/app/98 Functions/ArrayFunctions.php:91:
array (size=4)
  1 => int 1
  3 => int 63
  2 => string 'B' (length=1)
  0 => string 'E' (length=1)
```

```
/app/98 Functions/ArrayFunctions.php:92:
array (size=4)
  'first_key' => int 1
  1 => int 63
  'second_key' => string 'B' (length=1)
  0 => string 'E' (length=1)
```

# **排序()**

**要按值对数组进行反向排序，并保持索引关联，请使用`arsort`。**

```
<?php
$first  = ["E", "C", "B", "A"];
$second = ["E", "c", "B", "A"];
$third  = ["E", 1, "B", 63];
$fourth = ["E", "first_key" => 1, "second_key" => "B", 63];

arsort($first);
arsort($second);
arsort($third);
arsort($fourth);

var_dump( $first );
var_dump( $second );
var_dump( $third );
var_dump( $fourth );
```

```
/app/98 Functions/ArrayFunctions.php:105:
array (size=4)
  0 => string 'E' (length=1)
  1 => string 'C' (length=1)
  2 => string 'B' (length=1)
  3 => string 'A' (length=1)

/app/98 Functions/ArrayFunctions.php:106:
array (size=4)
  1 => string 'c' (length=1)
  0 => string 'E' (length=1)
  2 => string 'B' (length=1)
  3 => string 'A' (length=1)

/app/98 Functions/ArrayFunctions.php:107:
array (size=4)
  0 => string 'E' (length=1)
  2 => string 'B' (length=1)
  3 => int 63
  1 => int 1

/app/98 Functions/ArrayFunctions.php:108:
array (size=4)
  0 => string 'E' (length=1)
  'second_key' => string 'B' (length=1)
  1 => int 63
  'first_key' => int 1
```

# **计数()**

**`count`函数返回数组中数组元素的个数。一个常见的错误是认为如果一个数组元素在数组内部，答案应该是`0`。请记住，这与索引值没有任何关系。是元素的计数。**

```
<?php
$array  = ["E", "C", "B", "A"];

var_dump( count($array) );
```

```
/app/98 Functions/ArrayFunctions.php:113:int 4
```

# **sizeof()**

**`count`功能的别名。**

```
<?php
$array  = ["E", "C", "B", "A"];

var_dump( sizeof($array) );
```

```
/app/98 Functions/ArrayFunctions.php:176:int 4
```

# **in_array()**

**为了检查数组中是否存在一个值(不是键),我们将使用`in_array`函数。**

```
<?php
$first  = ["E", 1, "B", 63];
$second = ["E", "first_key" => 1, "second_key" => "B", 63];

var_dump( in_array("E", $first) );          // true
var_dump( in_array("A", $second) );         // false 
var_dump( in_array(63, $second) );          // true
var_dump( in_array("first_key", $second) ); // false
```

# **ksort()**

**要按关键字升序排列，使用`ksort`。**

```
<?php
$first  = ["E", 1, "B", 63];
$second = ["E", "z_key" => 1, "a_key" => "B", 63];

ksort($first);
ksort($second);
```

```
/app/98 Functions/ArrayFunctions.php:131:
array (size=4)
  0 => string 'E' (length=1)
  1 => int 1
  2 => string 'B' (length=1)
  3 => int 63
```

```
/app/98 Functions/ArrayFunctions.php:132:
array (size=4)
  0 => string 'E' (length=1)
  'a_key' => string 'B' (length=1)
  'z_key' => int 1
  1 => int 63
```

**`a_key`和`z_key`排在`0`和`1`之间真的很有意思。**

# **krsort()**

**要按键降序排列一个数组，使用`krsort`。**

```
<?php
// krsort
$first  = ["E", 1, "B", 63];
$second = ["E", "z_key" => 1, "a_key" => "B", 63];

krsort($first);
krsort($second);

var_dump( $first );
var_dump( $second );
```

```
/app/98 Functions/ArrayFunctions.php:141:
array (size=4)
  3 => int 63
  2 => string 'B' (length=1)
  1 => int 1
  0 => string 'E' (length=1)

/app/98 Functions/ArrayFunctions.php:142:
array (size=4)
  1 => int 63
  0 => string 'E' (length=1)
  'z_key' => int 1
  'a_key' => string 'B' (length=1)
```

**更有意思的是逆向排序的时候。你可能会认为`a_key`和`z_key`仍然在`0`和`1`之间，但正如你所看到的，事实并非如此。对混合键(索引和命名)进行排序时要小心。**

# **排序()**

**`sort`函数按照值的升序对数组进行排序，并删除所有的数组键关联。**

```
<?php
// sort
$first  = ["E", 1, "B", 63];
$second = ["E", "z_key" => 1, "a_key" => "B", 63];

sort($first);
sort($second);

var_dump( $first );
var_dump( $second );
```

```
/app/98 Functions/ArrayFunctions.php:151:
array (size=4)
  0 => int 1
  1 => int 63
  2 => string 'B' (length=1)
  3 => string 'E' (length=1)

/app/98 Functions/ArrayFunctions.php:152:
array (size=4)
  0 => int 1
  1 => int 63
  2 => string 'B' (length=1)
  3 => string 'E' (length=1)
```

# **rsort()**

**`rsort`函数按值降序排列数组，并删除所有的数组键关联。**

```
<?php
$first  = ["E", 1, "B", 63];
$second = ["E", "z_key" => 1, "a_key" => "B", 63];

rsort($first);
rsort($second);

var_dump( $first );
var_dump( $second );
```

```
/app/98 Functions/ArrayFunctions.php:161:
array (size=4)
  0 => string 'E' (length=1)
  1 => string 'B' (length=1)
  2 => int 63
  3 => int 1

/app/98 Functions/ArrayFunctions.php:162:
array (size=4)
  0 => string 'E' (length=1)
  1 => string 'B' (length=1)
  2 => int 63
  3 => int 1
```

# **无序播放()**

**`shuffle`函数对数组进行洗牌。**

```
<?php
$array  = ["first_key"  => "value", "Hey", "How", "Are", "You"];

shuffle($array);
var_dump($array);

shuffle($array);
var_dump($array);
```

```
/app/98 Functions/ArrayFunctions.php:168:
array (size=5)
  0 => string 'How' (length=3)
  1 => string 'You' (length=3)
  2 => string 'Are' (length=3)
  3 => string 'value' (length=5)
  4 => string 'Hey' (length=3)

/app/98 Functions/ArrayFunctions.php:171:
array (size=5)
  0 => string 'You' (length=3)
  1 => string 'Are' (length=3)
  2 => string 'value' (length=5)
  3 => string 'How' (length=3)
  4 => string 'Hey' (length=3)
```

# **is_array()**

**要检查变量是否包含数组，我们可以使用`is_array`函数。**

```
<?php
$first = ["A", "B"];
$second = "Dino";

var_dump( is_array($first) );  // true
var_dump( is_array($second) ); // false
```

# **爆炸()**

**`explode`函数将一个字符串转换成一个指定分隔符的数组。**

```
<?php
$string = "Hey there. How are you?";
$first = explode(" ", $string);
$second = explode(".", $string);

var_dump( $first );
var_dump( $second );
```

```
/app/98 Functions/ArrayFunctions.php:190:
array (size=5)
  0 => string 'Hey' (length=3)
  1 => string 'there.' (length=6)
  2 => string 'How' (length=3)
  3 => string 'are' (length=3)
  4 => string 'you?' (length=4)

/app/98 Functions/ArrayFunctions.php:191:
array (size=2)
  0 => string 'Hey there' (length=9)
  1 => string ' How are you?' (length=13)
```

# **内爆()**

**`implode`函数获取一个数组并将其转换成一个字符串。您可以添加您选择的分隔符。**

```
<?php
$array  = ["Hey,", "How", "Are", "You?"];
$string_one = implode(" ", $array);
$string_two = implode("-", $array);

var_dump($string_one);
var_dump($string_two);
```

```
/app/98 Functions/ArrayFunctions.php:198:string 'Hey, How Are You?' (length=17)
/app/98 Functions/ArrayFunctions.php:199:string 'Hey,-How-Are-You?' (length=17)
```

# **array_map()**

**我们要介绍的最后一个数组函数是`array_map`函数。`array_map`函数的第一个参数需要一个回调函数，第二个参数需要一个数组。回调函数为每个数组元素运行。因此，想象一下循环遍历一个数组并对每个值应用一些操作。一旦操作完成，您应该将它重新指定回数组。如果数组是`[1, 2, 3]`并且我们想要加倍每个数组元素，我们将把每个值乘以`2`并把它赋回数组以得到`[2, 4, 6]`。**

```
function multiply_by_two( $n )
{
    return ($n * 2);
}

$a = [1, 2, 3];
$b = array_map('multiply_by_two', $a);

var_dump( $a );
var_dump( $b );
```

```
/app/98 Functions/ArrayFunctions.php:210:
array (size=3)
  0 => int 1
  1 => int 2
  2 => int 3

/app/98 Functions/ArrayFunctions.php:211:
array (size=3)
  0 => int 2
  1 => int 4
  2 => int 6
```

**为了让这个例子更真实，我们可以将匿名函数作为第一个参数传递。**

```
<?php

$a = [1, 2, 3];
$b = array_map(function($n) {
    return ($n * 2);
}, $a);

var_dump( $a );
var_dump( $b );
```

**我们可以进一步简化它，把移动数组本身作为第二个参数，而不是先把它赋给`$a`。**

```
<?php
$b = array_map(function($n) {
    return ($n * 2);
}, [1, 2, 3]);

var_dump( $b );
```

**我们终于可以把匿名函数做成箭头函数了。**

```
<?php
$b = array_map( fn($n) => $n * 2, [1, 2, 3] );

var_dump( $b );
```

**如果你需要一个回调函数、箭头函数或者匿名函数的概要，我可以帮你。**

**[](/php-7-x-p39-anonymous-functions-75f6f99fa571) [## PHP — P39:匿名函数

### 匿名函数是没有名字的函数。

blog.devgenius.io](/php-7-x-p39-anonymous-functions-75f6f99fa571) [](/php-7-x-p40-use-keyword-37d8e7df9138) [## PHP — P40:使用关键字

### 当函数被定义时，Use 获取全局变量的值，而 global 将获取变量的值…

blog.devgenius.io](/php-7-x-p40-use-keyword-37d8e7df9138) [](/php-7-x-p41-arrow-functions-68a68b2debc6) [## PHP — P41:箭头函数

### PHP 中匿名函数和箭头函数的主要区别是使用了 use 关键字。

blog.devgenius.io](/php-7-x-p41-arrow-functions-68a68b2debc6) [](/php-7-x-p42-callback-functions-6552477f2a87) [## PHP — P42:回调函数

### 是时候结束这个话题了。我通过令人痛苦的细节来简化这个概念。

blog.devgenius.io](/php-7-x-p42-callback-functions-6552477f2a87) 

我们已经完成了数组函数。还有一些内置功能。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/d8306c276d559e3e776eb3eb4e62e436.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 负责人，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。**