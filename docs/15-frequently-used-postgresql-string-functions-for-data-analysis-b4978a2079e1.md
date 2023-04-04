# 用于数据分析的 15 个常用 PostgreSQL 字符串函数

> 原文：<https://blog.devgenius.io/15-frequently-used-postgresql-string-functions-for-data-analysis-b4978a2079e1?source=collection_archive---------18----------------------->

如何在您的数据分析项目中使用 PostgreSQL 字符串函数

![](img/b76cec5209b9daf122269119d7862210.png)

照片由老虎百合(Pexel)拍摄

由于数据存储方式的不一致，在数据库中处理文本数据可能会很有挑战性。

这篇博文解释了如何使用 PostgreSQL 中常用的二十个字符串函数来解决处理文本数据时遇到的一些挑战

1.  **串联**

函数的作用是:将两个或更多的字符串连接成一个。

![](img/e66688726a9dda6c04f14904fde70e08.png)

hr _ 数据表

上表显示了 hr_data 中的前五行。我们将用 CONCAT 函数将名和姓连接起来作为全名。

连接名字和姓氏的 select 语句:

![](img/bf5e51c49295f95250a5e0559c69bd6d.png)

**2。INITCAP**

INITCAP()函数将每个单词的第一个字母转换成大写，其余的转换成小写。

CONCAT 函数的结果显示，某些名称的大小写不一致。当生成一个报告时，INITCAP 可以用格式名作为专有名词。

将一个单词的第一个字母转换成大写字母，其余字母转换成小写字母的 select 语句:

![](img/0d583bbb9b54d460811d46f57ca5e6d1.png)

**3。上部**

UPPER()函数将字符串转换成大写字母。
将字符串转换成大写的 select 语句:

![](img/fe87c75f537ad3813d80da7496e2db51.png)

**4。降低**

LOWER()函数将字符串从大写转换成小写。

将字符串转换为小写的 select 语句:

![](img/0f778795b2c2fb7f13f4148ec4078258.png)

**5。CHAR_LENGTH**

CHAR_LENGTH 函数计算指定字符串中的字符数。它通常与其他函数一起用作 POSITION，从给定的字符串中提取指定数量的字符。

计算字符串字符数的 select 语句:

![](img/b37fd6e8b96f4ca82bb09afe86b51cd5.png)

**6。位置**

POSITION 函数在指定的字符串中查找子字符串的位置。

select 语句查找名和姓之间的空格:

![](img/20e065b6866024bbb8cf722d313793da.png)

**7。左侧**

LEFT()函数从给定字符串的左边提取 select 语句中指定的“n”个字符。使用 LEFT 和 POSITION 函数只能从包含全名的列中提取名字。

![](img/9379062094405fc6856d7d2d8075682b.png)

上表显示了“客户”表中的前五行。我们可以通过组合 LEFT 和 POSITION 函数来去掉 first_name 和 last_name。

从全名中删除名字的 select 语句:

![](img/4676670e666d7a75b273a0758c13908a.png)

**8。右**

RIGHT()函数从给定字符串的右边提取 select 语句中指定的“n”个字符。

可以使用 RIGHT、CHAR_LENGTH 和 POSITION 函数从全名中提取姓氏，如下面的 select 语句所示。

![](img/028189c83a22eee2f13f34d067be8c63.png)

**9。子串**

SUBSTRING 函数从给定字符串中的特定位置提取特定数量的字符。SUBSTRING 函数比 LEFT 和 RIGHT 函数灵活得多，因为它允许您从字符串的任何部分提取字符。

您可以使用下面的 select 语句以及 SUBSTRING 和 POSITION 函数从全名中提取姓氏。

![](img/436b29d63ad466ccfa2fd3caeb560ebc.png)

10。更换

REPLACE 函数搜索并使用新字符串替换指定字符串的所有匹配项。

select 语句将部门列中的销售替换为营销:

![](img/3ca52fd06144c5b2b09f32522e5a5acd.png)

**11。反向**

REVERSE 函数反转字符串中的字符。

反转字符串字符的 select 语句:

![](img/6c42950a4fe1070a7f78f081430c6263.png)

**12。分割 _ 零件**

SPLIT_PART 函数根据分隔符分割给定的字符串，并从左边开始从指定的字符串中挑选出所需的部分。

```
**Syntax:** SPLIT_PART(string, delimiter, position) 
```

将一列拆分为三列的 select 语句:

![](img/41937fd07f15c237c035f62c34991933.png)

13。修剪

TRIM 函数从字符串的开头、结尾或两边删除空格或字符集。

**语法:**

```
TRIM([leading|trailing|both] <removing_string> from <main_string>)
```

语法中的前导、尾随或两者都指示将移除 removing _ string 的 main_string 的位置。默认值为 both。

TRIM()函数对于处理列中字符串空格的不一致非常有用。

删除字符“0123456789 #”的 select 语句从字符串:

![](img/7acdee61d805eb3cf770546f3477d697.png)

LTRIM()函数只删除字符串中的前导字符，而 RTRIM()函数也删除字符串中的尾随字符。

**14。LPAD**

函数的作用是:从左边开始用一个子串填充一个特定长度的字符串。

在下面的 Select 语句中，通过从左侧填充字符“01-”将 7 个字符的 id 列扩展为 10 个字符的 id 列。

![](img/51ab2927c52ff7949f8e606e5229ee57.png)

**15。RPAD**

函数的作用是:从右边开始用一个子串填充一个特定长度的字符串。

在下面的 Select 语句中，通过从右侧填充字符“-01”，7 个字符的 id 列被扩展为 10 个字符的 id 列。

![](img/321b3d947ef3ee13c3a67bf9cf6ab769.png)

**结论**

这篇博文简化了 Postgresql 字符串函数在数据分析中的使用。

您可以从我的 [GitHub](https://github.com/amos-adewuni/freq_used_sql_string_func) 资源库下载数据集和 SQL 脚本供您练习。

关注我的 Twitter 以获取更多数据相关信息。