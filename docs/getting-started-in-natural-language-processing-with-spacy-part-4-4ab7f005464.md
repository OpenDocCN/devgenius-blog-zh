# spaCy 自然语言处理入门:第 4 部分

> 原文：<https://blog.devgenius.io/getting-started-in-natural-language-processing-with-spacy-part-4-4ab7f005464?source=collection_archive---------21----------------------->

在这个故事中，我们将着重于词汇化和停用词

与词干化相反，词汇化看起来不仅仅是单词缩减，而是考虑一种语言的全部词汇来对单词进行形态分析。“was”的引理是“be”，“mice”的引理是“mouse”。此外,“meeting”的引理可能是“meet”或“meeting ”,这取决于它在句子中的用法。

![](img/a3ee50418fc6cd06fea40cfd88b930b4.png)

图片来自 Spacy.io

```
*# Perform standard imports:* **import** spacy
nlp **=** spacy.load('en_core_web_sm')
In [2]:
doc1 **=** nlp(u"I am a runner running in a race because I love to run since I ran today")
**for** token **in** doc1:
   print(token.text, '\t', token.pos_, '\t', token.lemma, '\t', token.lemma_)
I 	 PRON 	 561228191312463089 	 -PRON-
am 	 VERB 	 10382539506755952630 	 be
a 	 DET 	 11901859001352538922 	 a
runner 	 NOUN 	 12640964157389618806 	 runner
running 	 VERB 	 12767647472892411841 	 run
in 	 ADP 	 3002984154512732771 	 in
a 	 DET 	 11901859001352538922 	 a
race 	 NOUN 	 8048469955494714898 	 race
because 	 ADP 	 16950148841647037698 	 because
I 	 PRON 	 561228191312463089 	 -PRON-
love 	 VERB 	 3702023516439754181 	 love
to 	 PART 	 3791531372978436496 	 to
run 	 VERB 	 12767647472892411841 	 run
since 	 ADP 	 10066841407251338481 	 since
I 	 PRON 	 561228191312463089 	 -PRON-
ran 	 VERB 	 12767647472892411841 	 run
today 	 NOUN 	 11042482332948150395 	 today
```

在上面的句子中，`running`、`run`和`ran`都指向同一个引理`run`(...11841)以免重复。

## 显示词条的函数

既然上面的显示是交错的，难以阅读，那我们就写一个函数，把我们想要的信息更整齐的显示出来。

```
In [3]:
**def** show_lemmas(text):
**for** token **in** text:
   print(f'{token.text:{12}} {token.pos_:{6}} {token.lemma:**<**{22}} {token.lemma_}')
```

这里我们使用一个 **f 字符串**通过设置最小字段宽度和添加 lemma 哈希值左对齐来格式化打印文本。

```
In [4]:doc2 **=** nlp(u"I saw eighteen mice today!")
show_lemmas(doc2)I            PRON   561228191312463089     -PRON-
saw          VERB   11925638236994514241   see
eighteen     NUM    9609336664675087640    eighteen
mice         NOUN   1384165645700560590    mouse
today        NOUN   11042482332948150395   today
!            PUNCT  17494803046312582752   !
```

注意，`saw`的引理是`see`，`mice`是`mouse`的复数形式，然而`eighteen`是它自己的数字，*不是*是`eight`的扩展形式。

```
In [5]:doc3 **=** nlp(u"I am meeting him tomorrow at the meeting.")
show_lemmas(doc3)I            PRON   561228191312463089     -PRON-
am           VERB   10382539506755952630   be
meeting      VERB   6880656908171229526    meet
him          PRON   561228191312463089     -PRON-
tomorrow     NOUN   3573583789758258062    tomorrow
at           ADP    11667289587015813222   at
the          DET    7425985699627899538    the
meeting      NOUN   14798207169164081740   meeting
.            PUNCT  12646065887601541794   .
```

这里`meeting`的引理是由其词性标签决定的。

