# PHP — P88: MySQL 选择

> 原文：<https://blog.devgenius.io/php-p88-mysql-select-d7a633e736b?source=collection_archive---------6----------------------->

![](img/28ac2b41e9702494e4203ff4869847e2.png)

我们已经完成了插入过程，在我们的`authors`表中插入了一些记录。现在怎么办？一个常见的任务是检索这些条目并在屏幕上显示它们。这就是`SELECT`语句的由来。

[](https://dinocajic.medium.com/php-p87-mysql-insert-multiple-records-with-prepared-statements-c5c7bcfb42a3) [## PHP — P87: MySQL 用准备好的语句插入多条记录

### 你可能会注意到文章之间有一些小的堆砌，这是有意的。没有理由压倒或淡化这个话题。在…

dinocajic.medium.com](https://dinocajic.medium.com/php-p87-mysql-insert-multiple-records-with-prepared-statements-c5c7bcfb42a3) 

# 一个简单的 Select 语句

为了让选择发挥作用，我们需要:

*   连接到数据库
*   传递我们的选择查询
*   检索结果，这只是数据行。
*   循环遍历每个结果，并提取返回的每一行的数据。
*   关闭连接

直截了当。让我们首先隔离选择过程。它从 select 语句开始。

```
$sql = "SELECT * FROM authors";

$results = $mysqli->query( $sql );
```

我们将传递我们的`SELECT * FROM authors`，它只是意味着“从 authors 表中选择所有列”这将返回我们所有的字段:`id`、`first_name`、`last_name`、`updated_at`和`created_at`字段。

`SQL`语句被传递给`mysqli`对象中的`query`方法，它返回包含所有这些字段的表中的所有行。它们存储在`$results`变量中。

接下来，我们需要遍历每一行并从中提取数据。

```
while( $row = $results->fetch_assoc() ) 
{
    var_dump($row);
}
```

`$row = $results->fetch_assoc()`从`$results`获取数据行，并将其存储在`$row`变量中。我们只是去查看数据，看看它给了我们什么。

```
**array** *(size=6)*
  'id' => string '1' *(length=1)*
  'first_name' => string 'Dino' *(length=4)*
  'last_name' => string 'Cajic' *(length=5)*
  'email' => string 'dino@example.com' *(length=16)*
  'updated_at' => string '2022-11-03 21:56:16' *(length=19)*
  'created_at' => string '2022-11-03 21:56:16' *(length=19)*
```

它返回给我们一个关联数组。这意味着我们可以使用这些键中的每一个来回显特定的数据。

```
while( $row = $results->fetch_assoc() ) 
{
    echo $row['first_name'];
}
```

我们现在要做的就是添加一个检查来确保我们确实收到了一些数据。

```
if ( $results->num_rows > 0 ) 
{
    while( $row = $results->fetch_assoc() ) {
        var_dump($row);
    }
}
```

`$results->num_rows`返回查询从表中提取的行数。我们检查行数是否大于 0。如果是，遍历每个关联数组并对数据做一些事情。

# WHERE 子句

我们不必每次都返回完整的列表。我们可以返回一个特定的作者，因为每个作者都有一个特定的用户`id`。只要我们知道作者的 id，我们就可以检索它。

```
SELECT * FROM authors WHERE id = '1';
```

我们仍然想要检索作者的所有字段，比如`first_name`和`last_name`，但是我们只想要一个作者。在这个例子中，作者的`id`是 1。

还有什么需要改变的吗？不，即使你可以。可以保留`while`循环，也可以只调用`while`循环外的`$row = $results->fetch_assoc()`。

```
$sql = "SELECT * FROM authors WHERE id = 1";
$results = $mysqli->query( $sql );

if ( $results->num_rows > 0 ) 
{
    $row = $results->fetch_assoc();
    var_dump($row);
} 
```

# 对作者类的修改

我们有一门`Author`课。让我们考虑一下我们可能需要的选择方法。基于一个`id`返回一个特定的作者，并返回所有作者。

## 全选

有几种方法可以做到这一点。我们可以选择所有的项目，将它们存储在一个数组中，然后返回该数组。

当然，有一种更简单的方法可以做到这一点。毕竟，将结果作为数组返回是很常见的做法。我们可以用`fetch_all`方法做到这一点。

```
$results->fetch_all(*MYSQLI_ASSOC*);
```

可以传递一个参数，比如`MYSQLI_ASSOC`，也可以不传递。不同的是，如果你不这样做，你将得不到任何钥匙。

```
**$results->fetch_all(*MYSQLI_ASSOC*);****array** *(size=7)*
  0 => 
    **array** *(size=6)*
      'id' => string '1' *(length=1)*
      'first_name' => string 'Dino' *(length=4)*
      'last_name' => string 'Cajic' *(length=5)*
      'email' => string 'dino@example.com' *(length=16)*
      'updated_at' => string '2022-11-03 21:56:16' *(length=19)*
      'created_at' => string '2022-11-03 21:56:16' *(length=19)*
  1 =>... **$results->fetch_all();****array** *(size=7)*
  0 => 
    **array** *(size=6)*
      0 => string '1' *(length=1)*
      1 => string 'Dino' *(length=4)*
      2 => string 'Cajic' *(length=5)*
      3 => string 'dino@example.com' *(length=16)*
      4 => string '2022-11-03 21:56:16' *(length=19)*
      5 => string '2022-11-03 21:56:16' *(length=19)*
  1 =>
```

## 选择作者

我们将把`id`传递给我们的`select`语句并返回结果。我们可以保持它与`select_all`完全相同，并返回`$results->fetch_all`关联数组，但这没有意义。我们只需要一行，所以我们将返回一行数据。

如果没有数据，将返回`null`。

# 完整代码

就是这样。让我们看看现在我们的代码是什么样的。

`DB`没有变化。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/a25979c19219b2d8673c967efa53efa1.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 负责人，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。