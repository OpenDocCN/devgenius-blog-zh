# PHP — P32: Foreach 循环

> 原文：<https://blog.devgenius.io/php-7-x-p32-foreach-loop-f38b88249e76?source=collection_archive---------2----------------------->

![](img/d0d247812e7663e21c4ec6e2743f62bf.png)

你可以使用 [*for* 循环](https://medium.com/@dinocajic/php-7-x-p31-for-loop-68724b49b861)来迭代一个数组，但是要真正闪耀，你需要使用 *foreach* 循环。 *foreach* 循环具有以下语法。

```
foreach ($array as $key => $value)
```

你会读到这样的结构:

*   对于每个数组作为键/值…或
*   对于数组中的每个键/值对。

这意味着 *foreach* 循环将遍历每个数组元素，并且您可以访问每个元素的每个键/值对。如果你正在处理[关联数组](https://medium.com/@dinocajic/php-7-x-p10-associative-arrays-e19f11edc3ff)，这是很棒的。如果键是一个字符串，您将无法访问带有数字索引的元素。

有时你也需要显示密钥。例如，假设您将一个待办事项列表存储在一个关联数组中，其中每个键代表一周中的某一天。您可能还需要向用户显示密钥。如果不这样做，可以减少 foreach 语句的结构。

```
foreach ($array as $value)
```

上面的结构将只为您提供对元素值的访问。您仍然可以循环遍历包含非数字关键字或数字但非连续关键字的数组。

让我们看一个 *foreach* 循环遍历具有数字索引的普通数组的例子。

我们可以使用 *for* 循环来遍历数组，或者我们可以使用 *foreach* 循环。

从上面的例子可以看出，有了 *foreach* 循环，语法就简单多了。我们不必使用 *count()* 函数来获取数组中元素的数量。我们也不必使用计数器变量( *$i* )来跟踪我们所在的索引。当我们使用 *foreach* 循环时，PHP 为我们做了所有这些。

检查上面的例子，PHP:

1.  遇到 *foreach* 语句。它知道它将遍历一个数组。
2.  *foreach* 循环内部的语法表示将访问每个数组元素值。
3.  在第一次迭代中，PHP 可以访问第一个元素。当执行 *echo* 语句时， *$value* 变量指向数组内部的第一个元素。所以，PHP 显示“碎肉机爪子”
4.  PHP 到达循环体的末尾，重复这个过程，直到数组中不再有元素。

使用 *foreach* 语句，您不必担心您选择的索引值是否会超出范围。PHP 将为您跟踪这一点。

*foreach* 循环结构内的 *$key* 和 *$value* 元素**不能**命名为 key/value。你可以给它们起任何你喜欢的名字。例如，假设我们有一组汽车。我们可以将元素命名为 *$car* ，而不是命名为 *$value* ，并明确该值代表什么。

这比我们使用*作为*循环或者简单地说 *$value* 而不是 *$car* 要容易理解得多。

让我们实现前面提到的待办事项列表示例。这一次，我们需要访问键和值。我们可以将键/值对命名为 *$key/$value* ，或者我们可以明确命名。键存储星期几，值存储待办事项。我们将把键命名为 *$day_in_week* ，把值命名为 *$to_do_item* 。

当我们遍历数组时，我们可以访问键和值。PHP 将显示键的内容，后面是一个冒号，再后面是数组中每个元素的值的内容。请记住， *foreach* 结构要求您使用*作为*关键字，如果您正在访问键/值对，则在 *foreach* 语句中使用箭头(= >)。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/5db297b86d18a5164c3526dc79ac032e.png)

迪诺·卡希奇目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物科技](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读迪诺·卡吉克(以及媒体上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。