```
In [6]:doc4 **=** nlp(u"That's an enormous automobile")
show_lemmas(doc4)That         DET    4380130941430378203    that
's           VERB   10382539506755952630   be
an           DET    15099054000809333061   an
enormous     ADJ    17917224542039855524   enormous
automobile   NOUN   7211811266693931283    automobile
```

注意，词汇化不会将单词简化为最基本的同义词，也就是说，`enormous`不会变成`big`,`automobile`不会变成`car`。

我们应该指出，虽然词汇化查看周围的文本来确定一个给定单词的词性，但它不对短语进行分类。在接下来的讲座中，我们将研究*单词向量和相似度*。

## 停止言语

像“a”和“the”这样的词出现得如此频繁，以至于它们不像名词、动词和修饰语那样需要彻底标记。我们把这些*停用词叫做*，它们可以从待处理的文本中筛选出来。spaCy 拥有一个大约 305 个英语停用词的内置列表。

```
In [1]:*# Perform standard imports:* **import** spacy
nlp **=** spacy.load('en_core_web_sm')In [2]:*# Print the set of spaCy's default stop words (remember that sets are unordered):* print(nlp.Defaults.stop_words){'hers', 'show', 'though', 'various', 'sixty', 'say', 'quite', 'ten', 'anything', 'although', 'hereby', 'in', 'ours', 'herself', 'among', 'unless', 'and', 'whole', 'anywhere', 'latter', 'therein', 'whereafter', 'that', 'one', 'whose', 'either', 'within', 'eight', 'three', 'latterly', 'anyone', 'a', 'less', 'former', 'been', 'same', 'anyway', 'else', 'cannot', 'five', 'i', 'until', 'last', 'thus', 'give', 'move', 'thereafter', 'via', 'than', 'empty', 'off', 'neither', 'too', 'please', 'over', 'just', 'otherwise', 'has', 'her', 'put', 'its', 'whether', 'herein', 'myself', 'me', 'nevertheless', 'whatever', 'someone', 'towards', 'whereby', 'onto', 'sometimes', 'thence', 'them', 'done', 'at', 'back', 'nor', 'another', 'behind', 'together', 'take', 'amongst', 'being', 'seemed', 'seeming', 'fifteen', 'do', 'further', 'something', 'again', 'this', 'were', 'wherein', 'how', 'up', 'must', 'get', 'whereas', 'much', 'upon', 'yet', 'both', 'many', 'very', 'may', 'after', 'regarding', 'full', 'through', 'below', 'his', 'well', 'everything', 'so', 'our', 'should', 'seem', 'while', 'for', 'might', 'mine', 'when', 'with', 'you', 'few', 'never', 'because', 'own', 'also', 'due', 'hence', 'it', 'more', 'their', 'such', 'becomes', 'first', 'hereupon', 'since', 'third', 'twenty', 'who', 'she', 'nobody', 'name', 'really', 'enough', 'least', 'two', 'whoever', 'which', 'yours', 'moreover', 'seems', 'before', 'therefore', 'then', 'used', 'even', 'nowhere', 'without', 'other', 'around', 'made', 'hundred', 'no', 'twelve', 'several', 'your', 'meanwhile', 'per', 'except', 'yourselves', 'why', 'some', 'not', 'yourself', 'sometime', 'somehow', 'become', 'beyond', 'almost', 'will', 'somewhere', 'the', 'everyone', 'about', 'everywhere', 'anyhow', 'side', 'next', 'fifty', 'they', 'most', 'perhaps', 'across', 'themselves', 'besides', 'against', 'can', 'him', 'there', 'noone', 'under', 'formerly', 'already', 'all', 'if', 'my', 'or', 'serious', 'four', 'thereupon', 'whence', 'here', 'whither', 'beside', 'wherever', 'to', 'himself', 'between', 'ourselves', 'none', 'on', 'became', 'an', 'have', 'part', 'did', 'had', 'each', 'six', 'those', 'from', 'whenever', 'any', 'am', 'would', 'make', 'could', 'does', 'go', 'call', 'indeed', 'these', 'often', 'above', 'during', 'by', 'nine', 'thereby', 'others', 'afterwards', 'throughout', 'whom', 'amount', 'as', 'hereafter', 'top', 'mostly', 'us', 'whereupon', 'once', 'only', 'still', 'namely', 'forty', 'ca', 'along', 'be', 'itself', 'where', 'see', 'into', 'toward', 'but', 'is', 'keep', 'bottom', 'ever', 'becoming', 'every', 'always', 'front', 'nothing', 'we', 'of', 'out', 'eleven', 'alone', 'he', 'however', 'rather', 'down', 'thru', 'now', 'using', 'are', 'doing', 'what', 'beforehand', 're', 'was', 'elsewhere'}In [3]:len(nlp.Defaults.stop_words)
Out[3]:305
```

