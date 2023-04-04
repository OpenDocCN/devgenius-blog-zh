# 厌倦了清理文本？为此我做了一个 Python 包。

> 原文：<https://blog.devgenius.io/tired-of-cleaning-texts-ive-made-a-python-package-for-that-9b28e1485a21?source=collection_archive---------9----------------------->

![](img/77c1332d0e267e9b79a69a36cbe46943.png)

形象礼貌[](https://unsplash.com/photos/xhWMi9wdQaE)

**文字可以是**冗长、凌乱、**和真正的群集，天知道是什么。由于 **NLP** 的工作，我几乎每个小时都在处理**文本**数据，这让我明白了一件事，没有一个合适的包可以清理文本数据并快速呈现给你，并且有许多功能，所以我决定**创建一个**来修复(几乎所有的文本清理问题和需求)。**

**[](https://pypi.org/project/cleantxty/) [## cleantxty

### Python 包来清理字符串，并使它们对于 NLP 来说是合理的。cleantxty 是一个开源的 python 包…

pypi.org](https://pypi.org/project/cleantxty/) 

它的一些特点是

*   **清理**:清理原始文本并返回清理后的文本
*   **clean_words** :清理原始文本，返回干净单词列表

可以同时使用的其他方法有:

*   **删除链接**:从文本中删除链接
*   **remove _ extra _ white _ space**:删除文本中多余的空白
*   **lower_text** :使文本的大小写变为小写
*   **upper_text** :将文字变成大写
*   **删除停用词**:从文本中删除停用词
*   **删除数字**:从文本中删除数字
*   **删除标点符号**:删除文本中的标点符号
*   **自定义正则表达式**:使用自定义正则表达式并将其应用于文本
*   **stem_text** :对提供的文本进行词干处理

# 装置

```
pip install cleantxty
```

# **使用库**

。用于过滤文本并返回结果的 clean 函数

```
import cleantxty

cleantxty.clean("this is a sample text for cleaning yay")

#output 

'sampl text clean yay'
```

。clean_words 函数用于过滤文本和返回单词

```
cleantxty.clean_words("this is a sample text for cleaning yay")

#output 

['sampl', 'text', 'clean', 'yay']
```

可以告诉这些函数使用 **default_case** 和 **regex** 来提供更专业的结果

```
cleantxty.clean_words("this is a sample text for cleaning yay",default_case="upper")

#output 

['SAMPL', 'TEXT', 'CLEAN', 'YAY']
```

**自定义正则表达式**在这里工作，

```
cleantxty.clean("this is a sample text for cleaning yay",r"[a-z0–9\.\-+_]+@[a-z0–9\.\-+_]+\.[a-z]+")

#output 

'SAMPL TEXT CLEAN YAY'
```

类似地，一些单独的功能也可以这样使用

```
#remove_link

cleantxty.remove_link("realhttps://pypi.org/project/cleantxty/ this is link it need to be removed ")

#output 

'real this is link it need to be removed '

#remove_digits

cleantxty.remove_digits("t6o remove digits is a m56ess 8929")

#output

't6o remove digits is a m56ess'

#remove_punctuations

cleantxty.remove_punctuations("hello !! , bro yay")

#output

'hello   bro yay'
```

如果你喜欢这篇文章，跟着我，你也可以做下面的事情。

让我们连线 https://www.linkedin.com/in/tripathiadityaprakash 的**LinkedIn**:

或者我的**网站**:

[https://tripathiaditya.netlify.app/](https://tripathiaditya.netlify.app/)**