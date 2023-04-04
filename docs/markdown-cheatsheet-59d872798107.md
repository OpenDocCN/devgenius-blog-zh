# 减价备忘单

> 原文：<https://blog.devgenius.io/markdown-cheatsheet-59d872798107?source=collection_archive---------12----------------------->

Markdown 是一种轻量级且易于使用的语法，用于设计所有形式的写作。

![](img/36397d3d8cd04a562a0055af894eff62.png)

照片由[拍下](https://unsplash.com/@burst?utm_source=medium&utm_medium=referral)在[上爆裂](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是降价？

Markdown 是一种设计网页文本风格的方法。您控制文档的显示；将单词格式化为粗体或斜体、添加图像和创建列表只是我们可以用 Markdown 做的一些事情。大多数情况下，Markdown 只是普通的文本，带有一些非字母字符，如#或*。

Markdown 在 Github 上写 README 的很流行。所以，我给了你一个降价的教程。

# 语法指南

你可以在这里找到关于 Markdown 的 syntex 的文档和完整解释。

# 内容

*   标题
*   强调
*   大胆的
*   意大利语族的
*   删除线
*   形象
*   链接
*   段落
*   大宗报价
*   列表
*   有序列表
*   无序列表
*   密码
*   语法突出显示或多行代码
*   桌子
*   横向规则

# 标题

您可以通过添加前缀`#`(散列符号)来创建标题。

## 标题 1

```
Syntex: 
    # Heading 1
        or
    =====
```

## 标题 2

```
Syntex:
    ## Heading 2
        or
    ----
```

## 标题 3

```
Syntex:
    ### Heading 3
```

## 标题 4

```
Syntex:
    #### Heading 4
```

## 标题 5

```
Syntex:
    ##### Heading 5
```

## 标题 6

```
Syntex:
    ###### Heading 6
```

# 强调

## 大胆的

您可以通过添加双星号(*)或双下划线(_)将任何文本加粗。

```
Syntex:

    **Bold**

    __Bold__
```

## 意大利语族的

您可以通过添加星号(*)或下划线(_)将任何文本变为斜体。

```
Syntex: *Italic* _Italic_
```

## 粗体和斜体

甚至你可以把两者结合起来。

```
Syntex:
    You **can** combine them_
```

## 删除线

Markdown 通过在`~~`中换行来支持删除线:

```
Syntex:
    ~~This text is strikethrough.~~
```

# 形象

```
Syntex:

![Alt text](https://github.com/dcurtis/markdown-mark/blob/master/png/208x128-solid.png)
```

# 链接

Markdown 支持内嵌链接和引用链接。

## 内嵌链接

要创建内联链接，请使用以下语法:

```
[ Text for the link ](URL)
```

## 参考链接

参考链接使用方括号代替参数。

```
Syntex:
    This is [reference][reference-link] link example.
```

# 段落

段落是一行或多行连续的文本，由一个或多个。

```
Syntex:
    This is paragramph one. This is paragraph two.
```

# 大宗报价

Markdown 使用>(大于)字符进行块引用。

```
Syntex:
    > This is quote.
```

# 列表

Markdown 支持有序(编号)和无序(项目符号)列表。

## 有序列表

有序列表要求数字字符后跟一个.(句点)。

```
Syntex:
	1\. Item one
	2\. Item two 
	3\. Item three
```

## 无序列表

使用*(星号)、+(加号)或-(破折号)中的任何一个来形成项目符号列表。您可以选择其中一项或多项，或混合使用，以形成一个列表:

```
Syntex:
    * Item one 
    + Item two
    - Item three
```

列表可以嵌入列表中。

```
Syntex:
    * Item one
    + Item two
        * Sub item one
        * Sub item two	
    - Item three
        1\. Sub item one
	        1\. sub sub item 1 
	        2\. sub sub item 2
        2\. Sub item two 
            * sub sub one
	        * sub sub two
        3\. Sub item three
```

# 密码

为了表示代码的跨度，用```(反勾)把它包起来。

```
Syntex:
    `print("This is Markdown demo.")`
```

# 语法突出显示或多行代码

为了指示多行代码垃圾，用`````(三个反勾)将代码换行

```
Syntex:
    ```
        Code line 1
        Code line 2
        Code line 3
    ```
```

# 桌子

在 Markdown 中，您可以使用-(破折号)和|(竖线)符号创建表格。第一行包含列标题。用管道符号分隔列。第二行必须是标题和内容之间的强制分隔线。后续行是表格行。列始终由竖线(|)字符分隔。

```
Syntex: Header One  | Header Two
    ------------- | -------------
    Cell Content  | Cell Content
    Cell Content  | Cell Content
```

# 横向规则

您可以使用以下任何代码创建 horizontol 规则:

```
Syntex:
    * * *
    ***
	- - - 
	---
```

如果你喜欢我的工作并想支持我，我会非常感谢你在我的社交媒体频道上关注我:

*   支持我的最好方式就是跟随我上[](https://medium.com/@themlphdstudent)**。**
*   **订阅我的新[YouTube 频道 。](https://www.youtube.com/c/themlphdstudent)**
*   **在我的 [**邮箱列表**](http://eepurl.com/hampwT) 报名。**

**以防你错过我的 python 系列。**

*   **第 0 天:[挑战介绍](/@durgeshsamariya/100-days-of-machine-learning-code-a9074e1c42c3)**
*   **第 1 天: [Python 基础知识— 1](/the-innovation/python-basics-variables-data-types-and-list-59cea3dfe10f)**
*   **第 2 天: [Python 基础知识— 2](https://towardsdatascience.com/python-basics-2-working-with-list-tuples-dictionaries-871c6c01bb51)**
*   **第 3 天: [Python 基础知识— 3](/towards-artificial-intelligence/python-basics-3-97a8e69066e7)**
*   **第 4 天: [Python 基础— 4](https://medium.com/javarevisited/python-basics-4-functions-working-with-functions-classes-working-with-class-inheritance-70e0338c1b2e)**

**看看我的其他文章。**

*   **[八月机器学习阅读清单](/@durgeshsamariya/august-2020-monthly-machine-learning-reading-list-by-durgesh-samariya-20028aa1d5cc)**
*   **[集群资源](/towards-artificial-intelligence/a-curated-list-of-clustering-resources-fe355e0e058e)**