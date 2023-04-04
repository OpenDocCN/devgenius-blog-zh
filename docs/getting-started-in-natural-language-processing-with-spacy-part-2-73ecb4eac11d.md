# spaCy 自然语言处理入门:第 2 部分

> 原文：<https://blog.devgenius.io/getting-started-in-natural-language-processing-with-spacy-part-2-73ecb4eac11d?source=collection_archive---------17----------------------->

如果您错过了第 1 部分。如果你需要一些关于基本安装、基本命令、标记化、词性标注、依赖性、标记属性、跨度和句子的基本知识，可以看看它。

[](https://medium.com/dev-genius/getting-started-in-natural-language-processing-with-spacy-part-1-5026748cadc2) [## spaCy 自然语言处理入门:第 1 部分

### spaCy(https://spacy.io/)是一个开源的 Python 库，可以解析和“理解”大量的文本…

medium.com](https://medium.com/dev-genius/getting-started-in-natural-language-processing-with-spacy-part-1-5026748cadc2) ![](img/a3ee50418fc6cd06fea40cfd88b930b4.png)

## 标记化

创建“Doc”对象的第一步是将输入文本分解成组成部分或“标记”。

```
In [1]:*# Import spaCy and load the language library* **import** spacy
nlp **=** spacy.load('en_core_web_sm')
In [2]:
*# Create a string that includes opening and closing quotation marks* mystring **=** '"We\'re moving to L.A.!"'
print(mystring)
"We're moving to L.A.!"
In [3]:
*# Create a Doc object and explore tokens* doc **=** nlp(mystring)
**for** token **in** doc:
   print(token.text, end**=**' | ')
" | We | 're | moving | to | L.A. | ! | " |
```

*   **前缀**:开头的字符`$ ( “ ¿`
*   **后缀**:末尾字符`km ) , . ! ”`
*   **中缀**:中间的字符`- -- / ...`
*   **异常**:特殊情况规则，当应用标点规则时，将一个字符串分割成几个记号或防止一个记号被分割

请注意，标记是原始文本的一部分。也就是说，我们没有看到任何词干或词条(单词的基本形式)的转换，我们也没有看到任何关于组织/地点/金钱等的内容。标记是 Doc 对象的基本构建块——帮助我们理解文本含义的一切都是从标记及其相互关系中派生出来的。

## 前缀、后缀和中缀

spaCy 将隔离不构成单词组成部分的标点符号。句末的引号、逗号和标点符号将被赋予各自的符号。但是，作为电子邮件地址、网站或数值的一部分存在的标点符号将作为令牌的一部分保留。

```
In [4]:doc2 **=** nlp(u"We're here to help! Send snail-mail, email support@oursite.com or visit us at http://www.oursite.com!")
**for** t **in** doc2:
   print(t)
We
're
here
to
help
!
Send
snail
-
mail
,
email
support@oursite.com
or
visit
us
at
[http://www.oursite.com](http://www.oursite.com/)
!
```

请注意,“蜗牛邮件”中的感叹号、逗号和连字符被赋予了自己的标记，但电子邮件地址和网站都被保留了下来。

```
In [5]:doc3 **=** nlp(u'A 5km NYC cab ride costs $10.30')
**for** t **in** doc3:
   print(t)
A
5
km
NYC
cab
ride
costs
$
10.30
```

在这里，距离单位和美元符号被赋予它们自己的记号，但是美元的数量被保留。

## 例外

作为已知缩写的一部分存在的标点符号将作为标记的一部分保留。

```
In [6]:doc4 **=** nlp(u"Let's visit St. Louis in the U.S. next year.")
**for** t **in** doc4:
   print(t)
Let
's
visit
St.
Louis
in
the
U.S.
next
year
.
```

在这里,“圣”和“美国”的缩写都保留了下来。

## 清点代币

`Doc`物体有一定数量的记号:

```
In [7]:len(doc)
Out[7]:
8
```

## 统计 Vocab 条目

`Vocab`对象包含一个完整的项目库！

```
In [8]:len(doc.vocab)
Out[8]:
57852
```

注意:这个数字根据开始时加载的语言库和创建`Doc`时引入到`vocab`中的任何新词位而变化。

## 令牌可以通过索引位置和切片来检索

`Doc`对象可以被认为是`token`对象的列表。因此，可以通过索引位置来检索各个标记，并且可以通过切片来检索标记的范围:

```
In [9]:doc5 **=** nlp(u'It is better to give than to receive.')
*# Retrieve the third token:* doc5[2]
Out[9]:
better
In [10]:
*# Retrieve three tokens from the middle:* doc5[2:5]
Out[10]:
better to give
In [11]:
*# Retrieve the last four tokens:* doc5[**-**4:]
Out[11]:than to receive.
```

## 令牌不能重新分配

虽然`Doc`对象可以被认为是令牌列表，但是它们不*也不*支持项目重分配:

```
In [12]:doc6 **=** nlp(u'My dinner was horrible.')
doc7 **=** nlp(u'Your dinner was delicious.')
In [13]:
*# Try to change "My dinner was horrible" to "My dinner was delicious"* doc6[3] **=** doc7[3]
**---------------------------------------------------------------------------**
**TypeError**                                 Traceback (most recent call last)
**<ipython-input-13-d4fb8c39c40b>** in <module>**()**
      1 **# Try to change "My dinner was horrible" to "My dinner was delicious"**
**----> 2** doc6**[3]** **=** doc7**[3]****TypeError**: 'spacy.tokens.doc.Doc' object does not support item assignment
```

## 命名实体

*命名实体*比令牌更进了一步，增加了另一层上下文。语言模型认为某些单词是组织名称，而其他单词是位置，还有一些组合与金钱、日期等有关。命名实体可以通过一个`Doc`对象的`ents`属性来访问。

```
In [14]:doc8 **=** nlp(u'Apple to build a Hong Kong factory for $6 million')
**for** token **in** doc8:
   print(token.text, end**=**' | ')
   print('\n----')
   **for** ent **in** doc8.ents:
      print(ent.text**+**' - '**+**ent.label_**+**' -**+**str(spacy.explain(ent.label_)))
Apple | to | build | a | Hong | Kong | factory | for | $ | 6 | million | 
----
Apple - ORG - Companies, agencies, institutions, etc.
Hong Kong - GPE - Countries, cities, states
$6 million - MONEY - Monetary values, including unit
```

注意两个令牌如何组合形成实体`Hong Kong`，三个令牌如何组合形成货币实体:`$6 million`

```
In [15]:len(doc8.ents)
Out[15]:3
```

命名实体识别(NER)是一种应用于自然语言处理的重要机器学习工具。
我们将在接下来的章节中做更多的工作。有关**命名实体**的更多信息，请访问[https://spacy.io/usage/linguistic-features#named-entities](https://spacy.io/usage/linguistic-features#named-entities)

## 名词块

与`Doc.ents`类似，`Doc.noun_chunks`是另一个对象属性。名词块是“基本名词短语”——以一个名词为中心的扁平短语。你可以把名词块想象成一个名词加上描述这个名词的单词——例如，在[谢布·伍利 1958 年的歌曲](https://en.wikipedia.org/wiki/The_Purple_People_Eater)中，一个*“独眼独角，飞翔，紫色的食人族”*就是一个长名词块。

```
In [16]:doc9 **=** nlp(u"Autonomous cars shift insurance liability toward manufacturers.")
**for** chunk **in** doc9.noun_chunks:
   print(chunk.text)
Autonomous cars
insurance liability
manufacturersIn [17]:
doc10 **=** nlp(u"Red cars do not carry higher insurance rates.")
**for** chunk **in** doc10.noun_chunks:
   print(chunk.text)
Red cars
higher insurance rates
In [18]:
doc11 **=** nlp(u"He was a one-eyed, one-horned, flying, purple people-eater.")
**for** chunk **in** doc11.noun_chunks:
   print(chunk.text)
He
a one-eyed, one-horned, flying, purple people-eater
```

在下一节中，除了`.text`之外，我们还会看到其他的 noun_chunks 组件。欲了解更多关于名词组块的信息，请访问 https://spacy.io/usage/linguistic-features#noun-chunks

## 内置可视化工具

spaCy 包括一个名为 **displaCy** 的内置可视化工具。displaCy 能够检测你是否在一个 Jupyter 笔记本中工作，并将返回可以立即在单元格中呈现的标记。导出笔记本时，可视化效果将以 HTML 格式包含在内。欲了解更多信息，请访问:

[](https://spacy.io/usage/visualizers) [## 可视化工具空间使用文档

### 在浏览器或笔记本中可视化依赖关系和实体

空间. io](https://spacy.io/usage/visualizers) 

## 可视化依赖解析

运行下面的单元格以导入 displacy 并显示相关性图形

```
In [19]:**from** spacy **import** displacy
doc **=** nlp(u'Apple is going to build a U.K. factory for $6 million.')
displacy.render(doc, style**=**'dep', jupyter**=True**, options**=**{'distance': 110})
```

Apple PROPN 是动词 go 动词 to PART build 动词 DET 英国 PROPN 工厂名词 ADP $ SYM 6 NUM 百万。NUM nsub aux xcomp det compound dobj prep quantmod compound pobj

可选的`'distance'`参数设置标记之间的距离。如果距离过小，出现在短箭头下方的文本可能会变得过于压缩而无法阅读。

## 可视化实体识别器

```
In [20]:doc **=** nlp(u'Over the last quarter Apple sold nearly 20 thousand iPods for a profit of $6 million.')
displacy.render(doc, style**=**'ent', jupyter**=True**)
```

在上个季度**日**苹果**组织**卖出了近 2 万台**红衣主教**ipod**产品**获利 600 万美元**金钱**。

太好了！现在，您应该了解了标记化是如何将文本划分为单个元素的，命名实体是如何提供上下文的，以及某些工具是如何帮助可视化语法规则和实体标签的。

[](https://medium.com/@santanudutta85/getting-started-in-natural-language-processing-with-spacy-part-3-824c1b291d22) [## spaCy 自然语言处理入门:第 3 部分

### 在这个故事中，我们将重点关注词干。

medium.com](https://medium.com/@santanudutta85/getting-started-in-natural-language-processing-with-spacy-part-3-824c1b291d22)