## 查看一个单词是否是停用词

```
In [4]:nlp.vocab['myself'].is_stop
Out[4]:True
In [5]:nlp.vocab['mystery'].is_stop
Out[5]:False
```

## 添加停用词

有时，您可能希望在默认集合中添加停用字词。也许你决定`'btw'`(“顺便说一下”的常用简写)应该被认为是一个停用词。

```
In [6]:*# Add the word to the set of stop words. Use lowercase!* nlp.Defaults.stop_words.add('btw')
*# Set the stop_word tag on the lexeme* nlp.vocab['btw'].is_stop **=** **True** In [7]:len(nlp.Defaults.stop_words)
Out[7]:306
In [8]:nlp.vocab['btw'].is_stop
Out[8]:True
```

添加停用词时，请始终使用小写。在添加到 **vocab** 之前，词位被转换成小写。

## 删除停用词

或者，您可以决定`'beyond'`不应被视为停用词。

```
In [9]:*# Remove the word from the set of stop words* nlp.Defaults.stop_words.remove('beyond')
*# Remove the stop_word tag from the lexeme* nlp.vocab['beyond'].is_stop **=** **False** In [10]:len(nlp.Defaults.stop_words)
Out[10]:305
In [11]:nlp.vocab['beyond'].is_stop
Out[11]:False
```

太好了！现在，您应该能够访问 spaCy 的默认停用词集，并根据需要添加或删除停用词。

如果您错过了前面的部分，以下是链接:

[](https://medium.com/dev-genius/getting-started-in-natural-language-processing-with-spacy-part-1-5026748cadc2) [## spaCy 自然语言处理入门:第 1 部分

### spaCy(https://spacy.io/)是一个开源的 Python 库，可以解析和“理解”大量的文本…

medium.com](https://medium.com/dev-genius/getting-started-in-natural-language-processing-with-spacy-part-1-5026748cadc2) [](https://medium.com/@santanudutta85/getting-started-in-natural-language-processing-with-spacy-part-2-73ecb4eac11d) [## spaCy 自然语言处理入门:第 2 部分

### 如果您错过了第 1 部分。如果您正在寻找基本安装、基本命令、令牌化，请查看它

medium.com](https://medium.com/@santanudutta85/getting-started-in-natural-language-processing-with-spacy-part-2-73ecb4eac11d) [](https://medium.com/@santanudutta85/getting-started-in-natural-language-processing-with-spacy-part-3-824c1b291d22) [## spaCy 自然语言处理入门:第 3 部分

### 在这个故事中，我们将重点关注词干。

medium.com](https://medium.com/@santanudutta85/getting-started-in-natural-language-processing-with-spacy-part-3-824c1b291d22) [](https://medium.com/@santanudutta85/getting-started-in-natural-language-processing-with-spacy-part-4-287975381254) [## spaCy 自然语言处理入门:第 5 部分

### 在这个故事中，我们将关注词汇和搭配。

medium.com](https://medium.com/@santanudutta85/getting-started-in-natural-language-processing-with-spacy-part-4-287975381254)