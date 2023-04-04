# PHP 7.4 有什么新特性？

> 原文：<https://blog.devgenius.io/whats-new-in-php-7-4-f859924be364?source=collection_archive---------0----------------------->

![](img/26d9cefc0142ff138d294ec72c8e14a5.png)

PHP 7.4

PHP 开发团队在 2019 年 11 月 28 日刚刚发布了 **PHP 7.4.0 版本**。

这个版本包括几个改进和新功能，我们将讨论其中重要的。

## 1.类型化属性

类属性现在支持类型声明:

```
<?php
class User {
    public int $id;
    public string $name;
}
?>
```

正如您所看到的，这段代码将强制要求`*$user->id*` 只能**被赋予整数值，而`*$user->name*`只能*被赋予字符串值。***

## ***2.在阵列内部拆包***

```
*<?php
$parts = ['apple', 'pear'];
$fruits = ['banana', 'orange', ...$parts, 'watermelon'];
// ['banana', 'orange', 'apple', 'pear', 'watermelon'];
?>*
```

***在上面的例子中，变量`$parts`是一个在`$fruits`数组中使用的数组，解包过程在`$fruits`数组中完成。这是对`array_merge`函数的替代，但是使用新语法的**优势是它的速度**。***

## ***3.箭头功能***

***箭头函数为单行函数提供了一种简化的语法。这些函数可以称为“短闭包”。***

***PHP 7.4.0 之前:***

```
*array_map(function (User $user) {
    return $user->id;  
}, $users)*
```

***现在:***

```
*array_map(fn (User $user) => $user->id, $users)*
```

*****更多信息，可以查看 PHP 7.4.0 新增功能和改进** [**这里**](https://www.php.net/archive/2019.php#2019-11-28-1) **。*****

***谢谢你抽出时间。如果你有任何想法或建议，请随意写评论。***